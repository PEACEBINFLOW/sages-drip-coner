# START HERE — Sage's Drip Coner prompt kit

This kit lets you rebuild the missing files of your repo using fresh Claude chats, one batch per chat. Your repo already has README.md, docs/FILE_MANIFEST.md and docs/DATA_MODEL.md. The other 128 files in the manifest are missing; this kit covers every one of them (the kit's FILE_CHECKLIST.md proves it).

## The golden rules
1. ONE new chat per prompt file. Do them in order.
2. In each new chat, FIRST paste `01_SHARED_CONTEXT.md`, THEN attach the documents named at the top of that prompt, THEN paste the prompt.
3. Each batch ends with a "HANDOFF" block. Copy it into a notes file on your phone or computer. The next prompts tell you which HANDOFF blocks to paste back in. This is what keeps separate chats consistent.
4. Never skip P01. It writes the API contract that every other batch obeys. Commit its three files to GitHub (in `docs/`) before doing anything else.
5. Do NOT connect Cloudflare to GitHub until every file is committed. A half-empty repo fails to build.

## The order

| Step | File | What you get | Approx. files |
| --- | --- | --- | --- |
| 1 | P01 | API contract, architecture, admin guide (docs) | 3 |
| 2 | P02 | Config files and the database schema | 13 |
| 3 | P03a | Backend helpers and queries | 17 |
| 4 | P03b | Backend public routes | 11 |
| 5 | P03c | Backend admin routes and entry point | 11 |
| 6 | P04a | Frontend foundation and UI kit | 23 |
| 7 | P04b | Frontend components, layout, routes, styles | 17 |
| 8 | P05a | Public pages 1 | 7 |
| 9 | P05b | Public pages 2 | 6 |
| 10 | P06a | Admin components | 8 |
| 11 | P06b | Admin pages 1 | 5 |
| 12 | P06c | Admin pages 2 | 5 |
| 13 | P07 | Seed SQL files and final checklist | 2 |
| any | P08 | Fix a build error | — |

## How to put files on GitHub (per file)
1. In your repo: Add file → Create new file.
2. Type the exact path from the heading (for example `worker/lib/http.js`). Slashes create folders.
3. Copy ONLY what is inside the code block (not the heading, not the backticks).
4. Commit to `main`. Repeat.
Tip: you can commit several files quickly; GitHub never needs to rebuild anything until Cloudflare is connected.

## If a chat runs out of room
Type `continue`. Claude will resume at the next file. If your usage limit runs out mid-batch, start a fresh chat with the same shared context and the same prompt, and add this line at the end: "Files already done: <list the paths>. Produce only the remaining files." Keep the HANDOFF of the finished part.

## When everything is on GitHub
Go back to Claude with: "All files are committed, take me through Cloudflare." You already created the D1 database. Remember to:
- Put the real Database ID into `wrangler.jsonc` (the placeholder is clearly marked).
- Create the R2 bucket `sages-drip-coner-media`.
- Run `db/01_schema.sql`, then `db/02_seed_core.sql` in the D1 console.
- Connect GitHub with Workers Builds (Worker, not Pages), then add the two secrets.
The repo's README.md walks through this in detail (sections 3 to 9).

## Honest warning
Code written in separate chats can have small mismatches (a wrong import name, a missing file). That is normal. The HANDOFF notes reduce it a lot, and P08 exists to fix what remains using the Cloudflare build log. Expect to use P08 once or twice.
