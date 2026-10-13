# Main Money Amount Visual Design Plan

Status: In progress

The main money amount appears in the top-center area of the dashboard.

## Background And Container

- The dashboard background behind the main money amount is `#FFFFFF` white.
- The main money amount appears directly on the page instead of inside a separate square or card.
- It has no surrounding border, shadow, or corner rounding.

## Top Space

- On phones, the top of `Current Balance` is `32px` below the top edge of the usable dashboard.
- On desktop, the top of `Current Balance` is `48px` below the top edge of the usable dashboard.

## Position And Typography

- The money amount is centered.
- It uses the largest font size used anywhere on the website.
- Its font size starts at `40px` on narrow phones, grows naturally as the available screen space increases, and stops growing at `64px` on desktop.
- Its font weight is `700` bold.
- Its text color is black.
- The exact text `Current Balance` appears directly above it.
- `Current Balance` is centered and uses a `14px` font size.
- Its font weight is `500` medium.
- Its text color is the muted grey `#66706C`.
- There is `8px` of space between `Current Balance` and the money amount.

## Width And Side Space

- The width is an invisible content limit; it does not create a visible box around the main money amount.
- On phones, there is `16px` between the main money amount area and each side of the screen.
- Between phone and desktop sizes, the area grows naturally with the available space.
- On desktop, the area is centered horizontally and stops growing at a maximum width of `640px`, matching the maximum width of the `Balance Changes` square.

## Area Height

- The main money amount area has no fixed height.
- Its height grows naturally to contain `Current Balance`, the money amount, and the main money action buttons when those buttons are open.
- It does not keep a large empty box around the content when the action buttons are closed.

## Amount Readability

- The complete displayed money amount stays centered on one line.
- A money amount such as `999,999.99$` never splits across two lines or gets cut off.
- The approved responsive size remains `40px` through `64px` on normal supported phone and desktop widths.
- On an unusually narrow screen, the amount may become only as much smaller than `40px` as necessary to remain completely visible without horizontal page overflow.

## Spacing Before Balance Changes

- The approved `32px` space before the `Balance Changes` title begins below the last visible item in the main money amount area.
- When the main money action buttons are closed, the `32px` begins below the money amount.
- When the main money action buttons are open, the `32px` begins below the buttons.
- Opening the buttons moves `Balance Changes` downward instead of allowing the buttons to cover it.
- Closing the buttons returns `Balance Changes` to its normal position without keeping permanent empty button space.

The detailed design of the main money action buttons and other content below the money amount will be decided later.

The accepted specifications remain the source of truth for how the website behaves.
