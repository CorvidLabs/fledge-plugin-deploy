# fledge-deploy

An example [fledge](https://github.com/CorvidLabs/fledge) plugin demonstrating deploy, rollback, and post-flow hooks. Use this as a reference when building your own plugin.

## Install

```sh
fledge plugin install CorvidLabs/fledge-deploy
```

This clones the repo into `~/.config/fledge/plugins/fledge-deploy/` and registers the commands and hooks from `plugin.toml`.

## Commands

### `fledge deploy`

Deploys your project to the target environment.

```sh
fledge deploy                            # deploy to staging (default)
fledge deploy --env production --version v1.2.3
fledge deploy --dry-run                  # print steps without running
```

**Environment variables:**

| Variable         | Default     | Description                              |
|------------------|-------------|------------------------------------------|
| `DEPLOY_ENV`     | `staging`   | Target environment                       |
| `DEPLOY_VERSION` | _(latest)_  | Version/tag to deploy (required in prod) |
| `DRY_RUN`        | `false`     | Print steps without executing            |

### `fledge rollback`

Rolls back to the previous stable deployment.

```sh
fledge rollback                   # rollback staging with prompt
fledge rollback --env production --yes
```

## Hooks

### `flow:post`

Runs automatically after every fledge flow completes. Prints a status line:

```
  [fledge-deploy] flow:post — ✓ build [success] (run: abc123)
```

fledge injects `FLEDGE_FLOW_NAME`, `FLEDGE_FLOW_STATUS`, and `FLEDGE_FLOW_RUN_ID` into the hook's environment.

## Plugin structure

```
fledge-deploy/
├── plugin.toml          ← manifest: name, version, commands, hooks
├── bin/
│   ├── fledge-deploy    ← executable for `fledge deploy`
│   └── fledge-rollback  ← executable for `fledge rollback`
└── hooks/
    └── flow-post        ← runs on flow:post event
```

### `plugin.toml` reference

```toml
[plugin]
name        = "fledge-deploy"
version     = "0.1.0"
description = "..."
author      = "CorvidLabs"

[[commands]]
name   = "deploy"
binary = "bin/fledge-deploy"   # relative to plugin root

[[commands]]
name   = "rollback"
binary = "bin/fledge-rollback"

[[hooks]]
event  = "flow:post"
binary = "hooks/flow-post"
```

## Building your own plugin

1. Fork or copy this repo
2. Edit `plugin.toml` — change `name`, add your commands and hooks
3. Replace the scripts in `bin/` and `hooks/` with your logic
4. Tag your repo with the `fledge-plugin` topic so it shows up in `fledge plugin search`
5. Publish: anyone can install with `fledge plugin install <owner>/<repo>`

See the [fledge plugin docs](https://github.com/CorvidLabs/fledge/blob/main/docs/plugins.md) for the full manifest reference and hook event list.

## License

MIT
