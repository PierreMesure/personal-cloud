# Personal Cloud

Here's the gitops repository for my personal server. Feel free to have a look for inspiration.

Currently managed services include Immich, Mealie, Karakeep, Vikunja, and Caddy for reverse proxy/TLS.

## 🚀 Quick Start

### Prerequisites

- Ansible installed locally (`brew install ansible`)
- SSH access to the server (key-based)
- A `.env` file with `SERVER_IP=your_server_ip`

### Deploying Changes

To provision the server or apply configuration changes, run:

```bash
./deploy.sh
```

## DNS at Infomaniak

The DNS role manages the A and AAAA records for the configured service domains.
Create an API token in Infomaniak Manager with `dns:read` and `dns:write` scopes:

```yaml
dns_zone: "example.com"
infomaniak_api_token: "..."
```
