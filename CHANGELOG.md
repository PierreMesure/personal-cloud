# Changelog

All notable changes to this project and the target server will be documented in this file.

## v2026-11-15

### Changed

- Upgraded Ubuntu 24.04.5 LTS to 26.04.1 LTS through the supported release upgrader, then rebooted successfully into kernel 7.0.0-43-generic. Updated host dependencies include systemd 259.9, OpenSSH 10.2p1, and Python 3.14.4.
- Created Hetzner snapshot `456727246` with the VPS powered off before the release upgrade, then deleted it at the user's request after successful verification. Confirmed all seven regular server backups remain available; application backups were left intact. The snapshot covered the VPS disk; the external Storage Box photo library is separate.
- Enabled Docker's official `resolute` repository and replaced its Ubuntu 24.04 packages with Ubuntu 26.04 builds. The release upgrader selected Ubuntu's standard repositories after rejecting the previous Hetzner mirror configuration.
- Temporarily preferred IPv4 to bypass stalled Ubuntu release-metadata connections over IPv6; restored the original address-selection settings after installation.
- Verified automatic container startup, the CIFS storage mount, all six public HTTPS endpoints, application readiness, browser rendering, selected content counts, SQLite integrity, and Ansible fact gathering after the Ubuntu 26.04 reboot. No failed systemd units or pending package updates remained.
- Before the release upgrade, applied 42 Ubuntu 24.04 package updates, including Docker Engine 29.9.3 and Compose 5.7.1. Installed the initially phased `distro-info` and `python3-distro-info` updates as release-upgrade prerequisites.
- Rebooted into kernel 6.8.0-149-generic on Ubuntu 24.04.5 LTS and confirmed all containers start automatically after the Storage Box mounts.
- Upgraded and pinned Immich 3.4.1, Mealie 3.31.0, Karakeep 0.34.1, Vikunja 2.8.0, Multica 0.9.0, and Caddy 2.11.9. Updated Multica's PostgreSQL 17 image to 17.12 and refreshed Karakeep's browser container.
- Updated Immich's Valkey image digest to the one shipped with release 3.4.1 and enabled the optimized v2 machine-learning models. Retained the PostgreSQL and Meilisearch pins recommended by the respective application releases.
- Removed legacy Chrome debugging-port overrides that conflict with the new Karakeep browser image. Confirmed browser discovery and actual page rendering through Karakeep's browser client.
- Restricted Karakeep authentication to its configured origin. Its automatic authentication migration signs web users out once; mobile sessions and API keys are unaffected ([upstream notes](https://github.com/karakeep-app/karakeep/releases/tag/v0.34.0)).
- Deployment now waits for Compose readiness and serializes image operations to reduce contention on shared containerd layers.
- Verified application readiness, SQLite integrity, and selected content counts after upgrading and rebooting, including all 18,720 Immich assets and 84 Vikunja tasks.
- Added a Docker systemd dependency on the Storage Box mount to protect Immich's photo storage during startup.
- Added PostgreSQL dumps and a consistent archive of application volumes and configuration under `/var/backups/personal-cloud/2026-11-15`; verified dump readability, archive integrity, and checksums.
- Copied the encrypted application backup to `/mnt/storage-box/maintenance-backups/2026-11-15/application-backup.tar.gpg`. The recovery passphrase is stored in the server backup directory and locally in Git-ignored `.ansible/maintenance/2026-11-15/offsite-backup.passphrase` (mode `0600`). The existing photo library and disposable ML cache are excluded from this archive.

## v2026-08-26

### Fixed

- Switched Karakeep's Chrome container from the billing-gated GCR image to Karakeep's public GHCR image.
- Recreate Caddy when its bind-mounted configuration changes so new hostnames take effect immediately.

### Changed

- Upgraded Immich from the v2 release channel to v3, retaining the ARMNN machine-learning image.
- Added Multica self-hosting with separate frontend and API hostnames, persistent PostgreSQL storage, and Caddy WebSocket routing.

## v2026-07-15

### Changed

- Migrated automated DNS management to the new provider's API.

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
- Replaced the dynamic inventory file with an integrated `hcloud_discovery` role that fetches the server IP and Storage Box details directly from the API.
- Refactored `storage` role to use the native `hetzner.hcloud.storage_box` module for automatic SSH key injection and service activation.
- Switched to direct OVH API calls via Ansible's `uri` module for DNS management, removing the need for external collections.
- Added `storage` role to automate Hetzner Storage Box mounting via SSHFS. It automatically generates SSH keys on the VPS and authorizes them on the Storage Box (delegated via localhost).
- Updated Immich configuration to store uploads on the mounted Storage Box (`/mnt/storage-box/immich-library`).
- Migrated Storage Box mount from SSHFS to CIFS (Samba) for improved stability and kernel-level performance. Implemented secure credential management via `/etc/storage-box.credentials`.
- Optimized Immich Machine Learning by switching to the `-armnn` image tag for improved facial recognition performance on ARM64.
- Added Mealie recipe manager service
- Implemented full dual-stack IPv4/IPv6 support for DNS. Ansible now automatically fetches the server's IPv6 address from Hetzner and creates both A and AAAA records via the OVH API.

## v2026-06-08

### Added

- Added Vikunja task management service with automatic deployment through the shared `docker_service` role.
- Added reverse proxy routing and DNS-managed domain support for Vikunja.

## v2026-07-08

### Added

- Added Karakeep service deployment with official `web`, `chrome`, and `meilisearch` containers via the shared `docker_service` role.
- Added Karakeep domain routing through Caddy and automatic DNS-managed domain support.

### Changed

- Extended `ansible/vars.yml.example` with the Karakeep domain and required authentication/search secrets.
