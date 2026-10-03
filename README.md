# proxmox-templates

Proxmox VM and LXC templates for the lab, each built from a pinned base image
by a script in this repository, so a template is reproducible and its drift is
a diff. The first family is the CI runners that take the trusted half of
GitHub Actions off hosted runners; other template families (services,
honeypots, lab targets) join as they are needed, one directory each.

| Family | Status | Plan |
|---|---|---|
| CI runners (`rt-*`) | planned | [docs/ci-runners.md](docs/ci-runners.md) |

## Layout, as it will be

```
docs/            one page per template family: what exists, why, how it is built
templates/<family>/<name>/   cloud-init user-data and the build parameters
scripts/         build-template.sh and friends: import, cloud-init, smoke test, promote
controller/      the ephemeral-runner controller for the CI family
```

## Rules every template follows

- The base image is pinned by checksum and named in the template's parameters.
- A template is rebuilt from scratch on a schedule rather than patched in place;
  the previous one is kept for rollback.
- A template that fails its smoke test is not promoted.
- Nothing durable lives in a clone: secrets arrive at clone time and die with it.

CI for this repository comes from
[git-your-ship-together](https://github.com/ChiefGyk3D/git-your-ship-together)
once there are scripts to lint.
