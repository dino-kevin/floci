# floci

Floci is a free, open-source local AWS emulator for development, testing, and CI.

It gives you AWS-shaped services on your machine without requiring a cloud account, an auth token, or paid feature gates. Point your AWS SDK, CLI, Terraform, CDK, OpenTofu, or test suite at <http://localhost:4566> and keep your existing workflows.

## Why you should use it

Floci is rapidly attracting attention among DevOps engineers, platform teams, backend developers, and cloud-native enthusiasts because it takes a radically different approach to local cloud emulation.

## Quick start with Docker Compose

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/) 20.10+
- [Docker Compose](https://docs.docker.com/compose/install/) v2
- A [Cloudflare Tunnel](https://one.dash.cloudflare.com/) token (only required if you want to expose floci through a tunnel)

### 1. Configure environment

Create a `.env` file in the project root:

```env
TUNNEL_TOKEN=your-cloudflare-tunnel-token
```

### 2. Launch the stack

```bash
docker compose up --build
```

This starts two services on a shared `floci-network`:

| Service     | Image                          | Description                                  |
| ----------- | ------------------------------ | -------------------------------------------- |
| `floci`     | `floci/floci:latest`           | Local AWS emulation, exposed on `4566`       |
| `cloudflared` | `cloudflare/cloudflared:latest` | Cloudflare Tunnel proxying `floci` securely |

The named volume `floci-data` is mounted at `/app/data` inside the `floci` container to persist AWS state across restarts.

### 3. Verify

- Floci is available locally at <http://localhost:4566>
- If a tunnel token is configured, `cloudflared` will publish floci through the tunnel configured in your Cloudflare Zero Trust dashboard.

### Useful commands

```bash
docker compose up -d          # run in the background
docker compose logs -f        # tail logs from all services
docker compose down           # stop the stack
docker compose down -v        # stop and remove the floci-data volume
```

## Thanks and attribution

Thanks to the **floci team** for building and maintaining such a great tool.

- Website: <https://floci.io/floci/>
- GitHub: <https://github.com/floci-io/floci>