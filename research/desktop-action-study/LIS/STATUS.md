# Status

Updated: 2026-09-26

## Cursor

- Action id: `connect.ssh.open`
- Allowed step: `split`
- Create `apps/desktop/actions/connect/ssh/open/do.ts` from the card. Point `hermes:connection` and `hermes:connection:for` at it. Do not pull in the channels listed under `not_on_path`.

## Last finished step

`connect.ssh.open` is `mapped`. Card: `LIS/actions/connect.ssh.open.md`. No application code has been moved. Every other action id is still `named`.

Channels the index had hung on this verb were detached and marked `unassigned`: `hermes:connections:update-managed`, `hermes:connections:update-all`, `hermes:connection-config:apply`, `hermes:gateway:ws-url`, `hermes:gateway:ws-url-for`, `hermes:get-remote-display-reason`.

## Where the words live

- Plan: `research/desktop-action-study/PLAN.md`
- Catalog: `research/desktop-action-study/LIS/INDEX.md`
- Card template: `research/desktop-action-study/LIS/RECORD.md`
- Published fork branch: `research/desktop-action-study` on https://github.com/chandshy/hermes-agent
- Published tip when this status was written: `67f2bce4bdd313336295a065c7622d2046515f23` plus the plan commit that follows it

## Running install

`/mnt/data/Hermes-Desktop` is the app in use. Its desktop build stamp is commit `5307e932`, built 2026-09-25 11:34 ET. Leave that checkout on its current files except for documents under `research/desktop-action-study/`. Do not reset it to the fork's newer `main`.

## Next session does this

1. Read `PLAN.md`, this file, and `LIS/actions/connect.ssh.open.md`.
2. Split only that verb into `apps/desktop/actions/connect/ssh/open/do.ts`.
3. Leave `ensureBackend`'s local-spawn branch where it is. `do` is the SSH bootstrap: `bootstrapSshConnectionInner` through `SshConnection.open` and `remoteLifecycle.connect`.
4. Set the INDEX row to `split` and move this cursor to `prove` for the same id.
5. Stop. Do not click through the UI in that session.
