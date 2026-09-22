# TECHNICAL-SPEC-007: Loading, Validating, And Recovering Saved Data

Spec ID: `TECHNICAL-SPEC-007`

Spec type: `TechnicalSpec`

Implements: [`FEATURE-SPEC-014: Recovering Unreadable Saved Data`](FEATURE-SPEC-014-recovering-unreadable-saved-data.md)

Covers: the version-1 saved-data document, initial loading, whole-data validation, item-level recovery, cleanup, and `Start again` execution.

Status: `Accepted`

## Purpose

Load every usable part of the website's saved information safely, isolate recoverable broken items, and enter whole-data recovery only when the complete document cannot be used reliably.

## Version-1 Stored Document

The stable key is `cash-money-organizer-website-data`.

```ts
type JsonValue =
  | null
  | boolean
  | number
  | string
  | readonly JsonValue[]
  | { readonly [key: string]: JsonValue };

type StoredWebsiteDataV1 = {
  version: 1;
  mainMoneyAmount: string;
  balanceChanges: readonly JsonValue[];
  savings: readonly JsonValue[];
};
```

The root is a non-null, non-array object with exactly these four fields. No temporary UI, failure state, coverage result, needed amount, or setting is stored.

`mainMoneyAmount` is a normalized decimal string from `0.00` through `999999.99`. It must match `^(0|[1-9][0-9]{0,5})\.[0-9]{2}$` and convert to no more than `99_999_999` cents. It has no sign, comma, `$` sign, or unnecessary leading zero.

The exact empty document is:

```ts
const EMPTY_WEBSITE_DATA_V1: StoredWebsiteDataV1 = {
  version: 1,
  mainMoneyAmount: "0.00",
  balanceChanges: [],
  savings: [],
};
```

Create a fresh copy whenever this empty document becomes an active candidate.

## Canonical Encoder

The shared version-1 encoder accepts a candidate only when:

- the root has the exact version-1 shape;
- `mainMoneyAmount` has the required normalized format and converts exactly to `0` through `99_999_999` integer cents;
- every `balanceChanges` item is a valid `StoredBalanceChangeV1` and IDs are unique;
- every `savings` item is either a valid `StoredSavingV1` or the exact unchanged `rawValue` reused from a broken slot in the supplied current loaded state;
- each invalid candidate item matches one current broken slot, each slot is used no more than once, and a reused broken value remains JSON-safe and is not synthesized or edited by the candidate builder; and
- the complete candidate contains no `bigint`, `symbol`, `undefined`, function, non-finite number, cycle, or unsupported object.

The encoder builds a fresh plain object in the field order `version`, `mainMoneyAmount`, `balanceChanges`, and `savings`, then calls `JSON.stringify`. It reports failure to the code requesting the save instead of throwing an error.

For every candidate `Saving` value carried forward from a current broken slot, the encoder must find one unused broken slot whose `rawValue` is the exact same in-memory value according to `Object.is`. An object or array copy that merely contains the same information does not match. One broken slot can authorize no more than one candidate item. If an exact unused match is not found, encoding fails without writing.

Because every retained parsed value is made immutable during loading, reusing the same `rawValue` also proves that its contents were not edited. The encoder does not use deep equality or compare newly serialized text to approve a preserved broken value.

Derived coverage and temporary runtime symbols are rebuilt after loading and never encoded.

## Safe Validation Boundary

Before reading fields from the root or any individual record, use this check:

```ts
function isNonNullNonArrayObject(
  value: unknown,
): value is Record<string, unknown> {
  return typeof value === "object" && value !== null && !Array.isArray(value);
}
```

The root, each `Balance Changes` item, each `Saving` item, and every nested object field must pass this check before any of its fields are read. This explicitly rejects `null` as well as arrays and primitive values.

Every item validator accepts `unknown` and returns a valid or invalid result instead of throwing. Each item check also runs inside its own failure boundary. If checking one item unexpectedly throws, that item is treated as invalid and scanning continues:

- an invalid `Balance Changes` item is removed from the usable list; and
- an invalid `Saving` item becomes one broken slot that retains its original raw value.

A root-level validation failure enters whole-data recovery. A bad individual list item never crashes the complete load or prevents later items from being checked.

## Save Coordination Contract

TechnicalSpec 007 requires the TechnicalSpec 009 save coordinator to use these results:

```ts
type StoredValueBaseline =
  | { kind: "known"; storedValue: string | null }
  | { kind: "unknown" };

type CoordinatedSaveResult =
  | { status: "saved"; serialized: string }
  | { status: "save-failed" }
  | { status: "superseded"; latestStoredValue: string | null };
```

A known string is the exact text read from the stable storage key. A known `null` means the key did not exist. `unknown` means the initial storage read failed before producing either result.

`saved` carries the exact text successfully written. `save-failed` means no successful write occurred and the existing baseline stays unchanged. `superseded` means the request was based on an older value; its candidate is discarded, and `latestStoredValue` carries the exact newer string or `null` that must be processed through the normal loading rules.

These values exist only in memory and are never written inside `StoredWebsiteDataV1`. TechnicalSpec 009 owns the coordinator implementation and must preserve this contract.

## Load State

```ts
type WebsiteLoadState =
  | { kind: "loading" }
  | {
      kind: "ready";
      storageBaseline: StoredValueBaseline;
      document: StoredWebsiteDataV1;
      mainMoneyAmountCents: number;
      balanceChanges: readonly StoredBalanceChangeV1[];
      savingSlots: readonly LoadedSavingSlot[];
      balanceCleanupPending: boolean;
    }
  | {
      kind: "recovery";
      storageBaseline: StoredValueBaseline;
      confirming: boolean;
      saveFailureVisible: boolean;
    };
```

`loading` is the initial state. It remains active until the initial process either determines that whole-data recovery is required or finishes the browser-storage read, root validation, both item-recovery passes, and any required single silent cleanup attempt. While `loading` is active, no ready or recovery result is exposed, so neither the normal dashboard nor the saved-data recovery state can be shown. In particular, the default `0.00$` dashboard cannot appear before the complete loading process finishes.

Only after that process finishes may `loading` be replaced with one `ready` or `recovery` result. If newer stored information supersedes a cleanup attempt, remain in `loading` while restarting the complete process from that newer information. TechnicalSpec 011 owns how the neutral loading state is presented.

`StoredValueBaseline` is used by TechnicalSpec 006 for safe coordinated writes.

A ready state always has a known baseline. Whether a stored document exists is derived instead of stored separately: a known `null` baseline means no stored document exists, and a known serialized string means one exists. An unknown baseline is allowed only in recovery.

`document` is the one canonical usable document for a ready state. `mainMoneyAmountCents`, `balanceChanges`, and `savingSlots` must all be derived from that same document during one load. They must never be combined with a baseline or document from a different load.

## Initial Read And Whole-Data Validation

Enter `loading` before reading or decoding any browser-storage value. Do not create or expose the default empty ready state before the read finishes.

Read the stable storage key inside a failure boundary.

- If `getItem` returns `null`, retain a known `null` storage baseline and create an in-memory ready state from a fresh empty document. Do not write browser storage merely because the website opened.
- If storage access throws before returning a value, retain an unknown storage baseline and enter recovery.
- If a string exists, retain that exact string as the known storage baseline before parsing it with `JSON.parse`. Parsing failure enters recovery while keeping that known unreadable baseline.
- A parsed root that is `null`, an array, a primitive, or a non-array object without exactly the four required fields enters recovery. No root field is read until the value passes `isNonNullNonArrayObject`.
- A version other than the number `1` enters recovery.
- An invalid `mainMoneyAmount`, non-array `balanceChanges`, or non-array `savings` value enters recovery.

The main money amount is parsed from its string parts into exact integer cents without decimal floating-point arithmetic. Examples such as `5`, `5.0`, `005.00`, `5,000.00`, `5.000`, negative values, and values above `999999.99` enter recovery.

Whole-data recovery exposes none of the partially decoded information to the normal dashboard.

After successful parsing, apply `Object.freeze` recursively to every parsed array and non-null object before retaining records or building loaded slots. Run the complete freezing step inside the root-level failure boundary. If it throws or cannot finish, keep the stored-value baseline unchanged and enter whole-data recovery instead of exposing partial information or allowing the error to crash loading. Parsed values are never mutated; cleanup and later changes always build fresh arrays and objects.

## Balance Changes Item Recovery

Capture one `currentTimeMs` for the complete load. Scan `balanceChanges` in saved-list order.

For each item:

1. validate the unknown value inside its own failure boundary using the accepted `StoredBalanceChangeV1` rules;
2. remove it from the usable list when validation fails;
3. remove it when `currentTimeMs` is equal to or later than `visibleUntilMs`; and
4. let the first remaining valid and unexpired record reserve its ID and remove a later record with the same ID.

Invalid, duplicate, and expired entries are never exposed in the ready read model. The remaining records retain their saved-list order; the accepted history selector owns newest-first display ordering.

When at least one entry is removed, mark balance cleanup as required. Finish `Saving` item recovery before building the cleaned complete document so the complete candidate contains the results of the same load.

Remain in `loading` while making exactly one silent cleanup attempt through TechnicalSpec 006. Do not expose a ready state until that attempt finishes.

- Successful cleanup replaces the stored-value baseline with the writer's exact returned `serialized` value, sets `balanceCleanupPending` to `false`, and then opens the ready state.
- Failed cleanup, including failure of the required latest-value check before writing, keeps the original stored-value baseline, sets `balanceCleanupPending` to `true`, and then opens the ready state without a message or whole-data recovery.
- A `superseded` cleanup discards its candidate, does not open a ready state from the older information, and restarts the complete loading process from the returned newer stored value. The newer load makes its own cleanup decision.
- The invalid or expired entries remain excluded in memory and from every later user-action candidate.
- A later successful complete write includes the cleaned list.

When no entry needs removal, make no cleanup write and open the ready state with `balanceCleanupPending` set to `false` after both lists finish recovery.

`balanceCleanupPending` does not start a timer or repeated background cleanup attempts. After failed cleanup, it remains `true` until either:

- a later user-requested complete save succeeds with the cleaned list, updates the baseline from the returned serialized value, and sets `balanceCleanupPending` to `false`; or
- newer stored information replaces the ready state and starts a new load with its own single cleanup decision.

If a later user-requested save also fails, `balanceCleanupPending` remains `true`. If no later save occurs, refreshing, closing and reopening, or otherwise starting a new load makes one new cleanup attempt when cleanup is still needed.

This silent failure behavior applies because loading and cleanup are not user-requested changes and FeatureSpec 014 requires broken history entries to produce no error message.

## Saving Item Recovery

Pass every raw `savings` item, in saved-list order, through the validation and slot-building rules in TechnicalSpec 005.

- A valid record becomes a normal slot.
- A value is checked inside its own failure boundary, and an invalid value or duplicate record becomes one visible broken slot without stopping later items from being checked.
- A broken slot retains the exact recursively frozen `rawValue` taken from the parsed `savings` list and receives only a temporary in-memory identity.
- Candidate construction must reuse that exact `rawValue` for every unchanged broken slot; it must not clone, rebuild, or edit the value.
- One or many broken items do not cause whole-data recovery while the surrounding document remains usable.
- Broken slots provide no coverage input, while normal slots are ordered and supplied to the accepted coverage calculator.

Detection alone does not rewrite the `savings` list.

## Ready Result

After both lists finish item recovery and any required silent cleanup attempt finishes, build one internally consistent ready state:

- `document` contains the exact loaded main money amount, the cleaned valid and unexpired `Balance Changes` list, and the same saved `Saving` values used to build the loaded slots;
- `mainMoneyAmountCents` is parsed from that document's `mainMoneyAmount`;
- `balanceChanges` contains exactly that document's `balanceChanges` records in the same saved-list order;
- `savingSlots` contains exactly one matching loaded slot for each item in that document's `savings` list;
- coverage is derived only from the normal records in those `savingSlots`; and
- that same `document` is the base for every later candidate.

The baseline, document, and derived values must belong to the same load result. After failed cleanup, the baseline is still the original stored string from which the cleaned document was derived, and `balanceCleanupPending` is `true`. After successful cleanup, the baseline is the exact serialization of the cleaned document and `balanceCleanupPending` is `false`.

No load result creates a `Balance Changes` record, success message, or normal user-controlled reset action.

## Start Again Recovery

The recovery state provides the accepted `Saved data could not be loaded.` message and `Start again` action. It does not expose the normal dashboard.

Every newly entered recovery state starts with `confirming` and `saveFailureVisible` set to `false`. `saveFailureVisible` is temporary in-memory state and is never stored.

Choosing `Start again` while `confirming` is `true` does nothing. Otherwise:

1. create a fresh `EMPTY_WEBSITE_DATA_V1` candidate;
2. set `saveFailureVisible` to `false`, set `confirming` to `true`, and request one complete write through TechnicalSpec 006;
3. never call `removeItem` before the write;
4. on `saved`, replace the baseline with the returned `serialized` value, destroy the recovery state, and open a ready state with stored empty information;
5. on `superseded`, discard the empty candidate, destroy the current recovery attempt, remain in `loading`, and process the returned current stored value through the normal loading rules with no save-failure message; do not retry the `Start again` empty-document write, but allow the newer load to make its own single silent balance-cleanup attempt when needed; or
6. on `save-failed`, reset `confirming`, keep recovery open, keep the previous unreadable stored value unchanged, keep `Start again` available, and set `saveFailureVisible` to `true`.

After failure, `saveFailureVisible` remains `true` without a timer while that recovery state is unchanged. It becomes `false` when another attempt begins. Another failed attempt sets it to `true` again. A successful attempt or any other exit from recovery destroys the state and removes the message. Refreshing, closing, or reopening creates a new recovery state with `saveFailureVisible` set to `false`.

When recovery has an unknown baseline because the initial storage read threw, use the following rule defined by this spec. The save coordinator rereads storage while holding the exclusive save turn. Newly readable usable information is loaded instead of overwritten; a missing or still-unreadable value can be replaced with the empty document; and another read failure keeps recovery open as `save-failed`.

Successful recovery creates no `Balance Changes` entry or success message. The replaced information is not retained for undo.

## Essential Verification

Verify that:

- the initial state exposes neither the normal dashboard nor recovery, and the default `0.00$` dashboard does not appear before the complete loading process finishes;
- a missing key produces the unsaved default `0.00$` state without creating browser storage;
- a missing key retains a known `null` baseline, an exact returned string retains a known string baseline even when unreadable, and a thrown initial read retains an unknown baseline;
- the first successful later action can write the complete version-1 document;
- a valid normalized document restores its main money amount, history, and `Saving` values;
- missing, extra, or invalid root fields, malformed JSON, a wrong version, an invalid main money amount, or unusable lists enter whole-data recovery;
- `null`, arrays, and primitive root values enter recovery without any field access or uncaught error;
- failure while making parsed information immutable enters recovery without exposing partial information or crashing loading;
- partially decoded root information is not shown during whole-data recovery;
- each invalid, duplicate, or expired `Balance Changes` item is excluded while the remaining information stays usable;
- `null`, number, text, boolean, array, and malformed-object list items are handled independently without stopping later items from being checked;
- an unexpected item-validator error is contained to that item instead of crashing the complete load;
- every broken `Balance Changes` item is removed independently without removing a usable item;
- the ready state waits for one required silent cleanup attempt, opens with cleanup not pending after success, opens with cleanup pending after failure, and restarts loading instead of opening when newer stored information supersedes the attempt;
- failed automatic history cleanup shows no message and excluded entries stay excluded from later candidates;
- pending cleanup does not retry repeatedly in the background, remains pending after another failed save, clears after a later successful complete save, and receives one new attempt during a later complete load;
- each invalid or duplicate `Saving` item becomes a broken slot without causing whole-data recovery;
- every retained broken `Saving` raw value is recursively frozen, and unchanged candidates reuse its exact in-memory value;
- an object or array clone does not pass as an unchanged broken value, and one broken slot cannot authorize two candidate items;
- normal `Saving` records alone supply exact coverage input;
- encoder validation rejects temporary state and non-JSON values, including in-memory `bigint` and `symbol` values;
- successful `Start again` overwrites the old value in one write with the exact empty document;
- a superseded `Start again` attempt does not retry its empty-document write, loads the newer information normally, and allows only that newer load's normal single cleanup attempt when needed;
- `Start again` with an unknown baseline rereads under the exclusive save turn, loads a newly usable value without overwriting it, and replaces a missing or still-unreadable value only through one complete empty-document write;
- failed `Start again` preserves the unreadable stored value and recovery state and shows `Changes could not be saved.`;
- the failure message remains without a timer, clears when another attempt starts, returns after another failure, and is removed when recovery closes;
- a new recovery state after refresh, close, or reopen does not restore an earlier failure message;
- stored-document existence is derived from the baseline, and every ready document and derived value belongs to the same load result;
- loading, cleanup, and recovery create no `Balance Changes` entry.

## Boundaries

The shared atomic writer and rollback result belong to TechnicalSpec 006. Received cross-tab updates belong to TechnicalSpec 009. Dashboard presentation after a ready result belongs to TechnicalSpec 011. Future data-version migration requires new accepted specification work.
