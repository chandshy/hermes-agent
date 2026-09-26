# Desktop action study

Research branch on the fork of NousResearch/hermes-agent.
Question: how should Hermes Desktop be organized so each user action is a small, obvious piece of code?

This phase studies. It does not restructure the app yet.

## Method

An action is a verb a person can name without knowing the code: send a message, open a file, connect to a host, schedule a job.

For each action, record only:

1. The entry the person uses (route, command, or key).
2. The one module that performs it.
3. The calls that module must make to finish the verb.
4. Everything else that currently runs, marked as not on the path.

A line stays in the study only when removing it stops the verb. Shared code is allowed when two recorded actions need the same call. A new framework, registry, or install path is out of scope until an action fails without it.

## Measured at the start

Checkout `5307e932` on `main`, 2026-09-26. Desktop TypeScript is about 114,000 lines across 3,081 files. `apps/desktop/electron/main.ts` is 18,716 lines. Electron exposes 208 IPC handlers across 17 files. The install tree on this machine is about 3.7 GB.

The window has 13 built-in routes (`apps/desktop/src/app/routes.ts`):

| Route | Verb it should be |
|---|---|
| `/` | Talk in a chat |
| `/settings` | Change a setting |
| `/command-center` | See sessions, usage, system |
| `/capabilities` | Turn a tool on or off |
| `/messaging` | Talk through a channel |
| `/webhooks` | Receive an external event |
| `/artifacts` | Look at a produced file |
| `/cron` | Schedule a job |
| `/profiles` | Switch persona or home |
| `/agents` | See or start an agent |
| `/starmap` | See how agents relate |
| `/session-import` | Bring in an old transcript |

Command palette adds window, update, reload, and browser actions on top of those routes.

## What the last install showed

This machine only displays Hermes and runs the agent on another host over SSH. The update still required a local git checkout, a Python runtime, Node, a browser engine, and a local Electron rebuild. Three launchers existed at once: `hermes-agent/venv/bin/hermes` (Python 3.11, left by an installer that ran against the wrong lockfile), `.hermes/bin/hermes` (Python 3.14, the one `hermes` actually runs), and `~/.local/bin/hermes` (a wrapper to the second). The desktop package version on this commit is `0.0.0`. The build stamp's `displayVersion` is null.

Those facts are the reason for the study. The connect verb worked. The code around it did not stay small.

## First action: Connect

Entry: Settings → Connections, or the saved primary `nucbox` at launch.

Code that performs it:

- Renderer connection settings and the connection store.
- IPC `hermes:connections:*` and `hermes:connection-config:*` in `electron/main.ts` (about line 14909 onward).
- SSH session setup beside that, including `electron` SSH lifecycle modules.

Code that ran anyway during "just connect":

- Installer checkout of `main`, virtualenv creation, Node and browser install.
- Desktop pack (`npm run pack` / the products stage).
- Update hand-off (`scripts/desktop-update/posix.sh`), which refuses to start unless a local launcher exists.

The study claim to test next: Connect needs the SSH modules and the connection IPC, and does not need the installer, the local virtualenv, or a rebuild of the window.

## Later actions, in order

Study one verb per note. Do not start the next until the current note names the module and the lines that are not on the path.

1. Connect (this note's claim).
2. Talk: send a message and see the stream.
3. Look: open a file or artifact beside the chat.
4. Schedule: create a cron job.
5. Update: replace the window without installing an agent.

## Organization to test after the notes

One directory per verb under `apps/desktop`, named for the verb, containing its renderer entry and its Electron handler. `main.ts` becomes the list of those handlers and nothing else. The agent checkout stays a separate program the Connect verb can point at. It is not a directory the window must live inside.

## Out of scope

Rewriting the agent, the TUI, or the web dashboard. Visual redesign. New features. Deleting upstream code on this branch before a note shows the verb still works without it.
