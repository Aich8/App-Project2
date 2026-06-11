# Cash Money Organizer

Cash Money Organizer is a browser-based personal cash tracking website. It is not a real bank account and does not collect personal information.

## Project Structure

- `website/`: Next.js website application.
- `website/app/`: app pages, layout, and global CSS.
- `website/public/`: static website assets.
- `docs/specs/`: all specifications.
- `docs/specs/business/`: business requirements.
- `docs/specs/functional/`: user-facing behavior and flows.
- `docs/specs/technical/`: implementation and storage rules.
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
