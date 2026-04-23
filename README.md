# SE2-26S server config

Central deployment config for the SE2 course server.
[doco-cd](https://doco.cd) polls this repo and deploys each group's server automatically.

## How it works

- Each group has a file in `groups/grp-N.yml` pointing to their server repository.
- On every push to `main`, a GitHub Action merges all group files (sorted by name) into `.doco-cd.yml`.
- doco-cd polls this repo every 3 minutes, reads `.doco-cd.yml`, and deploys each group's `production.docker-compose.yaml` from their own repository.

## Adding a new group

1. Create a file `groups/grp-N.yml` with the following content:

```yaml
name: grp-1-your-game-server       # lowercase, no spaces
repository_url: https://github.com/your-org/your-server-repo.git
reference: main
compose_files:
  - production.docker-compose.yaml
force_image_pull: true   # always pull the latest image on every deploy
```

Your `production.docker-compose.yaml` should use a pre-built image (e.g. from `ghcr.io`) rather than a `build:` instruction - we want to keep deployment fast and simple. You can set up GitHub Actions in your server repo to build and push the image on every push to `main`.

For all the possible configuration values, see the [doco-cd docs](https://doco.cd/v0.82/Deploy-Settings/#available-settings).

2. Open a Pull Request against `main`.
3. Once merged, the Action regenerates `.doco-cd.yml` and deployment starts within ~3 minutes.

## Important

- **Do not edit `.doco-cd.yml` directly** — it is auto-generated and your changes will be overwritten.
- The `name` field must be **lowercase** and contain only letters, numbers, hyphens, and underscores.
