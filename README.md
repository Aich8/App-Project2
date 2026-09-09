# Cash Money Organizer

Cash Money Organizer is a browser-based personal cash tracking website. It is not a real bank account and does not collect personal information.

## Project Structure

- `website/`: Next.js website application.
- `website/app/`: app pages, layout, and global CSS.
- `website/public/`: static website assets.
- `docs/specs/`: specification rules and indexes.
- `docs/specs/accepted/`: approved global, feature, and technical specifications.
- `docs/specs/drafts/`: specifications waiting for explicit user approval.
- `docs/raw-data/`: non-authoritative notes, references, legacy documents, and temporary information.
- `docs/plans/active/`: active implementation plan.
- `AGENTS.md`: project instructions, source-of-truth paths, and GitFlow.

## Run The Website

```powershell
cd website
npm run dev
```

Then open `http://localhost:3000`.

## GitFlow

This project uses one branch: `main`.

Work is committed directly on `main` and pushed to GitHub after each requested commit.
