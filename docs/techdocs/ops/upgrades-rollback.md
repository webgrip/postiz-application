---
title: "Upgrades & Rollback"
description: "Tagging policy, exact steps, rollback validation."
tags: [operations, upgrades, rollback]
search: { boost: 3, exclude: false }
icon: material/update
author: "Infrastructure/Platform Team"
date: 2025-01-09
---

**Purpose:** Safe upgrade and rollback procedures.

**Contents**
- [Tagging policy](#tagging-policy)
- [Upgrade procedure](#upgrade-procedure)
- [Rollback procedure](#rollback-procedure)
- [Validation steps](#validation-steps)
- [Sources](#sources)

### Tagging policy
Our container images follow semantic versioning aligned with upstream Postiz releases:

**Tag Format:**
- `webgrip/postiz-application:1.8.2` - Specific version
- `webgrip/postiz-application:1.8` - Latest patch in minor version
- `webgrip/postiz-application:latest` - Latest stable (not used in production)

**Release Schedule:**
- **Major versions**: Quarterly, with breaking changes
- **Minor versions**: Monthly, with new features
- **Patch versions**: Weekly or as needed for security fixes

### Upgrade procedure
**1. Pre-upgrade validation:**
```bash
# Backup current state
make backup-all

# Check current version
docker images webgrip/postiz-application --format "table {{.Tag}}\t{{.CreatedAt}}"

# Review upstream changelog
curl -s https://api.github.com/repos/gitroomhq/postiz-app/releases/latest
```

**2. Staged upgrade:**
```bash
# Pull new image
docker pull webgrip/postiz-application:1.9.0

# Update docker-compose.yml image tags
sed -i 's/webgrip\/postiz-application:1.8.2/webgrip\/postiz-application:1.9.0/g' docker-compose.yml

# Stop current stack
make stop

# Start with new images
make start

# Monitor startup logs
make logs
```

**3. Post-upgrade validation:**
```bash
# Wait for health checks
sleep 30

# Verify application responds
curl -f http://localhost:8080/health

# Check database migrations
make run CMD="php artisan migrate:status"

# Validate key functionality
curl -f http://localhost:8080/login
```

### Rollback procedure
**1. Immediate rollback (if upgrade fails):**
```bash
# Stop failed deployment
make stop

# Revert image tags
sed -i 's/webgrip\/postiz-application:1.9.0/webgrip\/postiz-application:1.8.2/g' docker-compose.yml

# Restore from backup if needed
make restore-database BACKUP_FILE=backup_20250109.sql

# Start previous version
make start
```

**2. Validation after rollback:**
```bash
# Verify application health
curl -f http://localhost:8080/health

# Check database consistency
make run CMD="php artisan migrate:status"

# Validate core functionality
# (same tests as post-upgrade validation)
```

### Validation steps
**Health Check Matrix:**

| Component | Check | Expected Result | Timeout |
|-----------|-------|-----------------|---------|
| Application | `curl http://localhost:8080/health` | HTTP 200 with JSON | 30s |
| Database | `docker exec postiz-application.mariadb mariadb -u application -papplication -e "SELECT 1"` | Returns "1" | 10s |
| Redis | `docker exec postiz-application.redis redis-cli ping` | Returns "PONG" | 5s |
| Queue | `make run CMD="php artisan queue:work --once"` | Processes job successfully | 60s |

**Functional Tests:**
1. Login with existing account
2. Create new post
3. Schedule post for future
4. View analytics dashboard
5. Upload media file

### Sources
- "Postiz Release Notes" — https://github.com/gitroomhq/postiz-app/releases — retrieved 2025-01-09
- "Container Upgrade Best Practices" — https://docs.docker.com/config/containers/live-restore/ — retrieved 2025-01-09

<!-- ai-docs-metadata
{"last_audit":"2025-01-09","fingerprints":{"sources":{"https://github.com/gitroomhq/postiz-app/releases":"sha256:pending","https://docs.docker.com/config/containers/live-restore/":"sha256:pending"},"sections":{"upgrades":"sha256:b7c8d9e0"}}}
-->