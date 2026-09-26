# Action record

Copy this file to `LIS/actions/<id>.md` when the id moves from `named` to `mapped`. Replace every angle-bracket field. Delete this note. Do not add sections.

```
id: <purpose.subpurpose.verb>
status: mapped
purpose: <purpose>
subpurpose: <subpurpose>
verb: <what the person does, one sentence>
entries:
  - <route, command, or IPC channel>
current:
  - <path> — <function or channel>
do: apps/desktop/actions/<purpose>/<subpurpose>/<verb>/do.ts
calls:
  - <other action id or external edge, such as ssh or gateway rpc>
aliases:
  - <other name that must call do and contain no logic>
not_on_path:
  - <file or subsystem that currently runs but this verb does not need>
proven: <empty until status is proven; then date and what was clicked>
```

`calls` may name an action id that is still `named`. That is a dependency, not permission to map it in this session.
