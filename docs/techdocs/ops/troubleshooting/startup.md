---
title: "Startup Troubleshooting"
description: "Common startup issues and their resolutions."
tags: [troubleshooting, startup]
search: { boost: 3, exclude: false }
icon: material/alert-circle
author: "Infrastructure/Platform Team"
date: 2025-01-09
---

**Purpose:** Diagnose and resolve startup failures.

**Contents**
- [Common Issues](#common-issues)
- [Diagnostic Steps](#diagnostic-steps)
- [Resolution Procedures](#resolution-procedures)
- [Sources](#sources)

### Common Issues

| Symptom | Cause | Fix |
|---------|-------|-----|
| Application container exits immediately | Missing APP_KEY in .env | Generate key: `openssl rand -base64 32` |
| Database connection failed | MariaDB not ready | Wait for health check: `docker ps` shows healthy |
| Redis connection timeout | Redis container not started | Check container status and restart if needed |
| Port 8080 already in use | Another service using port | Change port mapping or stop conflicting service |
| Permission denied on volumes | Docker volume permissions | `docker-compose down -v && docker-compose up` |
| Application shows 500 error | Database not migrated | Run migrations: `make run CMD="php artisan migrate"` |

### Diagnostic Steps

**1. Check Container Status:**
```bash
docker ps --filter "name=postiz-application"
```
Expected: All containers show "healthy" or "Up" status.

**2. Review Startup Logs:**
```bash
make logs
```
Look for error messages, connection failures, or stack traces.

**3. Verify Environment Configuration:**
```bash
# Check .env file exists and has required values
cat .env | grep -E "(APP_KEY|DB_|REDIS_)"
```

**4. Test Database Connectivity:**
```bash
docker exec postiz-application.mariadb mariadb \
  -u application -papplication \
  -e "SELECT 'Database OK' as status;"
```

**5. Test Redis Connectivity:**
```bash
docker exec postiz-application.redis redis-cli ping
```

### Resolution Procedures

**Database Issues:**
```bash
# Reset database and volumes
make stop
docker volume rm postiz-application-mariadb-data
make start

# Run migrations manually if needed
make run CMD="php artisan migrate --force"
```

**Redis Issues:**
```bash
# Clear Redis data and restart
make stop
docker volume rm postiz-application-redis-data
make start
```

**Permission Issues:**
```bash
# Fix volume permissions
make stop
docker-compose down --volumes
docker system prune -f
make start
```

**Complete Reset:**
```bash
# Nuclear option - reset everything
make stop
docker-compose down --volumes --remove-orphans
docker system prune -af
cp .env.example .env
# Edit .env with proper values
make start
```

### Sources
- "Postiz Troubleshooting Guide" — https://docs.postiz.com/troubleshooting/startup — retrieved 2025-01-09
- "Docker Compose Troubleshooting" — https://docs.docker.com/compose/troubleshooting/ — retrieved 2025-01-09

<!-- ai-docs-metadata
{"last_audit":"2025-01-09","fingerprints":{"sources":{"https://docs.postiz.com/troubleshooting/startup":"sha256:pending","https://docs.docker.com/compose/troubleshooting/":"sha256:pending"},"sections":{"startup-troubleshooting":"sha256:l1m2n3o4"}}}
-->