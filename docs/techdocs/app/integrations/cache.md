---
title: "Cache Integration"
description: "Redis configuration, connection format, and operational requirements."
tags: [cache, redis, performance]
search: { boost: 3, exclude: false }
icon: material/memory
author: "Infrastructure/Platform Team"
date: 2025-01-09
---

**Purpose:** Document cache integration and operational requirements.

**Contents**
- [Supported backends](#supported-backends)
- [Connection configuration](#connection-configuration)
- [Container interface](#container-interface)
- [Performance tuning](#performance-tuning)
- [Sources](#sources)

### Supported backends
Postiz supports multiple cache backends with Redis as the recommended choice:

| Backend | Status | Use Case | Notes |
|---------|--------|----------|-------|
| Redis | ✅ Recommended | Production | Session storage, queue management, application cache |
| Memcached | ✅ Supported | High-memory environments | Cache-only, no persistence |
| File | ⚠️ Development | Development/testing | Disk-based, single server only |
| Database | ❌ Not recommended | Fallback only | Poor performance characteristics |

### Connection configuration
Cache connection is configured via environment variables:

```bash
CACHE_DRIVER=redis
CACHE_HOST=postiz-application.redis
CACHE_PORT=6379
CACHE_PASSWORD=

# Redis-specific settings
REDIS_HOST=postiz-application.redis
REDIS_PORT=6379
REDIS_PASSWORD=
```

**Connection String Format:**
- **Redis**: `redis://[:password@]host[:port][/database]`
- **Memcached**: `memcached://host:port`

### Container interface
| Aspect | Value / Path | Notes |
|-------|---------------|-------|
| Ports | 6379/tcp | Standard Redis port |
| Healthcheck | `redis-cli ping` | Must return PONG |
| Volumes | `/data` | RDB/AOF persistence files |
| Memory Limit | 256MB | Configured via container limits |
| Persistence | RDB + AOF | Mixed persistence for durability |

### Performance tuning
**Redis Configuration:**
```redis
# Memory management
maxmemory 256mb
maxmemory-policy allkeys-lru

# Persistence
save 900 1
save 300 10
save 60 10000

# AOF for durability
appendonly yes
appendfsync everysec
```

**Monitoring Commands:**
```bash
# Memory usage
docker exec postiz-application.redis redis-cli INFO memory

# Hit/miss ratio
docker exec postiz-application.redis redis-cli INFO stats | grep keyspace

# Connected clients
docker exec postiz-application.redis redis-cli INFO clients
```

### Sources
- "Postiz Cache Configuration" — https://docs.postiz.com/installation/cache — retrieved 2025-01-09
- "Redis Docker Hub" — https://hub.docker.com/_/redis — retrieved 2025-01-09

<!-- ai-docs-metadata
{"last_audit":"2025-01-09","fingerprints":{"sources":{"https://docs.postiz.com/installation/cache":"sha256:pending","https://hub.docker.com/_/redis":"sha256:pending"},"sections":{"cache":"sha256:t9u0v1w2"}}}
-->