<!-- BEGIN:nextjs-agent-rules -->
# This is NOT the Next.js you know

This version has breaking changes — APIs, conventions, and file structure may all differ from your training data. Read the relevant guide in `node_modules/next/dist/docs/` before writing any code. Heed deprecation notices.
<!-- END:nextjs-agent-rules -->

## GitFlow

- Use a single branch only: `main`.
- Do not create feature branches, release branches, or temporary work branches unless the user explicitly changes this rule.
- Before making Git changes, confirm the working branch is `main`.
- When the user asks for a commit, stage the relevant changes, create the commit on `main`, and push `main` to GitHub.
- Do not use `Accept all incoming changes` as a conflict strategy because it can overwrite local work.
- If Git reports conflicts, resolve the files by preserving the user's intended local work and the needed incoming work before continuing.
- Keep the Git process simple: edit files, test when relevant, commit on `main`, push `main`.
