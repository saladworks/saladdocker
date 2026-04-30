# saladdockers

Docker images built for SaladCloud, published to GitHub Container Registry via GitHub Actions.

## Images

The mercury image is based on ubuntu 22. The image should support the sshd service inside the container. The image also support a user "ubuntu" to login instead of only for root.
The image should not use the unminimize command and only install the software necessary.


## Registry

Images are published to `ghcr.io/<owner>/saladdockers/<image>` with two tags:

- `latest` — updated on every push to `main`
- `YYYYMMDD` — date-stamped tag for each build

## CI/CD

The GitHub Actions workflow (`.github/workflows/docker-publish.yml`) runs on every push or pull request to `main`:

- **Pull requests**: builds all four images but does not push
- **Pushes to main**: builds and pushes all four images to GHCR

Builds use GitHub Actions cache (`type=gha`) scoped per image to speed up layer reuse.

## Local Build

```bash
podman build -t mercury ./mercury
podman build -t venus ./venus
podman build -t earth ./earth
podman build -t mars ./mars
```

## Run

**mercury** — starts sshd and maps it to port 2222 on the host:

```bash
podman run -d --name mercury -p 2222:22 mercury
```

Connect via SSH as the `ubuntu` user (default password: `ubuntu`):

```bash
ssh -p 2222 ubuntu@localhost
```

For other images (no exposed services, drops to a shell):

```bash
podman run -it --rm venus
podman run -it --rm earth
podman run -it --rm mars
```
