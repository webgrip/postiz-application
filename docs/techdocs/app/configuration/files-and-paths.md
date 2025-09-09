---
title: "Files & Paths"
description: "Config files, data directories, and persistence responsibilities."
tags: [configuration, filesystem]
search: { boost: 3, exclude: false }
icon: material/folder-cog
author: "Infrastructure/Platform Team"
date: 2025-01-09
---

**Purpose:** Make storage locations and ownership explicit.

**Contents**
- [Config files](#config-files)
- [Data directories & volumes](#data-directories--volumes)
- [Certificates & secrets](#certificates--secrets)
- [Sources](#sources)

### Config files
| Path | Purpose | Format | Notes |
|------|---------|--------|-------|
| `.env` | Environment configuration | Key=Value | Main application config |
| `config/app.php` | Laravel application settings | PHP array | Auto-generated from .env |
| `config/database.php` | Database connection settings | PHP array | Maps to DB_* env vars |
| `storage/logs/` | Application log files | Laravel logs | Rotated daily |

### Data directories & volumes
| Path | What it stores | Backup/Retention | Notes | Source |
|------|-----------------|------------------|-------|--------|
| `/var/www/app/storage` | Uploaded media, logs, cache | Daily backup, 90-day retention | User-generated content | "Postiz Storage Guide" — https://docs.postiz.com/installation/storage — retrieved 2025-01-09 |
| `/var/www/app/public` | Static assets, public uploads | Weekly backup, 365-day retention | Web-accessible files | "Postiz Storage Guide" — https://docs.postiz.com/installation/storage — retrieved 2025-01-09 |
| `/var/lib/mysql` | MariaDB data files | Daily backup, 30-day retention | Database persistence | "MariaDB Docker Documentation" — https://hub.docker.com/_/mariadb — retrieved 2025-01-09 |
| `/data` | Redis persistence (RDB/AOF) | Weekly backup, 7-day retention | Cache and session data | "Redis Docker Documentation" — https://hub.docker.com/_/redis — retrieved 2025-01-09 |

### Certificates & secrets
| Path | Purpose | Security Level | Rotation |
|------|---------|----------------|----------|
| `.env` | Application secrets | High | Manual |
| `age.agekey` | SOPS encryption key | Critical | Annual |
| `ssl/` | TLS certificates | High | Automated (Let's Encrypt) |

**Secret Management:**
- Use SOPS + age for encrypted secrets at rest
- Never commit plaintext secrets to git
- Rotate database passwords quarterly
- Use strong, unique passwords for all services

### Sources
- "Postiz Storage Configuration" — https://docs.postiz.com/installation/storage — retrieved 2025-01-09
- "Laravel File Storage" — https://laravel.com/docs/11.x/filesystem — retrieved 2025-01-09

<!-- ai-docs-metadata
{"last_audit":"2025-01-09","fingerprints":{"sources":{"https://docs.postiz.com/installation/storage":"sha256:pending","https://laravel.com/docs/11.x/filesystem":"sha256:pending"},"sections":{"volumes":"sha256:p5q6r7s8"}}}
-->