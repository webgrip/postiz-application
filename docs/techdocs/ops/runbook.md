---
title: "Runbook"
description: "Routine operations: start/stop, backups, restores, health."
tags: [operations, runbook]
search: { boost: 4, exclude: false }
icon: material/clipboard-text
author: "Infrastructure/Platform Team"
date: 2025-01-09
---

**Purpose:** Safe day-2 operations.

**Contents**
- [Start/Stop](#startstop)
- [Backups/Restores](#backupsrestores)
- [Health & Readiness](#health--readiness)
- [Common Tasks](#common-tasks)
- [Sources](#sources)

### Start/Stop

**Start Stack:**
```bash
cd /path/to/postiz-application
make start
```

**Verify Startup:**
```bash
# Check container health
docker ps --filter "name=postiz-application"

# Monitor logs during startup  
make logs

# Verify application responds
curl -f http://localhost:8080/health
```

**Stop Stack:**
```bash
make stop
```

**Clean Restart:**
```bash
make stop
docker system prune -f
make start
```

### Backups/Restores

**Database Backup:**
```bash
# Create timestamped backup
BACKUP_DATE=$(date +%Y%m%d_%H%M%S)
docker exec postiz-application.mariadb mariadb-dump \
  --user=application --password=application \
  --single-transaction --routines --triggers \
  application > "backup_${BACKUP_DATE}.sql"
```

**Application Data Backup:**
```bash
# Backup uploaded media and application data
docker run --rm -v postiz-application-application-storage-data:/data \
  -v $(pwd):/backup alpine tar czf /backup/storage_${BACKUP_DATE}.tar.gz -C /data .

docker run --rm -v postiz-application-application-public-data:/data \
  -v $(pwd):/backup alpine tar czf /backup/public_${BACKUP_DATE}.tar.gz -C /data .
```

**Restore Database:**
```bash
# Stop application during restore
make stop

# Restore database
docker run --rm -i -v $(pwd):/backup \
  --network postiz-application_default mariadb:latest \
  mariadb -h postiz-application.mariadb -u application -papplication \
  application < backup_YYYYMMDD_HHMMSS.sql

# Restart application
make start
```

### Health & Readiness

**Application Health:**
- **Healthcheck command**: `curl -f http://localhost:8080/health`
- **Expected response**: `{"status":"ok","timestamp":"2025-01-09T10:30:00Z"}`
- **Timeout**: 30 seconds
- **Retry interval**: 10 seconds

**Database Health:**
- **Healthcheck command**: `mariadb --user=application --password=application --execute="SELECT 1"`
- **Timeout**: 5 seconds
- **Retry interval**: 10 seconds

**Redis Health:**
- **Healthcheck command**: `redis-cli ping`
- **Expected response**: `PONG`
- **Timeout**: 10 seconds
- **Retry interval**: 30 seconds

### Common Tasks

**Create Admin User:**
```bash
# Create initial admin account
EMAIL=admin@example.com PASS=secure_password make user:create
```

**View Application Logs:**
```bash
# All services
make logs

# Specific service
SERVICE=postiz-application.application make logs
```

**Database Console Access:**
```bash
# Connect to database
docker exec -it postiz-application.mariadb mariadb -u application -p application
```

**Clear Cache:**
```bash
# Clear Redis cache
docker exec postiz-application.redis redis-cli FLUSHDB
```

### Sources
- "Postiz Operations Guide" — https://docs.postiz.com/operations/runbook — retrieved 2025-01-09
- "Docker Compose Operations" — https://docs.docker.com/compose/reference/ — retrieved 2025-01-09

<!-- ai-docs-metadata
{"last_audit":"2025-01-09","fingerprints":{"sources":{"https://docs.postiz.com/operations/runbook":"sha256:pending","https://docs.docker.com/compose/reference/":"sha256:pending"},"sections":{"runbook":"sha256:h7i8j9k0"}}}
-->