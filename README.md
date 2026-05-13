# saladdockers

Docker images built for SaladCloud, published to GitHub Container Registry via GitHub Actions.

## Images

### mercury

Based on Ubuntu 22.04. Supports `sshd` with a non-root `ubuntu` user (password: `ubuntu`). Built for `linux/amd64` and `linux/arm64`.

Installed packages:

| Tool | Purpose |
|------|---------|
| openssh-server | SSH daemon |
| curl | HTTP client |
| python3, python3-pip | Python runtime |
| iproute2 | `ip` networking utilities |
| git | Version control |
| gcc, g++ | C/C++ compilers |
| cmake, make | Build systems |
| gdb | Debugger |
| build-essential | C/C++ standard headers and toolchain |
| lsb-release | Distro info (`lsb_release`) |


## Registry

Images are published to `ghcr.io/<owner>/saladdockers/<image>` with two tags:

- `latest` — updated on every push to `main`
- `YYYYMMDD` — date-stamped tag for each build

## CI/CD

The GitHub Actions workflow (`.github/workflows/docker-publish.yml`) runs on every push or pull request to `main`:

- **Pull requests**: builds all images but does not push
- **Pushes to main**: builds and pushes all images to GHCR

Builds use GitHub Actions cache (`type=gha`) scoped per image to speed up layer reuse.

## Local Build

```bash
podman build -t mercury ./mercury
podman build -t earth ./earth
```

## Verify

After building, run the container and check all installed tools:

```bash
podman run --rm mercury bash -c "
  curl --version | head -1 &&
  python3 --version &&
  ip -V &&
  git --version &&
  gcc --version | head -1 &&
  g++ --version | head -1 &&
  cmake --version | head -1 &&
  gdb --version | head -1 &&
  make --version | head -1 &&
  lsb_release -a
"
```

Each command should print its version without errors.

## Run

**mercury** — starts sshd and maps it to port 2222 on the host:

```bash
podman run -d --name mercury -p 2222:22 mercury
```

Connect via SSH as the `ubuntu` user (default password: `ubuntu`):

```bash
ssh -p 2222 ubuntu@localhost
```


## 
```
 --security-opt seccomp=unconfined --cap-add SYS_ADMIN --cap-add=SYS_PTRACE  
```

