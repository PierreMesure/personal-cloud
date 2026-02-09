# Changelog

All notable changes to this project and the target server will be documented in this file.

## v2026-02-08

### Added

- Initialized project structure.
- Created `AGENTS.md` to define management roles.
- Created `CHANGELOG.md` for tracking infrastructure changes.
- Configured `.env` for sensitive server information and updated `.gitignore`.

### Changed

- Performed initial system update and upgrade (`apt upgrade`).
- Set up Ansible configuration for server provisioning.
  - Created `deploy.sh` wrapper script.
  - Added `common` role for system updates and basic tools.
  - Added `docker` role for installing Docker Engine and Compose.
- Updated project documentation (README, AGENTS, SPECS).
- Added generic `docker_service` role for scalable application deployment.
- Configured Immich (v2) using the new generic role and secure secret injection.
- Added Caddy as a reverse proxy with automatic HTTPS and private domain configuration via environment variables.
- Successfully deployed Immich and Caddy. Resolved initial Postgres authentication issues by re-initializing the database and fixed Caddy connectivity by refreshing its DNS cache.
- Refactored variable management: `.env` handles script-level variables (IP), while `ansible/vars.yml` handles all application secrets and configurations.
- Replaced the Hetzner dynamic inventory file with an integrated `hcloud_discovery` role that fetches the server IP and Storage Box details directly from the API.
- Refactored `storage` role to use the native `hetzner.hcloud.storage_box` module for automatic SSH key injection and service activation.
- Switched to direct OVH API calls via Ansible's `uri` module for DNS management, removing the need for external collections.
- Added `storage` role to automate Hetzner Storage Box mounting via SSHFS. It automatically generates SSH keys on the VPS and authorizes them on the Storage Box (delegated via localhost).
- Updated Immich configuration to store uploads on the mounted Storage Box (`/mnt/storage-box/immich-library`).
- Migrated Storage Box mount from SSHFS to CIFS (Samba) for improved stability and kernel-level performance. Implemented secure credential management via `/etc/storage-box.credentials`.
- Optimized Immich Machine Learning by switching to the `-armnn` image tag for improved facial recognition performance on ARM64.
- Added `backup` role to automate daily logical Postgres backups. Implemented a Bash script scheduled via cron that saves compressed SQL dumps to the Storage Box and retains the last 7 days of history.
