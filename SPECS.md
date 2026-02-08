# Server Specifications

This document outlines the hardware and software specifications of the managed VPS.

## Hardware

- **Architecture:** ARM64 (aarch64)
- **CPU:** 4 vCPUs (ARM Neoverse-N1)
- **RAM:** 8 GB Total (7.5Gi usable)
- **Storage:** ~80 GB Total (75G root partition)
  - `/`: 75G (ext4 presumably)
  - Swap: None active

## Software (Current State)

- **Operating System:** Ubuntu 24.04.3 LTS (Noble Numbat)
- **Kernel:** Linux 6.8.0-90-generic
- **Container Runtime:** Docker 27.x+ with Compose Plugin
- **Security:** Fail2Ban enabled (SSH protection)
- **Public IP:** Stored in .env
