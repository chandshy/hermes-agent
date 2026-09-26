# LIS index

Uniqueness: an action id appears once. A channel appears once. If a channel is listed twice, stop and fix this file before writing code.

Status of every row below is `named` until STATUS says otherwise. Current homes are where the code sits today, not the target path.

Target path for an id `purpose.subpurpose.verb` is:

`apps/desktop/actions/<purpose>/<subpurpose>/<verb>/do.ts`

## Purposes

| Purpose | Subpurposes |
|---|---|
| talk | compose, transcript, session |
| look | preview, artifact, files |
| connect | ssh, local, cloud, registry |
| direct | toolset, skill, plugin, model, profile |
| schedule | cron |
| reach | messaging, webhook, notify |
| arrange | route, pane, window, hud, terminal, git |
| maintain | update, boot |

## Actions

### talk

| Id | Verb | Current home |
|---|---|---|
| talk.compose.submit | Send the draft | `src/app/chat`, `src/components/assistant-ui` |
| talk.compose.attach | Attach a file to the draft | `hermes:readFileDataUrlForAttach`, `hermes:selectPaths` |
| talk.compose.paste | Paste text or an image | `electron/composer-paste.ts`, `hermes:savePastedText`, `hermes:saveClipboardImage` |
| talk.transcript.page | Load older turns | `src/api/sessions.ts` |
| talk.transcript.copy | Copy a turn | renderer clipboard call into `hermes:writeClipboard` |
| talk.session.open | Open a stored chat | `src/app/open-session.ts`, `hermes:window:openSession` |
| talk.session.rename | Rename a chat | session API in `src/api/sessions.ts` |
| talk.session.archive | Archive a chat | session API in `src/api/sessions.ts` |

### look

| Id | Verb | Current home |
|---|---|---|
| look.preview.open | Open a preview target | `hermes:normalizePreviewTarget`, `hermes:preview:reach`, `electron/preview-reach.ts` |
| look.preview.watch | Watch a preview file | `hermes:watchPreviewFile` |
| look.preview.stop | Stop watching a preview file | `hermes:stopPreviewFileWatch` |
| look.preview.capture | Capture the preview | `hermes:capturePreview` |
| look.artifact.reveal | Show a file in the folder | `hermes:fs:reveal` |
| look.files.readDir | List a directory | `hermes:fs:readDir`, `electron/fs-read-dir.ts` |
| look.files.readText | Read a text file | `hermes:readFileText` |
| look.files.writeText | Write a text file | `hermes:fs:writeText` |
| look.files.rename | Rename a file | `hermes:fs:rename` |
| look.files.trash | Move a file to trash | `hermes:fs:trash` |
| look.files.openDir | Open a directory | `hermes:fs:openDir` |
| look.files.saveDownload | Save a gateway file | `hermes:saveGatewayFile`, `electron/gateway-file-download.ts` |

### connect

| Id | Verb | Current home |
|---|---|---|
| connect.ssh.open | Open the SSH session to the selected host | `electron/connection-apply.ts`, `electron/managed-ssh-update.ts`, `hermes:connection` |
| connect.ssh.test | Test that a host answers | `hermes:connections:test`, `hermes:connection-config:test` |
| connect.ssh.resolveHost | Resolve an SSH config host | `hermes:ssh-config:resolve`, `hermes:ssh-config:hosts` |
| connect.local.probe | See if a local backend is there | `hermes:local-backend:probe`, `electron/backend-probes.ts` |
| connect.local.touch | Keep the local backend alive | `hermes:backend:touch` |
| connect.local.recycle | Restart the local backend | `hermes:backend:recycle` |
| connect.cloud.login | Sign in to cloud | `hermes:cloud:login` |
| connect.cloud.discover | List cloud agents | `hermes:cloud:discover` |
| connect.cloud.logout | Sign out of cloud | `hermes:cloud:logout` |
| connect.cloud.status | Read cloud sign-in | `hermes:cloud:status` |
| connect.registry.list | List saved connections | `hermes:connections:list`, `electron/connection-registry.ts` |
| connect.registry.save | Save a connection | `hermes:connections:save`, `hermes:connection-config:save` |
| connect.registry.remove | Remove a connection | `hermes:connections:remove` |
| connect.registry.setPrimary | Choose the startup connection | `hermes:connections:set-primary` |
| connect.registry.setLaunchMode | Choose last-used or primary at launch | `hermes:connections:set-launch-mode` |

### direct

| Id | Verb | Current home |
|---|---|---|
| direct.toolset.enable | Turn a toolset on | `src/app/capabilities/toolsets`, `src/api/toolsets.ts` |
| direct.toolset.disable | Turn a toolset off | same |
| direct.skill.install | Install a skill | `src/app/capabilities/skills`, `src/api/skills.ts` |
| direct.skill.remove | Remove a skill | same |
| direct.plugin.install | Install a desktop plugin | `hermes:plugin:installDesktop`, `electron/desktop-plugin-install.ts` |
| direct.plugin.remove | Remove a desktop plugin | `hermes:plugin:removeDesktop` |
| direct.plugin.probe | See if a plugin loads | `hermes:plugin:probe` |
| direct.model.pick | Choose the model | `src/app/model-picker-overlay.tsx`, `src/api/models.ts` |
| direct.profile.get | Read the active profile | `hermes:profile:get`, `electron/desktop-profile.ts` |
| direct.profile.set | Switch profile | `hermes:profile:set` |
| direct.profile.setDefault | Set the profile used at startup | `hermes:profile:default:set` |

### schedule

| Id | Verb | Current home |
|---|---|---|
| schedule.cron.create | Create a job | `src/app/cron/cron-actions.ts`, `src/api/cron.ts` |
| schedule.cron.pause | Pause a job | same |
| schedule.cron.run | Run a job now | same |
| schedule.cron.delete | Delete a job | same |

### reach

| Id | Verb | Current home |
|---|---|---|
| reach.messaging.save | Save a channel config | `src/app/messaging`, `src/api/messaging.ts` |
| reach.webhook.create | Create a webhook | `src/app/webhooks` |
| reach.webhook.delete | Delete a webhook | `src/app/webhooks` |
| reach.notify.show | Show a desktop notification | `hermes:notify`, `electron/notification-ipc.ts` |
| reach.link.title | Fetch a link title | `hermes:fetchLinkTitle` |
| reach.external.open | Open a URL in the system browser | `hermes:openExternal`, `electron/external-open.ts` |

### arrange

| Id | Verb | Current home |
|---|---|---|
| arrange.route.open | Go to a route | `src/app/routes.ts`, `src/app/shell` |
| arrange.window.zoom | Set zoom | `hermes:zoom:get`, `hermes:zoom:set-percent` |
| arrange.window.new | Open another window | `hermes:window:openInstance` |
| arrange.window.control | Minimize, maximize, or close | `hermes:window-control` |
| arrange.window.tray | Remember minimize-to-tray | `hermes:minimize-to-tray:get`, `hermes:minimize-to-tray:set` |
| arrange.hud.open | Open the HUD | `hermes:hud:open`, `electron/hud-ipc.ts` |
| arrange.hud.move | Move the HUD | `hermes:hud:begin-move`, `hermes:hud:move-by`, `hermes:hud:set-bounds`, `hermes:hud:end-move` |
| arrange.hud.close | Close the HUD | `hermes:hud:close` |
| arrange.terminal.start | Start a terminal | `hermes:terminal:start`, `electron/terminal-ipc.ts` |
| arrange.terminal.write | Write to a terminal | `hermes:terminal:write` |
| arrange.terminal.resize | Resize a terminal | `hermes:terminal:resize` |
| arrange.terminal.dispose | Close a terminal | `hermes:terminal:dispose` |
| arrange.git.status | Read repo status | `hermes:git:repoStatus`, `electron/git-review-ops.ts` |
| arrange.git.diff | Read a file diff | `hermes:git:fileDiff`, `hermes:git:review:diff` |
| arrange.git.stage | Stage or unstage | `hermes:git:review:stage`, `hermes:git:review:unstage` |
| arrange.git.commit | Commit | `hermes:git:review:commit` |
| arrange.git.worktree | Add or remove a worktree | `hermes:git:worktreeAdd`, `hermes:git:worktreeRemove`, `hermes:git:worktreeList` |

### maintain

| Id | Verb | Current home |
|---|---|---|
| maintain.update.check | See if an update exists | `src/app/updates-overlay.tsx`, `hermes:updates:branch:get` |
| maintain.update.apply | Apply an update | `hermes:updates:apply`, `scripts/desktop-update/posix.sh` |
| maintain.boot.get | Read boot progress | `hermes:boot-progress:get`, `hermes:bootstrap:get` |
| maintain.boot.repair | Repair a local bootstrap | `hermes:bootstrap:repair` |
| maintain.boot.cancel | Cancel bootstrap | `hermes:bootstrap:cancel` |
| maintain.version.read | Read the running version | `hermes:version`, `electron/app-version.ts` |

## IPC assigned above

Channels named on an action row are assigned. These channels are also assigned, to the id in the right column. They are not extra verbs.

| Channel | Id |
|---|---|
| `hermes:connections:set-last-used` | connect.registry.setPrimary |
| `hermes:connections:update-managed` | connect.ssh.open |
| `hermes:connections:update-all` | connect.ssh.open |
| `hermes:connection:for` | connect.ssh.open |
| `hermes:connection:revalidate` | connect.ssh.test |
| `hermes:connection-config:get` | connect.registry.list |
| `hermes:connection-config:probe` | connect.ssh.test |
| `hermes:connection-config:oauth-login` | connect.cloud.login |
| `hermes:connection-config:oauth-logout` | connect.cloud.logout |
| `hermes:connection-config:apply` | connect.ssh.open |
| `hermes:connection:active-route` | arrange.route.open |
| `hermes:gateway:ws-url` | connect.ssh.open |
| `hermes:gateway:ws-url-for` | connect.ssh.open |
| `hermes:cloud:agent-sign-in` | connect.cloud.login |
| `hermes:profile:default:get` | direct.profile.get |
| `hermes:profile:remember` | direct.profile.set |
| `hermes:plugin-profile-routes` | direct.profile.get |
| `hermes:fs:gitRoot` | arrange.git.status |
| `hermes:git:branchList` | arrange.git.status |
| `hermes:git:baseBranchList` | arrange.git.status |
| `hermes:git:branchSwitch` | arrange.git.commit |
| `hermes:git:scanRepos` | arrange.git.status |
| `hermes:git:review:list` | arrange.git.status |
| `hermes:git:review:revParse` | arrange.git.status |
| `hermes:git:review:commitContext` | arrange.git.commit |
| `hermes:git:review:push` | arrange.git.commit |
| `hermes:git:review:createPr` | arrange.git.commit |
| `hermes:git:review:prList` | arrange.git.status |
| `hermes:git:review:shipInfo` | arrange.git.commit |
| `hermes:git:review:revert` | arrange.git.stage |
| `hermes:terminal:attach` | arrange.terminal.start |
| `hermes:terminal:cwd` | arrange.terminal.start |
| `hermes:window:openInTerminal` | arrange.terminal.start |
| `hermes:window:openBrowser` | reach.external.open |
| `hermes:openPreviewInBrowser` | reach.external.open |
| `hermes:watchDirectory` | look.preview.watch |
| `hermes:readFileDataUrl` | talk.compose.attach |
| `hermes:readClipboard` | talk.compose.paste |
| `hermes:writeClipboard` | talk.transcript.copy |
| `hermes:selectPaths` | talk.compose.attach |
| `hermes:selectSavePath` | look.files.saveDownload |
| `hermes:saveImageFromUrl` | look.files.saveDownload |
| `hermes:saveImageBuffer` | look.files.saveDownload |
| `hermes:fs:desktopPluginsRoot` | direct.plugin.install |
| `hermes:fs:reconcileDesktopPlugins` | direct.plugin.install |
| `hermes:fs:logsRoot` | maintain.boot.get |
| `hermes:logs:recent` | maintain.boot.get |
| `hermes:logs:reveal` | look.artifact.reveal |
| `hermes:logs:renderer-error` | maintain.boot.get |
| `hermes:logs:renderer-line` | maintain.boot.get |
| `hermes:find-in-page` | talk.transcript.page |
| `hermes:stop-find-in-page` | talk.transcript.page |
| `hermes:updates:branch:get` | maintain.update.check |
| `hermes:app:relaunch` | maintain.update.apply |
| `hermes:bootstrap:continue-local` | maintain.boot.repair |
| `hermes:bootstrap:reset` | maintain.boot.repair |
| `hermes:quick-entry:submit` | talk.compose.submit |
| `hermes:quick-entry:dismiss` | arrange.route.open |
| `hermes:quick-entry:state` | arrange.route.open |
| `hermes:quick-entry:settings:get` | direct.profile.get |
| `hermes:quick-entry:settings:set` | direct.profile.set |
| `hermes:setting:defaultProjectDir:get` | direct.profile.get |
| `hermes:setting:defaultProjectDir:pick` | direct.profile.set |
| `hermes:setting:defaultProjectDir:set` | direct.profile.set |
| `hermes:hud:frost` | arrange.hud.open |
| `hermes:hud:reset-layout` | arrange.hud.move |
| `hermes:hud:ignore-mouse` | arrange.hud.move |
| `hermes:hud:native-drag` | arrange.hud.move |
| `hermes:hud:windowing` | arrange.hud.open |
| `hermes:hud:workspace-transfer` | arrange.hud.move |
| `hermes:hud:session` | arrange.hud.open |
| `hermes:pet-overlay:open` | arrange.hud.open |
| `hermes:pet-overlay:close` | arrange.hud.close |
| `hermes:pet-overlay:set-bounds` | arrange.hud.move |
| `hermes:pet-overlay:ignore-mouse` | arrange.hud.move |
| `hermes:pet-overlay:set-focusable` | arrange.hud.open |
| `hermes:pet-overlay:state` | arrange.hud.open |
| `hermes:pet-overlay:control` | arrange.hud.open |
| `hermes:intro-reveal:open` | arrange.route.open |
| `hermes:intro-reveal:close` | arrange.route.open |
| `hermes:intro-reveal:ready` | arrange.route.open |
| `hermes:intro-reveal:skip` | arrange.route.open |
| `hermes:chat-onboarding:grow` | arrange.window.control |
| `hermes:chat-onboarding:solo-boot` | maintain.boot.get |
| `hermes:context-menu:edit` | talk.compose.paste |
| `hermes:context-menu:copy-image` | talk.transcript.copy |
| `hermes:context-menu:spellcheck` | talk.compose.paste |
| `hermes:context-menu:guest-add-word` | talk.compose.paste |
| `hermes:screenshot:subscribe` | look.preview.capture |
| `hermes:wake-indicator:get` | arrange.window.control |
| `hermes:wake-indicator:set` | arrange.window.control |
| `hermes:keep-awake` | arrange.window.control |
| `hermes:power-battery:get` | arrange.window.control |
| `hermes:zoom:get` | arrange.window.zoom |
| `hermes:machine:profile` | maintain.version.read |
| `hermes:get-remote-display-reason` | connect.ssh.open |
| `hermes:secret-storage:get` | connect.registry.save |
| `hermes:secret-storage:set` | connect.registry.save |
| `hermes:requestMicrophoneAccess` | talk.compose.submit |
| `hermes:resolveFavicon` | reach.link.title |
| `hermes:sync-status` | connect.local.probe |
| `hermes:pool-limits:get` | connect.local.touch |
| `hermes:pool-limits:set` | connect.local.touch |
| `hermes:backend:touch` | connect.local.touch |
| `hermes:workspace:sanitize` | arrange.git.status |
| `hermes:deep-link-ready` | arrange.route.open |
| `hermes:ambient:claim` | arrange.window.new |
| `hermes:window:readBelow` | arrange.window.control |
| `hermes:data-url-read-max:get` | talk.compose.attach |
| `hermes:data-url-read-max:set` | talk.compose.attach |
| `hermes:readPluginSource` | direct.plugin.probe |
| `hermes:uninstall:summary` | maintain.update.apply |
| `hermes:mcp-oauth:listen` | direct.plugin.install |
| `hermes:mcp-oauth:wait` | direct.plugin.install |
| `hermes:mcp-oauth:cancel` | direct.plugin.install |
| `hermes:vscode-theme:search` | direct.profile.set |
| `hermes:vscode-theme:fetch` | direct.profile.set |
| `hermes:native-theme` | direct.profile.set |
| `hermes:titlebar-theme` | direct.profile.set |
| `hermes:skin:local` | direct.profile.set |
| `hermes:translucency` | arrange.window.control |
| `hermes:translucency:support` | arrange.window.control |
| `hermes:feature-flags` | maintain.boot.get |
| `hermes:devtools:disable-f12` | arrange.window.control |
| `hermes:f12ShortcutActive` | arrange.window.control |
| `hermes:previewShortcutActive` | look.preview.open |
| `hermes:active-work` | talk.session.open |
| `hermes:api` | talk.compose.submit |

`hermes:api` is the gateway RPC pipe used by Talk. Mapping `talk.compose.submit` must say whether other purposes call it. If they do, the pipe becomes `shared/gateway-rpc`, and Talk calls that shared id. Do not leave `hermes:api` implemented inside a purpose container.

`hermes:hud-modifier:${name}` in `electron/hud-modifier.ts` is a template, not one channel. Assign each concrete name to `arrange.hud.move` when that id is mapped.

## Routes

| Route | Id |
|---|---|
| `/` | talk.session.open |
| `/settings` | direct.profile.get |
| `/command-center` | arrange.route.open |
| `/capabilities` | direct.toolset.enable |
| `/messaging` | reach.messaging.save |
| `/webhooks` | reach.webhook.create |
| `/artifacts` | look.artifact.reveal |
| `/cron` | schedule.cron.create |
| `/profiles` | direct.profile.set |
| `/agents` | talk.session.open |
| `/starmap` | arrange.route.open |
| `/session-import` | talk.session.open |

A route opens a surface. The surface's buttons call the other ids. The route row is only the open.
