# Global plan: Hermes Desktop, one action per file

Any AI continues this work by reading this file, then `LIS/STATUS.md`, then the one row in `LIS/INDEX.md` named there. Do not start by reading `apps/desktop/electron/main.ts`.

Fork: https://github.com/chandshy/hermes-agent
Branch: `research/desktop-action-study`
Parent: NousResearch/hermes-agent
This machine's running build stays on local commit `5307e932`. Do not reset that checkout onto the fork branch. The fork branch is the plan. The install is the app people are using.

## Goal

The desktop app keeps every verb it has now. Each verb lives in one file at the bottom of a purpose tree. Folders above that file only call those verbs and pass their results up. The top of the app is a shell that mounts purpose containers. It contains no verb of its own.

Minimal code means: if deleting a line does not stop the verb, the line does not belong in that file. No second implementation of the same verb.

## How an AI resumes

1. Read `LIS/STATUS.md`. It names the single action id in progress.
2. Read that id's row in `LIS/INDEX.md` and, if it exists, `LIS/actions/<id>.md`.
3. Read only the current files named on that row.
4. Do the one step STATUS allows: `name`, `map`, `split`, or `prove`.
5. Update the row, the action record, and STATUS in the same change.
6. Stop. One action id per session.

If STATUS and INDEX disagree, INDEX is the catalog and STATUS is the cursor. Fix the cursor. Do not invent a second catalog.

## Rules

- One action id is one exported function, `do`, in one file.
- A button, menu item, route, and IPC channel may all call that function. They are entries, not copies.
- A helper used by two actions moves to `shared/<name>/do.ts` and both actions call it. It gets its own id. It is not a utils folder.
- A container file may import and call. It may not branch on the verb's data or reimplement the call.
- An action file that grows a second verb is split before any new work.
- Tests for a verb sit beside `do.ts`. They test the verb, not the container.
- Do not delete a current file until every caller named in the action record points at `do` and the verb has been proven.
- Do not add a framework, registry, or plugin ABI. The containers are ordinary TypeScript modules.

## Layers, from the bottom

| Layer | What it is | Allowed contents |
|---|---|---|
| 0 Action | One verb | `do.ts`, `do.test.ts` |
| 1 Record | The lookup card for that verb | `LIS/actions/<id>.md` only. Not imported by the app. |
| 2 Subpurpose | A folder of related verbs | `index.ts` that calls those `do` functions and nothing else |
| 3 Purpose | A folder of subpurposes | `index.ts` that composes subpurpose indexes into one surface |
| 4 Shell | The application | Route table, IPC registration, window bootstrap. Imports purpose indexes only |

Moving back up is call order, not inheritance. `shell` calls `connect/index.ts`. That calls `connect/ssh/index.ts`. That calls `connect/ssh/open/do.ts`. The result returns up the same path.

## Target tree

New code goes here. Current code stays where it is until the action that owns it is in status `proven`.

```
apps/desktop/actions/
  talk/
    compose/          submit, attach, paste, pick-model
    transcript/       render, page, copy, retry
    session/          open, rename, archive
    index.ts
  look/
    preview/          open, watch, stop-watch
    artifact/         list, open, reveal
    files/            read-dir, read-text, write-text, rename, trash
    index.ts
  connect/
    ssh/              open, test, resolve-host
    local/            probe, start, stop
    cloud/            login, discover, logout
    registry/         list, save, remove, set-primary
    index.ts
  direct/
    toolset/          enable, disable
    skill/            install, remove
    plugin/           install, remove, probe
    model/            pick, hide
    profile/          get, set, rename, set-default
    index.ts
  schedule/
    cron/             create, pause, run, delete
    index.ts
  reach/
    messaging/        save-channel
    webhook/          create, delete
    notify/           show
    index.ts
  arrange/
    route/            open
    pane/             open, close
    window/           zoom, new-window, minimize
    hud/              open, move, close
    terminal/         start, write, resize, dispose
    git/              status, diff, stage, commit
    index.ts
  maintain/
    update/           check, apply
    boot/             get, repair, cancel
    index.ts
  shared/             only helpers that two actions already call
  shell/
    routes.ts         purpose → route
    ipc.ts            channel → do
    index.ts
```

Each deepest folder is one verb: `connect/ssh/open/do.ts`. The folder name is the verb. The purpose and subpurpose are the parents.

## Purpose map

These eight purposes cover the current window. A new purpose requires a verb that fits none of them, written into INDEX before any file is created.

| Purpose | Person's purpose | Current homes to mine |
|---|---|---|
| talk | Speak with the agent and keep the thread | `src/app/chat`, `src/app/session`, `src/components/assistant-ui`, `src/components/chat` |
| look | See a file, preview, or artifact | `src/app/artifacts`, `electron/fs-ipc.ts`, `electron/preview-*.ts`, `electron/gateway-file-download.ts` |
| connect | Reach a local, SSH, or cloud agent | `electron/connection-*.ts`, `electron/main.ts` connection handlers, `electron/cloud-*.ts`, `electron/backend-*.ts` |
| direct | Choose tools, skills, plugins, model, profile | `src/app/capabilities`, `src/app/profiles`, `src/app/settings`, `electron/desktop-plugin-*.ts`, `electron/desktop-profile.ts` |
| schedule | Run work later | `src/app/cron`, `src/api/cron.ts` |
| reach | Leave the chat through a channel, hook, or notification | `src/app/messaging`, `src/app/webhooks`, `electron/notification-*.ts` |
| arrange | Place panes, windows, terminal, git review | `src/app/shell`, `src/components/pane-shell`, `electron/git-*.ts`, `electron/terminal-ipc.ts`, `electron/hud-*.ts`, `electron/window-controls` in `main.ts` |
| maintain | Know the version and replace the app | `src/app/updates-overlay.tsx`, `electron/bootstrap-*.ts`, `electron/app-updater.ts`, `scripts/desktop-update/` |

`src/app/routes.ts` is the route list the shell will replace. It is not a purpose.

Electron IPC channels already gathered are assigned in `LIS/INDEX.md`. A channel missing from that index is unassigned. Assign it before writing code for it.

## Known collisions

These are the same verb under two names. The mapping step picks one `do` and records the other name as an alias. Aliases contain no logic.

| One verb | Names that must collapse |
|---|---|
| Test a connection | `hermes:connections:test`, `hermes:connection-config:test` |
| Save a connection | `hermes:connections:save`, `hermes:connection-config:save` |
| Apply a connection | `hermes:connection-config:apply`, `hermes:connection` |
| Gateway websocket URL | `hermes:gateway:ws-url`, `hermes:gateway:ws-url-for` |
| Open a session window | `hermes:window:openSession`, route `/` plus a session id |

Search for more collisions only inside the action being mapped. Do not sweep the whole tree for duplicates in one session.

## LIS

LIS here is this project's library index. It is not the SE2 query API. Its job is the same: answer from the index, then open one record, and only then open code.

| File | Role |
|---|---|
| `LIS/STATUS.md` | Cursor. One action id, one allowed step, what finished last |
| `LIS/INDEX.md` | Every purpose, subpurpose, and action id. The uniqueness check |
| `LIS/RECORD.md` | The blank card. Copy it to `LIS/actions/<id>.md` when an id moves from `named` to `mapped` |
| `LIS/actions/<id>.md` | One verb: entries, current files, `do` path, calls, aliases |

Status values, in order: `named` → `mapped` → `split` → `proven`.

- `named` — id, purpose, and current homes are in INDEX. No card yet.
- `mapped` — card lists every current caller found for that id. No code moved.
- `split` — `do.ts` exists and every caller uses it. Old bodies deleted or reduced to a call.
- `proven` — the verb was exercised from the UI entry, and the card says what was clicked.

A session may advance an id by one status only.

## Work loop

The first id is `connect.ssh.open`. After it is `proven`, take the next `named` id in INDEX order. INDEX order is the order of the tables.

For `map`:

1. Find the IPC channels and UI entries on the row.
2. List the functions they call, with file paths. Stop at the process or network edge.
3. Write `LIS/actions/<id>.md` from `LIS/RECORD.md`.
4. Set the row to `mapped`.

For `split`:

1. Create `apps/desktop/actions/<purpose>/<subpurpose>/<verb>/do.ts` with the body from the card.
2. Point each current caller at `do`.
3. Delete the old body.
4. Set the row to `split`.

For `prove`:

1. Run the UI entry named on the card.
2. Record the date and what happened on the card.
3. Set the row to `proven`.

## Done

The plan is finished when every INDEX row is `proven`, `shell/index.ts` is the only top-level wiring, and `electron/main.ts` no longer contains a verb. The running app still performs the same routes and channels it does now.

Until then, the app people use stays the install under `/mnt/data/Hermes-Desktop`. Action files land on `research/desktop-action-study` and are not copied into the running build until a purpose container is `proven` and a session is told to rebuild.

## Do not

- Do not reorganize a purpose ahead of its first action.
- Do not create empty action folders "for later."
- Do not add a second document that repeats INDEX.
- Do not move the Python agent, the TUI, or `apps/web` as part of a desktop verb.
- Do not treat install, virtualenv, or browser-engine setup as part of Connect. Those stay under `maintain` and only if Update's card says the verb calls them.
