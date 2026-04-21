# SE2-26S-server-config

Central deployment config for the SE2 course server (`se2-demo.aau.at`).
[doco-cd](https://doco.cd) polls this repo and deploys each group's server automatically.

## How it works

- Each group has a file in `groups/grp-N.yml` pointing to their server repository.
- On every push to `main`, a GitHub Action merges all group files (sorted by name) into `.doco-cd.yml`.
- doco-cd polls this repo every 3 minutes, reads `.doco-cd.yml`, and deploys each group's `production.docker-compose.yaml` from their own repository.

## Adding a new group

1. Create a file `groups/grp-N.yml` with the following content:

```yaml
name: your-group-name       # lowercase, no spaces
repository_url: https://github.com/your-org/your-server-repo.git
reference: main
compose_files:
  - production.docker-compose.yaml
```

2. Open a Pull Request against `main`.
3. Once merged, the Action regenerates `.doco-cd.yml` and deployment starts within ~3 minutes.

## Important

- **Do not edit `.doco-cd.yml` directly** — it is auto-generated and your changes will be overwritten.
- The `name` field must be **lowercase** and contain only letters, numbers, hyphens, and underscores.
- Your server repo must contain a `production.docker-compose.yaml` in its root directory on the `main` branch.
