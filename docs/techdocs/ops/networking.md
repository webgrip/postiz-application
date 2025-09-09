---
title: "Networking"
description: "Ports, TLS, proxies, headers, timeouts."
tags: [operations, networking]
search: { boost: 3, exclude: false }
icon: material/network
author: "Infrastructure/Platform Team"
date: 2025-01-09
---

**Purpose:** Document network configuration and requirements.

**Contents**
- [Port mapping](#port-mapping)
- [TLS configuration](#tls-configuration)
- [Proxy settings](#proxy-settings)
- [Sources](#sources)

### Port mapping
| Service | Internal Port | External Port | Protocol | Purpose |
|---------|---------------|---------------|----------|---------|
| Nginx | 80 | 8080 | HTTP | Web traffic, reverse proxy |
| Application | 9000 | - | HTTP | PHP-FPM application server |
| MariaDB | 3306 | 3306 | TCP | Database connections |
| Redis | 6379 | - | TCP | Cache and session storage |

### TLS configuration
- **Termination**: At nginx reverse proxy
- **Certificates**: Let's Encrypt via cert-manager (Kubernetes) or manual (Docker Compose)
- **Ciphers**: TLS 1.2+ with modern cipher suites
- **HSTS**: Enabled with 1-year max-age

### Proxy settings
**Nginx Configuration:**
- **Upstream**: Application server on port 9000
- **Timeouts**: 60s read, 60s send, 30s connect
- **Buffer sizes**: 4k proxy buffers
- **Headers**: X-Forwarded-For, X-Real-IP preserved

### Sources
- "Postiz Networking Guide" — https://docs.postiz.com/installation/networking — retrieved 2025-01-09

<!-- ai-docs-metadata
{"last_audit":"2025-01-09","fingerprints":{"sources":{"https://docs.postiz.com/installation/networking":"sha256:pending"},"sections":{"networking":"sha256:z1a2b3c4"}}}
-->