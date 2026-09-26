id: connect.ssh.open
status: mapped
purpose: connect
subpurpose: ssh
verb: Open an SSH session to the selected host and bring up its remote Hermes dashboard.
entries:
  - App launch when the saved primary connection is SSH (`main.ts` startup calls `connectDesktopProfileRoute`)
  - `window.hermesDesktop.getConnection` → `hermes:connection` (`electron/preload.ts`)
  - `window.hermesDesktop.getConnectionFor` → `hermes:connection:for` (`electron/preload.ts`)
  - Renderer dials in `src/store/gateway.ts` (`dialProfile`) and `src/store/profile.ts`
current:
  - `apps/desktop/electron/main.ts` — `connectDesktopProfileRoute` (line 14919)
  - `apps/desktop/electron/main.ts` — `ensureBackend` (line 10623) when the route is remote SSH
  - `apps/desktop/electron/main.ts` — `ensureRegistryBackend` (line 10738) when `source.kind === 'ssh'`
  - `apps/desktop/electron/main.ts` — `resolveRemoteBackend` (line 10086)
  - `apps/desktop/electron/main.ts` — `bootstrapSshConnection` (line 9775)
  - `apps/desktop/electron/main.ts` — `bootstrapSshConnectionInner` (line 9861)
  - `apps/desktop/electron/ssh-connection.ts` — `SshConnection.open` (line 762)
  - `apps/desktop/electron/remote-lifecycle.ts` — `connect` (line 1463)
do: apps/desktop/actions/connect/ssh/open/do.ts
calls:
  - external edge: system `ssh` ControlMaster (`SshConnection.open`)
  - external edge: remote `hermes` dashboard (`remoteLifecycle.connect` or `connectWindowsRemote`)
  - `connect.registry.list` (`readDesktopConnectionsRegistry`) when the dial carries a connection id
  - external edge: `ssh -G` via `effectiveSshConfigFingerprint` before bootstrap
aliases:
  - `hermes:connection` and `hermes:connection:for` both call `connectDesktopProfileRoute` and must become calls of `do` with no logic of their own
not_on_path:
  - `electron/connection-apply.ts` — writes config and tears the old backend down; the next dial opens SSH
  - `electron/managed-ssh-update.ts` and `hermes:connections:update-managed` / `hermes:connections:update-all` — update a remote install
  - `hermes:connection-config:apply` — save and re-home, then the renderer dials again
  - `hermes:connection:revalidate` — probe a connection that is already open
  - `hermes:gateway:ws-url` and `hermes:gateway:ws-url-for` — read a URL after the dashboard exists
  - `hermes:get-remote-display-reason` — display copy, not the session
  - Installer, local virtualenv, Node, and the Electron rebuild
  - Local backend spawn inside `ensureBackend` when the route is not SSH
proven:
