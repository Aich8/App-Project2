# Functional Spec: Cash Money Organizer Website

## Quick Add and Subtract Controls

Under the main cash balance, the website should show two small icon actions:

- `+` for adding money to the main amount.
- `-` for subtracting money from the main amount.

These controls are manual balance actions. They do not represent a real bank deposit, bank withdrawal, or payment.

When the user taps the `+` icon:

- The website asks for the amount to add.
- The entered amount is added to the main cash balance.
- The action is saved in the balance history as a positive adjustment.

When the user taps the `-` icon:

- The website asks for the amount to subtract.
- The entered amount is subtracted from the main cash balance.
- The action is saved in the balance history as a negative adjustment.

Each add or subtract action should include:

- Amount.
- Date.
- Action type: added or subtracted.
- Optional note.

Each modification should include:

- New total amount.
- Date.
- Optional note.

Each entry should include:

- Previous amount.
- New amount.
- Difference.
- Date.
- Optional note.

Each cash category or envelope should include:

- Envelope name.
- Current assigned amount.
- Optional color or icon.
- Optional short description.

The envelope view should show the user how the current cash balance is divided. It should make clear which money is assigned to categories and which money is still unassigned.

Changing an envelope amount should be recorded as an envelope adjustment. Moving money between envelopes should not change the main cash balance.

The user should be able to edit or delete entries when they make a mistake.

The `+` and `-` icons should appear visually connected to the main amount so the user understands they directly change the displayed balance.
