# Technical Spec: Cash Money Organizer Website

## Website Identity And Trust Text

The implementation should render the exact website name and short trust label as `Manual Cash Tracker` wherever that first-screen identity or trust wording appears. The capitalization should match exactly.

## Interaction Input Scope

The first version should implement clickable controls for mouse clicks and finger taps. Keyboard support is required only for typing inside money amount inputs and `Saving` square inputs, including the specified `Space` key behavior inside main money amount inputs.

The implementation does not need custom keyboard navigation for clickable controls, custom `Tab` order rules, `Enter` or `Space` activation rules for clickable controls, focus-return rules after save/cancel/delete, or keyboard support for reordering `Saving` squares. Normal browser behavior may exist, but it is not a first-version requirement.

## Open UI Priority

The implementation should allow only one temporary UI to be open at a time.

Temporary UI includes main money action buttons, an `Add`, `Subtract`, or `Modify` money amount input flow, a `Balance Changes` delete action square, a `Balance Changes` delete confirmation, a `Saving` square action state, a `Saving` square create, rename, planned-money-amount change, or broken-square fix input flow, a `Saving` square delete confirmation, and an active `Saving` square reorder drag.

Full-screen `Savings` should be treated as the current view while it is open, not as a temporary UI layer. Opening full-screen `Savings` from the dashboard should be blocked while a dashboard temporary UI is open.

If a temporary UI is already open, the implementation should block attempts to open a different temporary UI until the current temporary UI is closed, canceled, saved, deleted from, finished, or the reorder is dropped or canceled.

If a click or tap both closes or cancels the current temporary UI by an outside-click rule and targets another control that could open a different temporary UI, the implementation should only close or cancel the current temporary UI for that event. It should not also activate the clicked or tapped target.

Actions inside the current temporary UI may transition to the next step of the same flow, such as main money action buttons opening the selected input flow, a `Balance Changes` delete action square opening its delete confirmation, or a `Saving` square action state opening rename, planned-money-amount change, or delete confirmation. The implementation should clear or replace the previous temporary UI state before opening the next one so only one temporary UI is visible at once.

The implementation should not persist temporary UI state in browser storage. This includes visible main money action buttons, selected main money action input flow state, accepted typed input, `Balance Changes` delete action square state, `Balance Changes` delete confirmation state, `Saving` square action state, `Saving` input flow state, `Saving` delete confirmation state, pending hold state, active reorder drag state, selected delete targets, and temporary reorder positions.

If the page refreshes, the browser tab or window closes, or the website is opened again later while any temporary UI is open, the implementation should silently discard that temporary UI. The next load should initialize with no temporary UI open and should render the latest successfully saved data that passes saved-data validation. The implementation should not restore the open temporary UI, should not save unsaved typed input, should not confirm any pending delete, should not save any pending reorder, should not create a `Balance Changes` entry, should not show a user-facing message, and should not register a browser leave warning only because a temporary UI is open.

## Money Amount Storage

The current money amount should be saved separately from the visible history list.

Money amount values can include cents, but the maximum allowed money amount is `999,999.99$`. The current money amount and each saved `Saving` square planned money amount should stay from `0.00$` through `999,999.99$` as their rules allow. The implementation should block or reject values above `999,999.99$`.

Money amount calculations should preserve the exact normalized money amount value and the two decimal places. Use an exact decimal-string representation or exact string/BigInt-based helpers so cents and formatting stay exact. Save money amount values as normalized plain decimal strings with exactly two decimal digits, no `$` sign, no comma separators, and no unneeded leading zeros before the decimal point except the single `0` in values below `1.00`. Valid saved examples include `0.00`, `5.00`, `5895.50`, and `999999.99`. Comma separators and the `$` sign are visible formatting only. They should be generated while editing and rendering visible money amounts, and they should not be saved or required for exact money amount math.

The money amount is calculated and updated from:

- Default `0.00$` starting state when no saved data exists.
- Recorded `Add` money actions.
- Recorded `Subtract` money actions.
- Silent `Modify` corrections.

Deleting old `Balance Changes` entries from browser storage should not change the current money amount, because the current money amount is saved separately.

Deleting a `Balance Changes` entry should only remove that visible history entry. It should not change, recalculate, or reverse the current money amount.

Deleting a `Balance Changes` entry should require user confirmation before deletion.

The delete action for a `Balance Changes` entry should open from the entry itself. Touch users should press and hold the entry for `600ms`. Mouse users should click and hold the entry for `600ms`. Before the `600ms` timer finishes, releasing the press or click, moving the pointer or finger, or starting to scroll should cancel the pending delete action. Canceling the pending hold should clear the pending timer, render no small square, write no browser storage, change no saved data, delete no entry, create no `Balance Changes` entry, and show no message. If the `600ms` hold completes without being canceled, the implementation should then render a small square in the middle of the screen with two actions exactly named `Delete` and `Cancel`. Only one `Balance Changes` delete action square should be open at a time.

Opening, canceling, or closing the small `Delete` and `Cancel` square should not delete the `Balance Changes` entry and should not write browser storage. Clicking `Cancel` should close the small square. Clicking or tapping outside the small square should close it. Moving the pointer or finger should not close the small square after it is open. Scrolling should not close the small square after it is open. Clicking `Delete` should close the small square and open the later delete confirmation. Only confirming the later delete confirmation should remove the visible history entry.

The implementation should handle the browser Back button, mobile browser back gesture, or system Back action while the small `Delete` and `Cancel` square is open by closing that small square and keeping the user on the dashboard. This should write no browser storage, change no saved data, delete no entry, create no `Balance Changes` entry, and show no message.

The confirmation message should be exactly `Delete this Balance Change?`, with buttons exactly named `Cancel` and `Delete`.

Clicking `Cancel` in the later `Delete this Balance Change?` confirmation should close the confirmation, keep the selected `Balance Changes` entry visible, write no browser storage, change no saved data, and show no message. Clicking or tapping outside that later confirmation should do the same. Only clicking `Delete` in that later confirmation should remove the visible history entry.

The implementation should handle the browser Back button, mobile browser back gesture, or system Back action while the later `Delete this Balance Change?` confirmation is open by closing that confirmation and keeping the user on the dashboard. This should keep the selected `Balance Changes` entry visible, write no browser storage, change no saved data, delete no entry, create no `Balance Changes` entry, and show no message.

After a `Balance Changes` entry is deleted, the website should not offer undo.

The default `0.00$` starting state should not create a visible `Balance Changes` entry.

When the visible `Balance Changes` list is empty after loading, cleanup, or a user action, the implementation should render no `Balance Changes` entry rows and no empty-state text, placeholder, icon, or other empty-state content.

The implementation should render `Balance Changes` as a large square directly under the main money amount on the dashboard. The square should take most of the dashboard page space under the main money amount, and its entry list should have its own internal scroll when the visible entries exceed the square height.

The dashboard page should still be scrollable past the `Balance Changes` square. `Savings` should render below the `Balance Changes` square in the normal dashboard page flow. The implementation should not cap valid 30-day `Balance Changes` entries to a smaller visible maximum; all valid non-expired entries should remain reachable inside the scrollable square.

The internal `Balance Changes` scroll container should prevent scroll chaining to the dashboard page. Wheel, trackpad, touch-scroll, or pointer-driven scroll that starts inside the large `Balance Changes` square should not move the dashboard page, including when the internal entry list is already at the top or bottom. Dashboard page scrolling should still work when the scroll starts outside the large `Balance Changes` square.

Visible `Balance Changes` rows should render the signed money amount, action text, and both the created date and created time for that entry: `+{money amount} added` or `-{money amount} subtracted`, plus the visible created date and created time. A date-only display is not enough. The visible date and time format should be like `July 21, 2026 at 3:45 PM`.

Visible `Balance Changes` entries should render as compact entry boxes, not large panels. The signed money amount and action text should align to the top-left corner of the entry. The visible created date and created time should align to the bottom-right corner of the same entry, below the money change text and close enough that the two details feel connected. Mobile should keep the same layout. Text may wrap only as needed to stay readable, but the implementation should preserve the top-left money change text area and bottom-right visible date and time area on small screens.

Visible `Balance Changes` dates and times should use the user's browser/device local date and time. The first version should not use a server time, cloud time, or external time service for `Balance Changes`.

Visible `Balance Changes` rows should not render the previous money amount, new money amount, internal exact created date and time, seconds, milliseconds, or internal visible until date and time. Saved previous money amount, new money amount, internal exact created date and time, and internal visible until date and time are internal data fields only. The visible created date and created time should be saved and rendered in the visible `Balance Changes` row.

Money action availability should be based on the current money amount:

- If the current money amount is `0.00$`, `Add` and `Modify` should be available. `Subtract` should not be available because the money amount cannot go below `0.00$`.
- If the current money amount is greater than `0.00$` and less than `999,999.99$`, `Add`, `Subtract`, and `Modify` should be available.
- If the current money amount is exactly `999,999.99$`, only `Subtract` and `Modify` should be available. `Add` should not be rendered because the current money amount cannot go above `999,999.99$`.

When main money actions are visible, they should render as buttons lined together horizontally near the main money amount. When `Subtract` is hidden at `0.00$`, the visible order should be `Add`, then `Modify`. When all three actions are visible, the left-to-right order should be `Add`, `Subtract`, and `Modify`. When `Add` is hidden at `999,999.99$`, the visible order should be `Subtract`, then `Modify`. The main money action buttons should not render as a modal, bottom sheet, separate page, or large expanded area.

When main money action buttons are visible, a click or tap on the main money amount again should close the buttons without changing saved data, writing browser storage, creating a `Balance Changes` entry, or showing a message.

When main money action buttons are visible, a click or tap outside the main money amount and outside the action buttons should close the buttons without changing saved data, writing browser storage, creating a `Balance Changes` entry, or showing a message.

When main money action buttons are visible, dashboard page scrolling should close the buttons without changing saved data, writing browser storage, creating a `Balance Changes` entry, or showing a message.

Clicking a visible `Add`, `Subtract`, or `Modify` action button should start that selected money amount input flow and should not be handled by the outside-click dismissal behavior.

When a selected main money action input flow starts, the implementation should hide the visible main money action buttons. The selected action should be kept as internal state for validation and saving only. The open input flow should not render a visible `Add`, `Subtract`, or `Modify` reminder.

Money action validation should block negative values and values greater than `999,999.99$`. `Add` and `Subtract` should require a money amount greater than `0.00$` and not greater than `999,999.99$`. `Modify` should allow money amounts from `0.00$` through `999,999.99$`. Clicking `Yes` while the main money action input is `0.00` in `Add` or `Subtract` should be treated as no action: no message, no money amount change, no browser storage write, no `Balance Changes` entry, and the same money amount input step stays open. Clicking `Yes` in `Modify` with the same money amount that is already shown should also be treated as no action: no message, no money amount change, no browser storage creation or write, no `Balance Changes` entry, no `Balance Changes` cleanup, and the same money amount input step stays open until the user enters a different valid money amount or cancels. If an `Add` action would make the current money amount greater than `999,999.99$`, the website should do nothing: no message, no money amount change, no browser storage write, no `Balance Changes` entry, and the same money amount input step stays open.

`Add`, `Subtract`, and `Modify` money amount input flows should render a horizontal money amount input square in the middle of the screen. The main money amount circle should remain visible only as a dimmed, inactive background element while the horizontal input square is open. The input flow should be the active part of the screen. Clicking or tapping the visible main money amount circle behind the input flow should be handled by the same outside-cancel behavior as other outside areas: close the input flow, return to the dashboard money amount view, leave saved data unchanged, create no `Balance Changes` entry, and show no message. Clicking or tapping the visible main money amount circle should not reopen the main money action buttons. The square should start by showing `0.00` without the `$` sign.

When a main money action input flow opens, the implementation should focus the horizontal input square immediately so accepted typing is handled without requiring a second tap or click. On supported mobile devices, this focus should request the mobile keyboard immediately. The cursor should stay at the end when shown.

Main money action input fields should request a mobile keyboard suitable for digit entry on supported devices. The implementation should still validate the entered value because desktop keyboards, physical keyboards, and browser differences can bypass the mobile keyboard layout. The implementation should render a rectangular `Cent` button for all users while a main money action input is open, without using device or keyboard detection to decide whether the button appears. The `Cent` button should be part of the main money amount input controls and should appear directly under the horizontal input square on mobile and desktop. On mobile, when the browser and keyboard allow it, this placement should also keep the `Cent` button directly above the mobile keyboard. The `Cent` button should behave the same as pressing `Space` in the accepted input sequence and should not trigger input-flow cancellation.

Main money action input fields should accept only digits from `0` through `9` and separator input from the `Space` key or `Cent` button. The first accepted character should be a digit from `0` through `9`. The decimal point, comma separators, `$` signs, letters, minus signs, and other blocked characters should keep the previous input value and show no message. The decimal point and comma separators shown while editing should be generated by the implementation only.

Accepted digits before an accepted separator should append to the whole money amount part. Accepted digits should not automatically shift into cents because more digits were typed. `0` should be treated as a normal digit, not as a zero-position skip. Unneeded leading zeros in the whole money amount should be normalized away while editing and saving; typing `0005` should display `5.00`.

The implementation should keep an accepted input sequence made of digits and non-visible separators. Separator input from the `Space` key or `Cent` button should be accepted only after at least one digit and only when the previous accepted input is a digit. Starting `Space` key presses, starting `Cent` taps, consecutive `Space` key presses, consecutive `Cent` taps, or mixed consecutive separator inputs should be blocked with no message. Typing `Space`, `Space`, `Space`, then `5` should accept only the `5` and display `5.00`. Tapping or clicking `Cent`, `Cent`, `Cent`, then typing `5` should accept only the `5` and display `5.00`.

Main money action input values should always display with two digits after the decimal point, automatic comma separators for thousands and larger values, and no `$` sign while the user is typing. After the user saves, browser storage should save the money amount as a normalized plain decimal string without the `$` sign or comma separators, and the rendered money amount should show comma separators for thousands and larger values and the `$` sign at the end.

For accepted input without separator input, all digits are whole money amount digits: typing `5` should display `5.00`, `58` should display `58.00`, `589` should display `589.00`, `5895` should display `5,895.00`, `58955` should display `58,955.00`, and `589550` should display `589,550.00`.

For accepted input with separator input from `Space` or `Cent`, split the accepted input sequence into digit groups at accepted separators. If the accepted input ends with a separator, treat all completed digit groups as the whole money amount and display cents as `00`. If the final group after a separator has one or two digits, treat that final group as cents and left-pad it with `0` when it has one digit; join all earlier groups as the whole money amount. If there is only one accepted separator and the final group grows to three or more digits, treat that final group as another whole money amount group and display cents as `00`. Once the input has two accepted separators, the final group is the cents group and should accept at most two digits. Additional separator input after two accepted separators should be blocked with no message.

Examples: typing `5`, then `Space`, should keep the display at `5.00`; typing `5`, then `Space`, then `5` should display `5.05`; typing `5`, then `Space`, then `50` should display `5.50`; typing `58`, then `Space`, then `430` should display `58,430.00`; typing `58`, then `Space`, then `430`, then `Space`, then `88` should display `58,430.88`; typing `0`, then `Space`, then `5` should display `0.05`; and typing `999999`, then `Space`, then `99` should display `999,999.99`. Tapping or clicking `Cent` should work the same as pressing `Space`, so typing `5`, then choosing `Cent`, then typing `50` should display `5.50`.

When a user deletes one typed character from a main money action input, the implementation should remove the last accepted digit or accepted separator from the accepted input sequence and format the remaining accepted input again with two digits after the decimal point, automatic comma separators when needed, and no `$` sign while the input is still open. Deleting from `5.50` after typing `5`, `Space`, `50` or typing `5`, tapping `Cent`, then typing `50` should display `5.05`; deleting again should display `5.00`.

Implementation should treat main money action input as an append-only controlled input surface, not a normal free text editor. Pointer and touch focus should not move the cursor away from the end. The implementation should prevent or ignore partial text selection and selection replacement. Accepted key events should update only the end of the accepted input sequence. Backspace and Delete should remove only the last accepted digit or accepted separator, regardless of any native cursor or selection state. Generated comma separators and the generated decimal point should never be directly editable.

When a user deletes all typed digits from a main money action input, the implementation should set the input display back to `0.00`. The main money action input should not render as an empty field.

Main money action input fields should keep the previous input value when the user types a character that is not allowed. Letters, minus signs, decimal points, `$` signs, manually typed comma separators, starting `Space`, consecutive `Space`, additional `Space` after two accepted separators, a third cents digit after the cents group is fixed by two accepted separators, and other blocked typed characters should not appear in the field, and no message should appear for those blocked typed characters. If the user taps `Cent` when a separator would be blocked by the same rules, the field should keep its previous input value and no message should appear.

Main money action input fields should enforce the `999,999.99` maximum by parsed money amount value, not by an unrelated character-count limit.

Money amount values up to `999,999.99$` should stay contained in the horizontal input square, dashboard display, `Balance Changes` rows, and `Savings` displays. The implementation should prevent horizontal page overflow, text overlap, hidden actions, and broken layout. It may use wrapping, long-string breaking, readable text scaling, or internal input scrolling as needed, but it must preserve the full typed or visible money amount. Main money action inputs and rendered visible money amounts of `1,000.00$` or more should include required comma separators.

Main money action input fields should block paste. If the user tries to paste letters, numbers, symbols, or any other content into a main money action input, the pasted content should not appear, the field should keep its previous value, and no message should appear.

`Add`, `Subtract`, and `Modify` money amount input flows should render the rectangular `Cent` button under the horizontal input square, the exact text `Save Changes` under `Cent`, and buttons exactly named `Yes` and `Cancel` under `Save Changes`. `Yes` should run the validation and save rules for the selected action. `Cancel` should close the money amount input flow, return to the dashboard money amount view, leave all saved data unchanged, create no `Balance Changes` entry, and show no message. Clicking or tapping outside the horizontal input square, `Save Changes`, `Yes`, `Cancel`, and the `Cent` button should act like `Cancel`, including clicking or tapping the dimmed, inactive main money amount circle behind the input flow. The outside-cancel handler should ignore taps or clicks on the `Cent` button so the `Cent` button can update the accepted input sequence before any dismissal behavior runs.

The implementation should handle the browser Back button, mobile browser back gesture, or system Back action while an `Add`, `Subtract`, or `Modify` main money action input flow is open by running the same close-without-saving behavior as `Cancel`. It should close the input flow, return to the dashboard money amount view, leave saved data unchanged, create no `Balance Changes` entry, and show no message.

The implementation should not persist temporary main money action input state. If the user refreshes the page, closes the browser tab or window, or opens the website again later while an `Add`, `Subtract`, or `Modify` main money action input flow has unsaved typed input, the next load should restore only the last successfully saved browser data and should not restore the open input flow or typed value. It should not write browser storage, create a `Balance Changes` entry, or trigger a browser leave warning for that unsaved input.

After a successful `Add`, `Subtract`, or `Modify` that changes the money amount, the implementation should close the horizontal input square, reset the temporary accepted input sequence so the next main money action input starts at `0.00`, return to the dashboard money amount view, hide the main money action buttons, render the updated money amount, write only the data required by that action, and show no message. Invalid attempts and no-action attempts should keep the same money amount input step open according to their specific validation rules.

Main money action input values should be shown as decimal money amount text without the `$` sign while editing, such as `5.00`, `5.05`, `0.05`, `58,430.88`, or `999,999.99`. Editing display should include automatic comma separators when the value is `1,000.00` or more. Raw typed digit sequences should be converted immediately as whole money amount digits unless the user separates a final cents group with the `Space` key or `Cent` button; for example, raw digits `589550` should display as `589,550.00`, while `58 Space 430 Space 88` should display as `58,430.88`. Using `58`, `Cent`, `430`, `Cent`, `88` should display as `58,430.88`. Saved money amounts should use normalized plain decimal strings without the `$` sign or comma separators, such as `5.00`, `5.05`, `0.05`, `58430.88`, or `999999.99`. Rendered money amounts should show the `$` sign at the end and comma separators for thousands and larger values, such as `5.00$`, `5.05$`, `0.05$`, `58,430.88$`, or `999,999.99$`. Exact internal math should use a string or `BigInt` representation, and saved browser data should preserve the normalized plain decimal money amount.

## Savings Coverage And Storage

Visible money amounts should use two digits after the decimal point, such as `0.00$`, `5.00$`, and `14.50$`. Visible money amounts of `1,000.00$` or more should use comma separators every three digits before the decimal point, starting from the right, such as `5,895.50$` and `999,999.99$`.

Each `Saving` square should be saved with:

- ID.
- Name.
- Planned money amount greater than `0.00$` and not greater than `999,999.99$`.
- Order.

The first version should not save created date or updated date fields on `Saving` squares. If saved browser data contains extra `Saving` square date fields anyway, those extra fields should be ignored and should not make that `Saving` square broken.

The website should not save `Saving` squares with a planned money amount of `0.00$`.

Trying to save a new `Saving` square with a planned money amount of `0.00$` or greater than `999,999.99$` should be treated as no action: no message, no new `Saving` square, no browser storage write, no `Balance Changes` entry, and the same `Saving` square create input step stays open.

`Saving` square planned money amounts should allow cents, with up to two digits after the decimal point, and should be saved as normalized plain decimal strings from greater than `0.00` through `999999.99`, with no `$` sign and no comma separators, such as `14.56` and `5895.50`. Rendered planned money amounts should show the `$` sign at the end and comma separators for thousands and larger values, such as `14.56$` and `5,895.50$`.

`Saving` square planned money amounts should use their own shown money amount normalization: `0.00$` for zero, two decimal digits for nonzero whole money amounts, two decimal digits for nonzero money amounts with cents, and comma separators for visible planned money amounts of `1,000.00$` or more. Saved planned money amount values should use the normalized plain decimal storage format.

`Saving` square planned money amount inputs should enforce the `999,999.99$` maximum and should not save values above that limit. Saved planned money amounts should use the same exact normalized plain decimal representation as the main money amount.

If the planned money amount is empty when creating a `Saving` square, the create flow should stay open without showing an error message. The failed save should not write browser storage, should not create a `Saving` square, and should not create a `Balance Changes` entry.

If the planned money amount is empty when changing an existing `Saving` square, the planned-money-amount change flow should stay open without showing an error message. The failed save should not write browser storage, should not change the saved planned money amount, and should not create a `Balance Changes` entry.

If the planned money amount is greater than `999,999.99$` when creating or changing a `Saving` square, the flow should stay open without showing an error message. The failed save should not write browser storage, should not create a new `Saving` square, should not change the saved planned money amount, and should not create a `Balance Changes` entry.

`Saving` square planned money amount inputs should request a decimal numeric keyboard on devices that support it. The implementation should still validate the entered value because desktop keyboards and browser differences can bypass the mobile keyboard layout.

`Saving` square planned money amount inputs should allow numbers and one decimal point, while still blocking more than two digits after the decimal point, `$` signs, comma separators, and other invalid typed characters.

While editing, `Saving` square planned money amount inputs should keep and display the accepted value as raw decimal number text. The implementation should not add the `$` sign, comma separators, or automatic trailing decimal zeros before `Save`. It should not render a `Cent` button for these inputs, and it should not apply the main money action input `Space` key cents behavior. The `Space` key should be treated as blocked input in `Saving` square planned money amount inputs. If the accepted value has no decimal point, parse it as a whole money amount. If the accepted value has one decimal point, parse digits after the decimal point as cents. On successful `Save`, normalize the saved value to a plain decimal string with exactly two digits after the decimal point and no comma separators, then render it with two decimal digits, comma separators when needed, and the `$` sign.

`Saving` square planned money amount inputs should block paste. If the user tries to paste letters, numbers, symbols, or any other content into a planned money amount input, the pasted content should not appear, the input should keep its previous value, and no message should appear.

`Saving` square create, rename, planned-money-amount change, and broken-square fix input flows should render two text actions at the bottom: `Save` and `Cancel`. `Save` should run the validation and save rules for that action. `Cancel` should close the input flow, return to the `Saving` squares view, leave all saved data unchanged, create no `Balance Changes` entry, and show no message.

The implementation should not persist temporary `Saving` square input state as draft data. If the user refreshes the page, closes the browser tab or window, opens the website again later, cancels the input flow, closes the input flow with `<` or Back, or leaves `Savings` after the input flow has been closed before a successful `Save`, the next load or next `Savings` open should restore only the last successfully saved browser data and should not restore the open input flow, typed `Saving` name, or typed planned money amount. It should not write draft input to browser storage, create a `Balance Changes` entry, trigger a browser leave warning, or show a message. This rule should not change browser storage save failure behavior; if a valid `Saving` input save fails, the same input flow should stay open with the same typed values.

Saved `Saving` square names should be unique inside `Savings`.

The implementation should not set a character-count maximum for `Saving` square names and should not reject names only because they are long. Names should be treated as text values, so one-letter names, number-only names, names with numbers before or after words, multiple-word names, full sentences, symbols, punctuation, emoji characters, and very long names are valid when they satisfy the required-name and unique-name rules.

Before creating, renaming, or fixing a `Saving` square, the website should trim spaces at the beginning and end of the entered name.

Spaces inside the trimmed `Saving` square name should be preserved in saved browser data.

If the trimmed name is empty when creating a `Saving` square, the create flow should stay open without showing an error message. The failed save should not write browser storage, should not create a `Saving` square, and should not create a `Balance Changes` entry.

If the trimmed name is empty when renaming a `Saving` square, the rename flow should stay open without showing an error message. The failed save should not write browser storage, should not rename the `Saving` square, and should not create a `Balance Changes` entry.

The website should check duplicate `Saving` square names by comparing trimmed names without uppercase or lowercase differences.

Duplicate-name checks should include normal saved `Saving` squares and visible broken saved `Saving` squares whose saved name can be read and is not empty after trimming spaces. A broken `Saving` square with a readable saved name should reserve that name until it is fixed or deleted.

During a broken-square fix, duplicate-name checks should not count the broken `Saving` square being fixed against itself. Normal saved `Saving` squares and other broken saved `Saving` squares with the same trimmed name should still block the fix.

If a create, rename, or fix action would duplicate a reserved `Saving` square name, the website should not save that duplicate name.

If a create, rename, or fix action would duplicate a reserved `Saving` square name, the website should close the create, rename, or fix flow and return to the `Saving` squares view without showing a duplicate-name error message. The failed duplicate-name action should not write browser storage, should not create a `Saving` square, should not rename an existing `Saving` square, should leave the broken `Saving` square broken when a fix name is duplicate, and should not create a `Balance Changes` entry.

For new `Saving` square creation, validation should run in this order:

1. Planned money amount is missing, `0.00$`, or greater than `999,999.99$`.
2. Trimmed `Saving` name is empty.
3. Trimmed `Saving` name duplicates another saved `Saving` square name, ignoring uppercase or lowercase differences.

The first matching rule should decide the result. Duplicate-name create behavior should run only after the planned money amount is greater than `0.00$`, not greater than `999,999.99$`, and the trimmed `Saving` name is not empty. If a duplicate name is entered with a missing, `0.00$`, or above-limit planned money amount, the same `Saving` square create input step should stay open with no message and without writing browser storage.

For broken-square fix, validation should run in the same order:

1. Planned money amount is missing, `0.00$`, or greater than `999,999.99$`.
2. Trimmed `Saving` name is empty.
3. Trimmed `Saving` name duplicates a reserved `Saving` square name, ignoring uppercase or lowercase differences.

The first matching fix rule should decide the result. Duplicate-name fix behavior should run only after the planned money amount is greater than `0.00$`, not greater than `999,999.99$`, and the trimmed `Saving` name is not empty. If a duplicate name is entered during broken-square fix with a missing, `0.00$`, or above-limit planned money amount, the same broken-square fix input step should stay open with no message, without writing browser storage, without fixing the broken `Saving` square, and without creating a `Balance Changes` entry.

Rendered `Saving` square names should wrap and stay contained inside the square layout. Long names, including long unbroken number or letter sequences, should not cause horizontal page overflow, overlap other square content, or force the website to reject the saved name. The implementation may use wrapping, breaking, or internal containment for very long names, but it should keep the saved name complete.

When saved browser data can be read and has data version `1`, the implementation should validate each saved `Saving` square before using it as a normal square.

Saved `Saving` square planned money amounts should be treated as valid only when they use the same normalized plain decimal storage format as the main money amount. Saved planned money amount values such as `5`, `5.0`, `005.00`, `5,000.00`, and `5.000` should make only that `Saving` square broken when the rest of the saved browser data can still be read.

A saved `Saving` square should be treated as a broken square if it has a missing ID, duplicate ID, missing name, empty trimmed name, duplicate trimmed name ignoring uppercase or lowercase differences, missing planned money amount, invalid planned money amount, planned money amount of `0.00$` or less, planned money amount greater than `999,999.99$`, missing order, invalid order, or duplicate order.

For duplicate saved IDs, duplicate saved names, or duplicate saved orders, the first matching saved `Saving` square in the saved list order should stay normal if it is otherwise valid. Later matching saved `Saving` squares should be treated as broken squares.

Broken `Saving` squares should be kept in the loaded `Savings` view as error squares instead of making all saved browser data fail. If a broken square has a usable saved position, it should appear near that saved position. If no usable position exists, it should appear after the valid `Saving` squares.

A broken `Saving` square should render the exact text `Saving could not be loaded.` and actions exactly named `Fix` and `Delete`.

Broken `Saving` squares should be excluded from `Savings money amount` calculation, top needed note calculation, coverage calculation, `Saving` square duplicate-name checks for normal valid squares, and reorder calculation until fixed. Broken `Saving` squares should not render coverage bars.

Broken `Saving` squares should be locked in their current displayed positions until fixed or deleted. They should not be draggable, should not start a reorder timer, should not render a drag placeholder, and should not enter a dragged visual state. Holding, clicking and holding, moving, or dragging a broken `Saving` square should write no browser storage, change no saved data, delete no square, create no `Balance Changes` entry, and show no message.

While broken `Saving` squares are visible, normal default `Saving` squares should still be reorderable when no `Saving` square is in action state, input state, or delete confirmation state. During normal square reorder, broken `Saving` squares should act as fixed non-draggable visual entries. Only normal `Saving` squares should move through normal-square positions, and broken saved square data should not be modified by the normal square reorder.

Activating `Fix` on a broken `Saving` square should replace that broken square with a temporary `Saving` input square in the same visible position. The fix input square should ask for a valid `Saving` name and planned money amount greater than `0.00$` and not greater than `999,999.99$`. The fix input square should not render as a modal, bottom sheet, or separate page. The fix flow should use the same required-name, duplicate-name, money amount, typing, paste-blocking, `Save`, and `Cancel` rules as creating a new `Saving` square, including readable saved names from other broken `Saving` squares.

When a broken `Saving` square is fixed successfully, the implementation should replace the broken saved square with a valid normal `Saving` square, save browser storage, recalculate the `Savings money amount`, recalculate the top needed note, recalculate coverage bars, create no `Balance Changes` entry, and show no message. The fixed square should keep the broken square's current displayed position when possible. If the broken square had a missing or duplicate ID, the fixed square should get a valid unique ID. If the broken square had a missing, invalid, or duplicate order, the fixed square should get a valid order based on its current displayed position.

Activating `Delete` on a broken `Saving` square should use the normal `Saving` square delete confirmation: a small confirmation square in the middle of the screen with the exact message `Delete this Saving?` and buttons exactly named `Cancel` and `Delete`. Clicking `Cancel` or clicking or tapping outside the small confirmation square should close the confirmation, keep the broken square visible, write no browser storage, create no `Balance Changes` entry, and show no message. Confirming delete should close the confirmation, remove only that broken square from saved browser data, save browser storage, recalculate the `Savings money amount`, recalculate the top needed note, recalculate coverage bars, create no `Balance Changes` entry, update no other `Saving` squares, show no message, and offer no undo.

Opening `Savings` should render a full-screen `Savings` view instead of leaving `Savings` as only an inline dashboard section. The full-screen `Savings` view should include the `Savings money amount` and the visible `Saving` squares.

The full-screen `Savings` view should render a small `<` sign in the top-left corner as the back action. When no `Saving` square input flow or `Saving` square delete confirmation is open, activating `<` should return to the dashboard without writing browser storage, changing saved data, or creating a `Balance Changes` entry.

The implementation should handle the browser Back button, mobile browser back gesture, or system Back action while the full-screen `Savings` view is open by closing `Savings` and returning to the dashboard when no `Saving` square input flow or `Saving` square delete confirmation is open. This browser Back behavior should not write browser storage, change saved data, create a `Balance Changes` entry, or show a message.

The implementation should give an open `Saving` square input flow or `Saving` square delete confirmation priority over closing the full-screen `Savings` view. If a `Saving` square create, rename, planned-money-amount change, or broken-square fix input flow is open, activating `<` or using the browser Back button, mobile browser back gesture, or system Back action should close that input flow with the same behavior as `Cancel`, keep the full-screen `Savings` view open, discard temporary typed values, restore the `+`, normal square, or broken square that the input replaced, write no browser storage, change no saved data, create no `Balance Changes` entry, and show no message.

If a `Saving` square delete confirmation is open, activating `<` or using the browser Back button, mobile browser back gesture, or system Back action should close that confirmation, keep the full-screen `Savings` view open, delete nothing, write no browser storage, change no saved data, create no `Balance Changes` entry, and show no message. If the confirmation belongs to a normal `Saving` square, that square should return to its default state. If the confirmation belongs to a broken `Saving` square, the broken square should remain visible. After the input flow or delete confirmation is closed, the next `<` activation or Back action should close the full-screen `Savings` view when no `Saving` square input flow or delete confirmation is open.

The implementation should render the full-screen `Savings` view with a fixed top area containing the small `<` sign, the `Savings money amount`, and the optional top needed note. The `Saving` squares container should scroll below the fixed top area. The scrollable area should include enough top spacing or layout offset so no `Saving` square, temporary input square, confirmation state, or coverage bar is hidden underneath the fixed top area.

The implementation should render visible `Saving` squares in one vertical column on mobile and desktop. It should not use multiple columns or a grid for `Saving` squares. For normal `Saving` squares, the DOM order, visual column order, saved order, drag order, and coverage order should match after each completed reorder action. Broken saved `Saving` squares can appear in the same column as locked non-draggable entries, but they are not part of coverage order or drag order until fixed.

The implementation should render the add-a-saving control as a circular `+` control inside the `Saving` squares area. If there are no visible normal `Saving` squares and no visible broken saved `Saving` squares, the circular `+` control should be centered in that area and should be the empty state. The implementation should not render a separate empty-state text sentence for no `Saving` squares.

If one or more visible normal `Saving` squares exist, the circular `+` control should render at the top-left of the `Saving` squares area, before the ordered squares. A broken saved `Saving` square should count as visible content for this circle `+` placement rule only. If one or more broken saved `Saving` squares are visible and no normal `Saving` squares are visible, the circular `+` control should still render at the top-left of the `Saving` squares area, before those broken saved `Saving` squares. Broken saved `Saving` squares should still be excluded from `Savings money amount`, top needed note, and coverage bar calculations. Activating the `+` control from either position should open the same new `Saving` square create flow and should not save browser storage until the user successfully creates a valid `Saving` square.

Activating the `+` control should replace that `+` control with a temporary `Saving` input square inside the `Saving` squares area. If the `+` control was centered, the temporary input square should render centered in that area. If the `+` control was at the top-left, the temporary input square should render at the top-left before the existing squares. The create input square should contain the `Saving` name input, planned money amount input, and bottom text actions `Save` and `Cancel`. The create input square should not render as a modal, bottom sheet, or separate page. While the create input square is open, the implementation should not render a second create `+` action. The temporary create input square should not be saved as a `Saving` square, should not affect `Savings money amount`, should not affect the top needed note, and should not affect coverage bars until `Save` succeeds.

In its default state, each visible `Saving` square should render the `Saving` name in the top-left corner, the planned money amount on the right side of the same top row, and the thin coverage bar at the bottom.

Activating a `Saving` square should change that same square into its action state. In the action state, the square should still render the `Saving` name in the top-left corner and the planned money amount on the right side of the same top row, should render `Delete` at the bottom center, and should not render the thin coverage bar.

For touch input on a normal default `Saving` square, the implementation should treat finger movement of more than `8px` before touch release and before the `600ms` reorder hold completes as scroll intent. Scroll intent should cancel the pending tap activation and any pending reorder hold, should not open the action state, should not start reorder, should not render a drag placeholder, should write no browser storage, should change no saved data, should create no `Balance Changes` entry, and should show no message. If touch release happens before `600ms` without movement greater than `8px`, the implementation should treat it as a tap activation. If touch input stays down for `600ms` but releases before moving at least `8px`, the implementation should clear the pending reorder state and do nothing: no action state, no reorder, no drag placeholder, no browser storage write, no saved data change, no `Balance Changes` entry, and no message.

In the action state, activating the rendered `Saving` name should open the rename flow for that square. Activating the rendered planned money amount should open the planned-money-amount change flow for that square. Activating `Delete` should start the delete confirmation for that square. The implementation should not open a separate action menu or larger action square for rename, planned-money-amount change, and delete.

The `Saving` square delete confirmation should render as a small confirmation square in the middle of the screen, not inside the `Saving` square, not as a temporary square state, not as a bottom sheet, and not as a separate page. It should render the exact message `Delete this Saving?` and buttons exactly named `Cancel` and `Delete`. Only one `Saving` square delete confirmation should be open at a time.

Opening a `Saving` square delete confirmation from a normal `Saving` square should clear that square's action state. Clicking `Cancel` or clicking or tapping outside the small confirmation square should close the confirmation, return the selected normal `Saving` square to its default state, write no browser storage, create no `Balance Changes` entry, and show no message. Clicking `Delete` in the small confirmation square should close the confirmation and run the delete confirmation behavior for only the selected `Saving` square.

Opening rename for an existing `Saving` square should replace that same square with a temporary `Saving` input square in the same visible position. The rename input square should render only the new `Saving` name input and the bottom text actions `Save` and `Cancel`. It should not render as a modal, bottom sheet, separate page, or separate floating input.

Opening planned-money-amount change for an existing `Saving` square should replace that same square with a temporary `Saving` input square in the same visible position. The planned-money-amount change input square should render only the new total planned money amount input and the bottom text actions `Save` and `Cancel`. It should not render as a modal, bottom sheet, separate page, or separate floating input.

When a normal `Saving` square is in action state, the implementation should close that action state if the user clicks or taps outside that same square. This outside-action-state dismissal should return the square to its default state, write no browser storage, create no `Balance Changes` entry, and show no message.

Clicks or taps inside the open action-state square should not be handled as outside-action-state dismissal. Activating the rendered `Saving` name, rendered planned money amount, or `Delete` should run the matching square action instead. A click or tap on a blank area inside the same open action-state square should do nothing: keep that square in action state, write no browser storage, change no saved data, create no `Balance Changes` entry, and show no message.

If the user clicks or taps another normal default `Saving` square while one square is in action state, the implementation should close the old action state and should not open the clicked square in its own action state from that same event. The user can click or tap that other square again after no temporary UI is open. Scrolling the `Saving` squares container should not close the action state by itself. Activating the small `<` sign should clear any open `Saving` square action state as part of returning to the dashboard.

The whole normal default `Saving` square should be draggable for touch and mouse users. Touch users should reorder by holding the whole square for `600ms`, then moving it. Mouse users should reorder by clicking and holding the whole square for `600ms`, then dragging it. For touch users, moving the finger more than `8px` before the `600ms` hold completes should be handled as scroll intent and should cancel the pending reorder hold. After the `600ms` hold completes without being canceled, dragging should start only after the pointer or finger moves at least `8px`. If a touch or mouse input holds for `600ms` but releases before moving at least `8px`, the implementation should clear the pending reorder state and do nothing: no action state, no reorder, no drag placeholder, no browser storage write, no saved data change, no `Balance Changes` entry, and no message. Holding or dragging should not open rename, planned-money-amount change, or delete.

During reorder, the dragged `Saving` square should follow the user's finger or mouse pointer. The original position should render a placeholder the same size as the dragged square. If the pointer or finger is within `40px` of the top or bottom edge of the scrollable `Saving` squares container during reorder, that container should auto-scroll at a fixed speed of `8px` per animation frame. Top and bottom auto-scroll should use the same `40px` trigger distance and the same fixed `8px`-per-frame speed. The auto-scroll speed should not change based on how close the pointer or finger is to the edge. If the pointer or finger leaves the screen, the browser window loses focus, or the drag is otherwise interrupted before a completed drop, the implementation should cancel the reorder. Canceling an interrupted reorder should restore the square to its original position, remove the drag placeholder, stop reorder auto-scroll, write no browser storage, change no saved data, create no `Balance Changes` entry, and show no message.

The implementation should disable reordering while any `Saving` square is in action state, input state, or delete confirmation state. The user should close, cancel, save, or finish that state before any reorder can start.

Normal `Saving` square order should be saved in browser storage so the same normal square order appears after refresh. A normal square reorder should not repair, delete, or rewrite broken saved square data.

When the user finishes moving a `Saving` square and lets go, the website should save the final visible order in browser storage. Intermediate drag positions should not be saved as the source of truth.

After a reorder action finishes, coverage bars and the money amount shown inside `Savings` should be recalculated from the new visible order of normal `Saving` squares, skipping any broken `Saving` squares.

Reordering `Saving` squares should not change the current money amount and should not create a `Balance Changes` entry.

Coverage bars and `{money amount} needed` notes should be calculated from the current money amount and the ordered `Saving` square planned money amounts. They should not be stored as the source of truth.

Each `Saving` square coverage bar should be a thin horizontal bar at the bottom of the square in the square's default state.

The coverage bar fill should use width, not height. A 0% covered square should have a fully grey bar, a partly covered square should fill green from left to right by the covered percentage, and a 100% covered square should have a fully green bar.

When a `Saving` square is not fully covered, the `{money amount} needed` note should be rendered at the top-left of the bottom coverage bar. The note position should not depend on the width of the grey part of the bar, so it stays readable when the grey part is small.

The coverage calculation should:

- Start with the current money amount.
- Check `Saving` squares from top to bottom.
- Use the final visible order after any completed reorder action.
- Mark each square as fully covered, partly covered, or not covered.
- Reduce the remaining money amount used for display after each square.
- Stop the money amount shown inside `Savings` at `0.00$`.

The calculated money amount shown at the top of `Savings` should render with the exact user-facing label `Savings money amount`.

`Savings money amount` is display text only. It should not be stored as the source of truth for the calculated value.

The `Savings money amount` render state should not branch for `0.00$` except for the numeric text. The implementation should use the same component styling for `0.00$` and nonzero values, and should not render special warning or error colors, icons, badges, or messages only because the calculated value is zero.

The implementation should calculate a top needed note when total planned money amount in valid `Saving` squares is greater than the current money amount. The top needed note value should equal total planned money amount in valid `Saving` squares minus the current money amount, rendered as visible money amount text followed by ` needed`, such as `20.00$ needed`. The top needed note should render directly under `Savings money amount` in the fixed top `Savings` area. It should not render when total planned money amount in valid `Saving` squares is equal to or less than the current money amount.

While any `Saving` square create, rename, planned-money-amount change, or broken-square fix input flow is open, the implementation should calculate `Savings money amount`, the top needed note, and coverage bars from saved `Saving` squares only. Temporary input values should not be included until `Save` succeeds. `Cancel` should discard temporary values and leave the calculated display unchanged.

Changing the current money amount should update coverage bars and the money amount shown inside `Savings`, but it should not automatically change the saved planned money amounts inside `Saving` squares.

Changing a `Saving` square planned money amount should replace the saved planned money amount for that square with the new total planned money amount.

Changing a `Saving` square planned money amount should recalculate coverage bars, the top needed note, and the money amount shown inside `Savings`.

Changing a `Saving` square planned money amount should not create a `Balance Changes` entry and should not change the current money amount.

If a `Saving` square planned money amount is changed to `0.00$`, the website should remove that `Saving` square from saved browser storage instead of saving it with `0.00$`.

After removing a `Saving` square because its planned money amount became `0.00$`, the remaining `Saving` squares should keep their relative order. The website should recalculate the money amount shown inside `Savings`, the top needed note, and coverage bars from the remaining `Saving` squares.

Deleting a `Saving` square should remove only that square and its saved details from browser storage.

Deleting a `Saving` square should require user confirmation before deletion.

The confirmation message should be exactly `Delete this Saving?`, with buttons exactly named `Cancel` and `Delete`.

The confirmation should render as a small confirmation square in the middle of the screen. It should not render inside the `Saving` square, as a temporary square state, as a bottom sheet, or as a separate page. Only one `Saving` square delete confirmation should be open at a time.

Clicking `Cancel` or clicking or tapping outside the small confirmation square should close the confirmation without writing browser storage, changing saved data, creating a `Balance Changes` entry, or showing a message.

Deleting a `Saving` square should not change the current money amount, should not create a `Balance Changes` entry, and should not change the saved name or planned money amount of any other `Saving` square.

After deleting a `Saving` square, the remaining `Saving` squares should keep their relative order. The website should recalculate the money amount shown inside `Savings`, the top needed note, and coverage bars from the remaining `Saving` squares.

After a `Saving` square is deleted, the website should not offer undo.

## Browser Storage

The first version should save data in the user's browser storage.

The first version should not require:

- Email account.
- Login.
- Online sync.
- Server database.

The saved browser data should include:

- Money amount.
- Visible `Balance Changes` history for the last 30 days.
- `Saving` squares.

The default `0.00$` starting state is displayed when no saved data exists. It should not write browser storage just because the website opened.

Browser storage should be created after the first successful saved user action. In the normal first money flow, this is the first successful `Add`.

The first version has no website settings to save. Do not save a general settings item or an empty settings object.

The first version should not save temporary UI state, unsaved typed input, pending delete state, or pending reorder state in browser storage.

The first version should not implement a user-controlled reset, clear-all-data, or start-fresh action for normal valid saved data. The implementation should not render `Start again`, `Reset`, `Clear all data`, or similar all-data delete actions while saved data is valid.

The website should save data after every successful:

- `Add`.
- `Subtract`.
- `Modify`.
- `Saving` square change.
- `Balance Changes` delete that removes only the visible history entry.

A user action should count as successful only after the required browser storage write succeeds.

The implementation should listen for same-browser storage changes to the stable website data key from other open tabs or windows. When another tab or window successfully changes the saved data, the receiving tab or window should load and validate the latest saved data using the same normal saved-data load rules.

After a valid same-browser storage update from another tab or window, the receiving tab or window should replace its in-memory saved state with the latest saved data and rerender the visible money amount, `Balance Changes`, `Savings`, `Savings money amount`, top needed text, and coverage bars from that latest saved data.

The receiving tab or window should not write browser storage again only because it received a same-browser storage update. It should not create a `Balance Changes` entry, should not run a user action, and should show no user-facing message only because another tab or window saved a change.

If the receiving tab or window has a temporary UI open when a valid same-browser storage update arrives, the implementation should close that temporary UI like a cancel before replacing the displayed state. Any unsaved typed input in the receiving tab or window should be discarded, no pending delete should be confirmed, no reorder should be saved from that tab or window, no browser storage write should be made from that tab or window, and no `Balance Changes` entry should be created from that tab or window.

If browser storage is unavailable, full, blocked, or throws an error while saving a valid user action, the implementation should block that action and keep or restore the last successfully saved in-memory state. It should render the exact message `Changes could not be saved.`. It should not create, update, delete, or reorder saved data, should not create a `Balance Changes` entry, and should not run `Balance Changes` cleanup as a successful saved user action.

For main money amount input flows, a save failure should keep the same input flow open with the same accepted typed value. For `Saving` square create, rename, planned-money-amount change, and broken-square fix input flows, a save failure should keep the same input flow open with the same typed values. For `Saving` square delete and `Balance Changes` delete confirmations, a save failure should keep the confirmation open and keep the selected item visible. For completed `Saving` square reorder, a save failure should restore the last successfully saved order.

A `Modify` attempt with the same money amount that is already shown is not a successful saved user action and should not write browser storage or run `Balance Changes` cleanup.

Clicking `Yes` while the main money action input is `0.00` in `Add` or `Subtract` is not a successful action and should not write browser storage.

Trying to save a new `Saving` square with a planned money amount of `0.00$` or greater than `999,999.99$` is not a successful action and should not write browser storage.

When the user closes the website, refreshes the page, or opens the website again later in the same browser, the saved money amount and 30-day visible history should still be there.

The saved data only belongs to that browser on that device.

If the user opens the website on another phone, another computer, another browser, or private browsing mode, the saved data may not appear.

If the user clears browser data, clears site data, or uninstalls the browser, the saved website data may be deleted.

Browser storage is an internal implementation detail. The website should not tell the user where saved information is stored.

The website should not tell the user that saved information belongs to the same browser or device.

The website should not warn the user that saved information may disappear after changing browser, changing device, using private browsing, clearing browser data, clearing site data, or uninstalling the browser.

## Local Privacy Scope

The MVP does not need a PIN lock, website passcode, login, or local privacy feature because the website is not a real bank account and should not collect personal information.

## History Retention

Visible money change history should be kept for 30 days.

Visible `Balance Changes` entries should be displayed newest first by internal exact created date and time. Older changes should go lower in the list. If two entries have the exact same internal exact created date and time, entries earlier in the saved list order should render first. New entries should be saved before older entries in the saved list so the saved list order tie-breaker still keeps older changes lower.

After a successful `Add` or `Subtract` creates and saves a new `Balance Changes` entry, the implementation should set the internal `Balance Changes` scroll container to its top position so the newest entry is visible. This automatic scroll-to-top should not run for `Modify`, no-action attempts, failed save attempts, cleanup that removes old entries, or deleting a `Balance Changes` entry.

For visible `Balance Changes` history, one month means 30 days, not a calendar month.

The user should not choose dates for `Add` or `Subtract` entries.

The website should store a visible `Balance Changes` created date and created time for display using the user's browser/device local date and time. The rendered visible format should use the full month name, day, year, `at`, and 12-hour time with uppercase `AM` or `PM`, like `July 21, 2026 at 3:45 PM`. The website should also store an internal exact created date and time with seconds and milliseconds using the user's browser/device local date and time. The internal exact created date and time should be used for ordering and 30-day clearing only.

When a `Balance Changes` entry is created, its visible until date should be calculated as the internal exact created date and time plus 30 days using the user's browser/device local date and time.

The implementation should run `Balance Changes` cleanup when saved data is loaded while the website opens.

The implementation should also run `Balance Changes` cleanup after every successful saved user action.

During cleanup, any `Balance Changes` entry whose visible until date and time is at or before the current date and time should be deleted from browser storage and removed from the visible history list.

During saved-data load, the implementation should validate each saved `Balance Changes` entry before rendering it or running 30-day cleanup. A saved `Balance Changes` entry is valid only when it has a unique ID, action type `added` or `subtracted`, a valid money amount greater than `0.00` and not greater than `999999.99`, valid previous and new money amount values from `0.00` through `999999.99`, valid visible created date and visible created time values, a valid internal exact created date and time, and a valid visible until date and time. If duplicate IDs exist, the first matching saved entry in saved list order may stay if it is otherwise valid, and later matching entries should be treated as broken.

Broken saved `Balance Changes` entries should be removed from the loaded visible history and removed from browser storage during load cleanup. Removing a broken saved `Balance Changes` entry should not change the current money amount, should not affect `Savings`, should not show the full saved-data error, should not render a broken history row, and should show no user-facing message.

The first version does not need a background timer that checks old `Balance Changes` entries while the website stays open with no user action.

The current money amount should not change when old visible history entries are deleted.

## Data Safety

If saved browser data is broken or cannot be read, the website should not silently delete it.

The website should show this exact message:

`Saved data could not be loaded.`

The website should show a `Start again` action with that message.

When the user clicks `Start again`, the website should:

- Delete the broken saved data from browser storage.
- Immediately create fresh browser storage data with the saved money amount set to `0.00` and data version `1`.
- Use an empty `Balance Changes` list.
- Use an empty `Saving` squares list.

The implementation should expose `Start again` only from the broken or unreadable saved-data recovery state. It should not expose `Start again` or any normal all-data reset action when saved data is valid.

This full saved-data error should be used when the saved browser data file cannot be read, has an invalid top-level shape, has a missing current money amount, has a saved current money amount outside `0.00` through `999999.99`, has a saved current money amount that is not in normalized plain decimal format, or has an invalid data version. It should not be used only because one saved `Saving` square or one saved `Balance Changes` entry is broken while the rest of the saved browser data can still be read.

The website should use exactly one stable storage key for website data: `cash-money-organizer-website-data`.

The storage key should stay the same when the saved data version changes. Future upgrade logic should use the data version field inside the saved data instead of changing the storage key.

The website should include a data version number so future versions can upgrade old saved data safely. The first saved data version value should be `1`.

The first version should load saved browser data only when the data version value is exactly `1`.

If saved browser data has a missing, wrong, future, unreadable, or unrecognized data version, or a saved current money amount outside `0.00` through `999999.99` or not in normalized plain decimal format, the website should treat it as broken saved data. Saved current money amount values such as `5`, `5.0`, `005.00`, `5,000.00`, and `5.000` should be treated as broken saved data. The first version should not try to upgrade or guess how to read saved browser data with any other data version.
