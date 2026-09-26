# Status

Updated: 2026-09-26

## Cursor

- Action id: `connect.ssh.open`
- Allowed step: `map`
- Do not create `do.ts` in this next session. Write the card only.

## Last finished step

The global plan and this index were written. No application code has been moved. Every action id in `INDEX.md` is `named`.

## Where the words live

- Plan: `research/desktop-action-study/PLAN.md`
- Catalog: `research/desktop-action-study/LIS/INDEX.md`
- Card template: `research/desktop-action-study/LIS/RECORD.md`
- Published fork branch: `research/desktop-action-study` on https://github.com/chandshy/hermes-agent
- Published tip when this status was written: `67f2bce4bdd313336295a065c7622d2046515f23` plus the plan commit that follows it

## Running install

`/mnt/data/Hermes-Desktop` is the app in use. Its desktop build stamp is commit `5307e932`, built 2026-09-25 11:34 ET. Leave that checkout on its current files except for documents under `research/desktop-action-study/`. Do not reset it to the fork's newer `main`.

## Next session does this

1. Read `PLAN.md` and this file.
2. Map `connect.ssh.open` from `electron/connection-apply.ts`, `electron/connection-config.ts`, `electron/managed-ssh-update.ts`, and the `hermes:connection*` handlers in `electron/main.ts`.
3. Add `LIS/actions/connect.ssh.open.md` using `RECORD.md`.
4. Set that INDEX row to `mapped` and move this cursor to `split` for the same id.
5. Stop.
