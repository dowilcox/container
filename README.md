# container

A CLI tool for managing k3s deployments.

## Installation

```bash
# Clone the repo
git clone https://github.com/dowilcox/container.git

# Copy to your PATH
cp container/container ~/.local/bin/

# Or symlink it
ln -s $(pwd)/container/container ~/.local/bin/container
```

## Requirements

- k3s cluster with kubectl configured
- podman (for build commands)

## Usage

```
container <command> [arguments]
```

### Commands

| Command | Aliases | Description |
|---------|---------|-------------|
| `list` | `ls` | List all deployments |
| `status <name>` | `st` | Show status and health of a container |
| `logs <name> [-f]` | `log` | View container logs (-f to follow) |
| `shell <name>` | `sh`, `exec` | Open a shell in the container |
| `describe <name>` | `desc` | Show detailed container information |
| `events <name>` | `ev` | Show recent events for the container |
| `start <name>` | | Start a stopped container (scale to 1) |
| `stop <name>` | | Stop a container (scale to 0) |
| `restart <name>` | | Restart a container |
| `scale <name> <n>` | | Scale container to n replicas |
| `build <name>` | | Build container image only |
| `update <name>` | `up` | Build and deploy container |
| `help` | | Show help message |

### Examples

```bash
# List all deployments
container list

# Check status of jellyfin
container status jellyfin

# Follow logs for openwebui
container logs openwebui -f

# Restart scrypted
container restart scrypted

# Scale jellyfin to 2 replicas
container scale jellyfin 2

# Stop a container
container stop myapp

# Open shell in a container
container shell nginx

# Rebuild and deploy a local image
container update myapp
```

## Features

- Case-insensitive partial name matching (e.g., `container status jelly` finds jellyfin)
- Automatic shell detection (tries bash, sh, ash)
- Color output (disabled when piping)
- Auto-rebuild for local images on update
- Filters out system deployments (kube-system, local-path, svclb)
- Handles hostNetwork deployments correctly (uses scale down/up instead of rolling restart)

## Build Command

The `build` command looks for a Dockerfile in these locations:

- `~/projects/<name>/`
- `~/<name>/`
- `/opt/<name>/`
- `~/src/<name>/`

Images are built with podman and imported into k3s containerd.

## License

MIT
