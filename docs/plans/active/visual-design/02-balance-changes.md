# Balance Changes Visual Design Plan

Status: Complete

This file records the completed visual design of `Balance Changes`.

The accepted specifications remain the source of truth for how the website behaves.

`Balance Changes` appears directly under the main money amount as a large square. Most of the dashboard space below the main money amount is dedicated to this square.

## Large Square

- The background color is `#F5F6F4`.
- The corners are sharp.
- The square has a `1px solid #D9DDDA` border.
- The square has no shadow.
- On phones, it is a true square that uses the available width between `16px` left and right page margins.
- Between phone and desktop sizes, the square grows naturally with the available space.
- On desktop, it remains a true square, is centered horizontally, and stops growing at a maximum size of `640px` by `640px`.

## Title

- The exact title is `Balance Changes`.
- The title appears outside and above the square.
- It aligns with the square's left edge on phones and desktop.
- The title is black.
- Its font size is `28px`, making it large but still smaller than the main money amount.
- Its font weight is `600` semi-bold.

## Position And Spacing On Phones

- There is `32px` between the bottom of the main money amount area and the title.
- There is `12px` between the title and the square.
- There is `16px` between each side of the square and the corresponding edge of the phone screen.
- There is `24px` below the square before the next dashboard content.

## Position And Spacing On Desktop

- The phone's initial vertical spacing values remain the starting values on desktop: `32px` from the main money amount area to the title, `12px` from the title to the square, and `24px` below the square.
- The empty space on the left and right keeps the desktop layout centered and prevents entries from stretching across a wide screen.

## Entry List Spacing

- On phones and desktop, there is `12px` of space inside the square between its top border and the first entry.

When there are no entries, the inside of the square stays blank. It does not show an empty-state sentence, placeholder, or icon.

## Scrolling

- When there are too many entries to fit, the entries scroll vertically inside the `Balance Changes` square.
- During scrolling, entry squares move only straight up and down. They do not move, drift, or wobble from side to side.
- There is no horizontal scrolling.
- Scrolling is free and natural, without snapping from one entry to the next.
- The normal scrollbar supplied by the user's browser and device is used instead of a specially designed scrollbar.
- On phones, the device's small scroll indicator appears while the user is scrolling and disappears afterward.
- There is `12px` of empty space after the final entry, matching the `12px` above the first entry.
- After reaching the final entry, continuing to scroll moves the dashboard page toward `Savings` below the square.
- After a successful new Add or Subtract entry is created or received from another open tab or window, the entries return to the top immediately so the newest entry is visible. This movement is not animated.
- No scrolling arrows or fading-edge effects are added.
- The dashboard page itself can still scroll past the bottom of the square so the user can reach `Savings`.

## Individual Entries

- Each entry has a `#FFFFFF` background.
- Each entry has `16px` corner rounding.
- Each entry has a `1px solid #E0E4E1` border.
- Entries have no divider lines and no shadows.
- There is `8px` of empty space between entries.
- Entries stay `12px` away from the left and right inside edges of the outer `Balance Changes` square.
- Each entry has a minimum height of `88px` and can grow taller when text needs to wrap.
- Inside each entry, there is `12px` of space above and below the content and `16px` on the left and right.

Each entry is compact:

- The signed money amount and action text appear in the top-left.
- An Add entry uses text such as `+50.00$ added`.
- In an Add entry, the signed money amount such as `+50.00$` uses the dark-green color `#1F7A4D`, an `18px` font size, and a `600` semi-bold font weight.
- The Add money amount has no colored background, colored side line, or separate plus icon because the required amount text already begins with `+`.
- There is `8px` of space between the signed Add money amount and the word `added`.
- In an Add entry, the word `added` is black, uses a `16px` font size, and has a `500` medium font weight.
- A Subtract entry uses text such as `-50.00$ subtracted`.
- In a Subtract entry, the signed money amount such as `-50.00$` uses the dark-red color `#B33A3A`, an `18px` font size, and a `600` semi-bold font weight.
- The Subtract money amount has no colored background, colored side line, or separate minus icon because the required amount text already begins with `-`.
- There is `8px` of space between the signed Subtract money amount and the word `subtracted`.
- In a Subtract entry, the word `subtracted` is black, uses a `16px` font size, and has a `500` medium font weight.
- The date and time appear in the bottom-right, below the money change text and not too far away from it.
- The date and time use the muted-grey color `#66706C`, a `14px` font size, a `400` normal font weight, and a `20px` line height.
- The date and time are right-aligned with at least `12px` of space above them.
- The date and time have no icon, background color, or border.
- On narrow phones, the date and time may wrap naturally onto additional right-aligned lines, and the entry grows taller than its `88px` minimum when needed.
- The date and time are never made smaller, cut off, or replaced with an ellipsis to force them onto one line.
- Add and Subtract entries remain separate.
- The newest entry appears first.

## Hovered, Pressed, And Held Appearance

- A normal entry keeps its white background and normal light border.
- On desktop, hovering the mouse pointer over an entry changes its background to `#F8FAF8` and its border to `#C8D0CA`.
- A hovered entry uses a hand pointer and a `120ms` color transition.
- Hovering does not add a shadow, move or resize the entry, or show extra text or a delete icon.
- While an entry is being pressed or held, its background changes to `#F3F5F3` and its border changes to `#AEB8B1`.
- The entry does not move, shrink, or gain a shadow.
- Canceling the hold immediately returns the entry to its normal appearance.
- After a completed hold, the selected entry stays lightly highlighted while its deletion controls are open.
- Closing the deletion controls returns the entry to its normal appearance.

Example entry:

```text
+50.00$ added

     October 6, 2026 at 3:45 PM
```

Phones use the same basic entry layout. Text may wrap only when needed to remain readable, while the money change stays in the top-left area and the date and time stay in the bottom-right area.

## Delete Controls

Holding an entry for `600ms` opens a small square in the middle of the screen containing `Cancel` and `Delete`. Choosing `Delete` then opens the final centered deletion confirmation.

When either set of delete controls is open:

- A transparent black layer with `28%` darkness slightly darkens the page behind it.
- The page is not blurred.
- The selected entry remains visible and lightly highlighted underneath.

The first `Cancel` and `Delete` square uses:

- A size of `240px` by `240px`.
- The exact center of the screen.
- A `#FFFFFF` background.
- `16px` corner rounding.
- A `1px solid #D9DDDA` border.
- No shadow.
- `24px` of inner padding.

Its complete two-button row is centered horizontally and vertically inside the square. The buttons sit beside each other: `Cancel` is on the left, `Delete` is on the right, both buttons have the same width, and there is `12px` between them.

The first `Delete` button uses:

- A light-red `#FDECEC` background.
- Dark-red `#B33A3A` text.
- A `16px` font size and `600` semi-bold font weight.
- A height of `48px` and `12px` corner rounding.
- No delete icon.

The `Cancel` button uses:

- A `#F5F6F4` background.
- Black text.
- A `1px solid #D9DDDA` border.
- A `16px` font size and `500` medium font weight.
- A height of `48px` and `12px` corner rounding.

The final confirmation uses:

- The exact centered question `Delete this Balance Change?`.
- A maximum width of `320px`, with at least `16px` between it and each screen edge on smaller phones.
- A height that grows naturally with its content.
- A `#FFFFFF` background.
- `16px` corner rounding.
- A `1px solid #D9DDDA` border.
- No shadow.
- `24px` of inner padding.

The confirmation question is centered, black, `20px`, and `600` semi-bold. When there is no failure message, there is `24px` between the question and the button row. Its buttons sit beside each other, use the same width, and have `12px` between them. `Cancel` is on the left and uses the neutral design. `Delete` is on the right and uses a solid `#B33A3A` background with white text. The final `Delete` text is `16px` with a `600` semi-bold font weight, and its button has `12px` corner rounding and no border. Both buttons are `48px` tall.

On desktop, hovering changes the neutral button background to `#E9ECEA`, the first light-red `Delete` background to `#F9DCDC`, and the final solid-red `Delete` background to `#9F3030`. Pressing makes a button slightly darker without moving it, shrinking it, or adding a shadow. These color changes use a `120ms` transition.

If deletion cannot be saved, the exact message `Changes could not be saved.` appears centered below the question and above the buttons. It uses `#B33A3A`, a `14px` font size, and a `500` medium font weight, with `12px` of space above and below it. The confirmation grows taller to fit the message instead of squeezing or cutting off its content.

After the final `Delete` is chosen and while the deletion is being saved:

- The confirmation remains open.
- The final `Delete` button is disabled and appears slightly faded so it cannot look ready for another press.
- Its exact text remains `Delete`; it does not show a spinner or different text.
- `Cancel` remains available and keeps its normal appearance.
- If saving fails, the final `Delete` button returns to its normal appearance and the failure message is shown.

The same delete-control design and dimensions are used on phones and desktop. The controls stay small and centered instead of growing on a large screen.

## Movement And Animation

- Opening or closing delete controls fades the darkened background and centered control in or out over `120ms`.
- The centered control remains still during the fade. It does not slide, grow, shrink, or rotate.
- Moving from the first delete-control square to the final confirmation uses a `120ms` cross-fade. The first control fades out and the confirmation fades in at its final size in the same centered position.
- A new Add or Subtract entry appears immediately, and the entries return to the top immediately. The new entry does not fade, slide, flash, or pulse.
- After a successful deletion, the deleted entry fades away and its empty space closes over `160ms`. Entries below it move only straight upward into the available space and never move sideways.
- A successful deletion does not return the entries to the top.
- An entry that reaches its exact 30-day expiration disappears immediately without a fade. The remaining entries take their new vertical positions immediately.
- The failure message `Changes could not be saved.` appears immediately without fading, shaking, flashing, or pulsing. It disappears immediately when a retry begins or the confirmation closes.
- If the user's device requests reduced motion, delete-control fades and the successful-deletion closing animation are disabled. These changes then happen immediately, while normal user-controlled finger, mouse-wheel, and trackpad scrolling remains available.
- Movement never bounces, zooms, shakes, or travels sideways. Decorative animation is not used.

The only planned animation is the existing `120ms` color change, the `120ms` delete-control fade, and the `160ms` straight vertical closing movement after a successful deletion. These short effects soften visual changes without making the user wait.
