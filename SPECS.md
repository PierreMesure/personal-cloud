# Server Specifications

This document outlines the hardware and software specifications of the managed VPS.

## Hardware

- **Architecture:** ARM64 (aarch64)
- **CPU:** 4 vCPUs (ARM Neoverse-N1)
- **RAM:** 8 GB Total (7.5Gi usable)
- **Storage:** ~80 GB Total (75G root partition)
  - `/`: 75G (ext4 presumably)
  - Swap: None active

## Software (Verified 2026-11-15)

- **Operating System:** Ubuntu 26.04.1 LTS (Resolute Raccoon)
- **Kernel:** Linux 7.0.0-43-generic
- **Container Runtime:** Docker Engine 29.9.3, Compose 5.7.1, containerd 2.4.2 (Ubuntu 26.04 packages)
- **Host Dependencies:** systemd 259.9, OpenSSH 10.2p1, Python 3.14.4
- **Security:** Fail2Ban enabled (SSH protection)
- **Public IP:** Stored in .env

## Applications

| Service | Version |
| --- | --- |
| Immich server and ARMNN machine learning | 3.4.1 |
| Mealie | 3.31.0 |
| Karakeep | 0.34.1 |
| Vikunja | 2.8.0 |
| Multica frontend and backend | 0.9.0 |
| Caddy | 2.11.9 |
| Immich Valkey | 9.1.2 |
| Multica PostgreSQL | 17.12 with pgvector |
| Immich PostgreSQL | 14.19, VectorChord 0.4.3 (upstream-pinned image) |
| Karakeep Meilisearch | 1.41.0 (upstream-pinned version) |

Docker waits for the CIFS Storage Box mount before starting. The existing photo
library is stored at `/mnt/storage-box/immich-library`; application databases are
stored on local disk.
