# fledge-plugin-deploy

**The reference implementation for fledge lane hooks and deploy workflows.**

This plugin demonstrates the canonical patterns for building deploy/rollback commands and lane lifecycle hooks using the fledge plugin protocol. Use it as the starting point when building your own plugin that needs to integrate with lanes or manage deployments.

## Install

```sh
fledge plugin install CorvidLabs/fledge-plugin-deploy
```

This clones the repo into `~/.config/fledge/plugins/fledge-plugin-deploy/` and registers the commands and hooks from `plugin.toml`.

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

### `lane:post`

Runs automatically after every fledge lane completes. Prints a status line:

```
  [fledge-deploy] lane:post — ✓ build [success] (run: abc123)
```

fledge injects `FLEDGE_LANE_NAME`, `FLEDGE_LANE_STATUS`, and `FLEDGE_LANE_RUN_ID` into the hook's environment.

## Plugin structure

```
fledge-plugin-deploy/
├── plugin.toml          ← manifest: name, version, commands, hooks
├── bin/
│   ├── fledge-deploy    ← executable for `fledge deploy`
│   └── fledge-rollback  ← executable for `fledge rollback`
└── hooks/
    └── lane-post        ← runs on lane:post event
```

### `plugin.toml` reference

```toml
[plugin]
name        = "fledge-plugin-deploy"
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
event  = "lane:post"
binary = "hooks/lane-post"
```

## Lane hooks reference

This plugin is the canonical example for lane lifecycle hooks. The `hooks/lane-post` script shows how to:

- Read fledge-injected environment variables (`FLEDGE_LANE_NAME`, `FLEDGE_LANE_STATUS`, `FLEDGE_LANE_RUN_ID`)
- Report status based on lane outcomes
- Exit non-zero on lane failure so fledge surfaces hook errors

Any plugin can register hooks for `lane:pre` or `lane:post` events in its `plugin.toml`. Hooks run automatically as part of the lane lifecycle and receive context through environment variables.

## CI

This repo uses GitHub Actions for continuous integration:

- **ShellCheck** lints all scripts in `bin/` and `hooks/` on every push to `main` and on pull requests.

## Building your own plugin

1. Fork or copy this repo
2. Edit `plugin.toml` — change `name`, add your commands and hooks
3. Replace the scripts in `bin/` and `hooks/` with your logic
4. Tag your repo with the `fledge-plugin` topic so it shows up in `fledge plugin search`
5. Publish: anyone can install with `fledge plugin install <owner>/<repo>`

See the [fledge plugin docs](https://github.com/CorvidLabs/fledge/blob/main/docs/plugins.md) for the full manifest reference and hook event list.

## License

MIT
