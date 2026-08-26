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

## GitHub Actions deployment

Pushes to `master` or `main` that change the Ansible configuration automatically
run the deployment workflow. The workflow first checks Ansible syntax, then deploys
through the `production` GitHub Environment using pinned `uv`, Python 3.14, and
Ansible versions.

Configure these secrets in the `production` environment:

- `HCLOUD_TOKEN`
- `STORAGE_BOX_PASSWORD`
- `IMMICH_DB_PASSWORD`
- `KARAKEEP_NEXTAUTH_SECRET`
- `KARAKEEP_MEILI_MASTER_KEY`
- `KARAKEEP_OPENROUTER_API_KEY`
- `INFOMANIAK_API_TOKEN`
- `SSH_PRIVATE_KEY`: the private key used to connect to the VPS as `root`.
- `IMMICH_DOMAIN`
- `MEALIE_DOMAIN`
- `KARAKEEP_DOMAIN`
- `VIKUNJA_DOMAIN`
- `DNS_ZONE`.

Configure these non-sensitive Environment variables there as well:

`SERVER_NAME` and `STORAGE_BOX_NAME`.

The runner assembles a temporary `ansible/vars.yml` from these values and removes
it after deployment; no credentials are committed to the repository.

### Authorize the GitHub Actions SSH key

Create a deployment-only key locally:

```bash
ssh-keygen -t ed25519 -f ~/.ssh/personal-cloud-actions -C github-actions-personal-cloud
```

Authorize its public half on the VPS using an existing administrative key:

```bash
cat ~/.ssh/personal-cloud-actions.pub | ssh root@YOUR_SERVER_IP \
  'umask 077; mkdir -p /root/.ssh; cat >> /root/.ssh/authorized_keys; chmod 600 /root/.ssh/authorized_keys'
```

Copy the complete contents of `~/.ssh/personal-cloud-actions` into the
`SSH_PRIVATE_KEY` secret. Keep the private key out of the repository. The
playbook discovers the server address through Hetzner, then Ansible uses this
authorized key for its root SSH connection.

The workflow also supports manual runs from the Actions tab. Its concurrency lock
prevents overlapping deployments.
