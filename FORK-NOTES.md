# Fork notes

This repository is a maintenance fork of
[fitchmultz/pi-agent-browser-native](https://github.com/fitchmultz/pi-agent-browser-native),
based on upstream `v0.6.10`.

## Changes

### Resolve a real Node runtime for the script sandbox worker

Upstream `script` mode starts its sandbox worker with `spawn(process.execPath, ...)`. When the
host Pi is a Node single executable application (SEA), `process.execPath` points at the Pi binary
instead of a Node runtime, so the worker never signals `ready` and every `script` call runs until
it times out.

This fork resolves the worker runtime before spawning:

1. `PIAB_SCRIPT_NODE` when it points at an executable file;
2. `process.execPath` when its basename already is `node`/`nodejs` (no behavior change for
   regular Node hosts);
3. `/pi/node/tool/fnm/aliases/default/bin/node`, then `/usr/local/bin/node`, then `/usr/bin/node`;
4. fallback to `process.execPath`.

The change is limited to `extensions/agent-browser/lib/input-modes/script.ts`.

## Installing this fork

```bash
pi install git:github.com/outmost9271/pi-agent-browser-native@v0.6.10-piab.1
```

## License

MIT, see [LICENSE](LICENSE).
