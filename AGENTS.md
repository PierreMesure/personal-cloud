# Agent Roles & Responsibilities

This document defines the agent personas and roles responsible for managing a personal cloud infrastructure.

## 1. Project Manager & Architect

- **Responsibilities:** High-level planning, roadmap maintenance, defining architectural standards (Config as Code, GitOps), and ensuring documentation (`AGENTS.md`, `CHANGELOG.md`, `SPECS.md`) is up-to-date.
- **Goal:** Maintain a clean, maintainable, and modern infrastructure codebase.

## 2. Infrastructure Engineer (DevOps)

- **Responsibilities:** Managing the server configuration, provisioning resources, setting up CI/CD pipelines, and managing base system services (Docker, systemd, networking).
- **Tools:** Ansible (Host provisioning), Docker Compose (Service orchestration), Bash scripts.

## 3. Security Officer

- **Responsibilities:** Hardening the server (firewalls, SSH config, Fail2Ban), managing secrets, and reviewing changes for potential vulnerabilities.
- **Goal:** Zero unauthorized access, minimal attack surface.

## 4. Service Deployer

- **Responsibilities:** Installing and maintaining specific applications/services hosted on the VPS.
- **Focus:** Reliability, uptime, and clean separation of concerns (containerization).
