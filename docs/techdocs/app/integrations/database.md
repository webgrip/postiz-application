---
title: "Database Integration"
description: "MariaDB configuration, connection format, and operational requirements."
tags: [database, mariadb, persistence]
search: { boost: 3, exclude: false }
icon: material/database
author: "Infrastructure/Platform Team"
date: 2025-01-09
---

**Purpose:** Document database integration and operational requirements.

**Contents**
- [Supported backends](#supported-backends)
- [Connection configuration](#connection-configuration)
- [Container interface](#container-interface)
- [Backup & restore](#backup--restore)
- [Sources](#sources)

### Supported backends
Postiz supports multiple database backends with MariaDB as the recommended production choice:

| Backend | Status | Use Case | Notes |
|---------|--------|----------|-------|
| MariaDB | ✅ Recommended | Production | High performance, full feature support |
| MySQL | ✅ Supported | Production | Compatible with MariaDB configuration |
| PostgreSQL | ✅ Supported | Production | Set DB_CONNECTION=pgsql |
| SQLite | ⚠️ Development | Development only | Not recommended for production |

### Connection configuration
Database connection is configured via environment variables:

```bash
DB_CONNECTION=mysql
DB_HOST=postiz-application.mariadb
DB_PORT=3306
DB_DATABASE=application
DB_USERNAME=application
DB_PASSWORD=application
```

**Connection Format:**
- **MariaDB/MySQL**: `mysql://username:password@host:port/database`
- **PostgreSQL**: `postgresql://username:password@host:port/database`
- **SQLite**: `sqlite:///path/to/database.sqlite`

### Container interface
| Aspect | Value / Path | Notes |
|-------|---------------|-------|
| Ports | 3306/tcp | Standard MariaDB port |
| Healthcheck | `mariadb --user=${DB_USERNAME} --password=${DB_PASSWORD} --execute=SELECT 1` | Must match upstream defaults |
| Volumes | `/var/lib/mysql` | Data persistence and retention required |
| Charset | `utf8mb4_unicode_ci` | Required for full Unicode support |
| Engine | InnoDB | Default storage engine for ACID compliance |

### Backup & restore
**Backup Strategy:**
```bash
# Create backup
docker exec postiz-application.mariadb mariadb-dump \
  --user=${DB_USERNAME} --password=${DB_PASSWORD} \
  --single-transaction --routines --triggers \
  ${DB_DATABASE} > backup.sql

# Restore from backup
docker exec -i postiz-application.mariadb mariadb \
  --user=${DB_USERNAME} --password=${DB_PASSWORD} \
  ${DB_DATABASE} < backup.sql
```

**Retention Policy:**
- Daily backups retained for 30 days
- Weekly backups retained for 12 weeks
- Monthly backups retained for 12 months

### Sources
- "Postiz Database Requirements" — https://docs.postiz.com/installation/database — retrieved 2025-01-09
- "MariaDB Docker Hub" — https://hub.docker.com/_/mariadb — retrieved 2025-01-09

<!-- ai-docs-metadata
{"last_audit":"2025-01-09","fingerprints":{"sources":{"https://docs.postiz.com/installation/database":"sha256:pending","https://hub.docker.com/_/mariadb":"sha256:pending"},"sections":{"database":"sha256:v5w6x7y8"}}}
-->