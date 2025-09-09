---
title: "Observability"
description: "Metrics endpoints, logging configuration, and monitoring setup."
tags: [operations, observability, monitoring]
search: { boost: 3, exclude: false }
icon: material/chart-line
author: "Infrastructure/Platform Team"
date: 2025-01-09
---

**Purpose:** Document observability and monitoring configuration.

**Contents**
- [Metrics collection](#metrics-collection)
- [Logging setup](#logging-setup)
- [Health endpoints](#health-endpoints)
- [Sources](#sources)

### Metrics collection
**Application Metrics:**
- **Endpoint**: `/metrics` (Prometheus format)
- **Key metrics**: Request rate, error rate, response time, queue depth
- **Collection interval**: 30 seconds

**Infrastructure Metrics:**
- **Database**: Connection pool usage, query performance, replication lag
- **Cache**: Hit/miss ratio, memory usage, eviction rate  
- **Web server**: Request count, response codes, upstream health

### Logging setup
**Log Format**: JSON structured logging
**Log Levels**: DEBUG, INFO, WARN, ERROR, FATAL
**Retention**: 30 days for application logs, 7 days for debug logs

**Key Log Sources:**
- Application: `/var/www/app/storage/logs/laravel.log`
- Nginx: `/var/log/nginx/access.log`, `/var/log/nginx/error.log`
- Database: MariaDB error and slow query logs
- Queue: Job processing logs and failures

### Health endpoints
| Endpoint | Purpose | Expected Response |
|----------|---------|-------------------|
| `/health` | Overall application health | `{"status":"ok","timestamp":"..."}`  |
| `/ready` | Readiness for traffic | `{"ready":true,"dependencies":{"db":"ok","redis":"ok"}}` |
| `/metrics` | Prometheus metrics | Prometheus exposition format |

### Sources
- "Postiz Monitoring Guide" — https://docs.postiz.com/operations/monitoring — retrieved 2025-01-09

<!-- ai-docs-metadata
{"last_audit":"2025-01-09","fingerprints":{"sources":{"https://docs.postiz.com/operations/monitoring":"sha256:pending"},"sections":{"observability":"sha256:d5e6f7g8"}}}
-->
