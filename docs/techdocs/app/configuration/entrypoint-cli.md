---
title: "Entrypoint & CLI"
description: "Official entrypoint behavior and supported CLI flags used here."
tags: [entrypoint, cli]
search: { boost: 2, exclude: false }
icon: material/console
author: "Infrastructure/Platform Team"
date: 2025-01-09
---

**Purpose:** Capture startup behavior and commands we rely on.

**Contents**
- [Entrypoint behavior](#entrypoint-behavior)
- [CLI commands & flags used](#cli-commands--flags-used)
- [Sources](#sources)

### Entrypoint behavior
The Postiz application container uses a standard Laravel/PHP-FPM entrypoint that:

1. **Environment validation**: Checks for required environment variables
2. **Database preparation**: Runs migrations if needed (`php artisan migrate --force`)
3. **Cache warming**: Precompiles configuration and routes
4. **Queue worker startup**: Launches background job processors
5. **Web server initialization**: Starts PHP-FPM and serves the application

**Default Command:**
```bash
php artisan serve --host=0.0.0.0 --port=9000
```

### CLI commands & flags used
| Command | Purpose | When Used | Notes |
|---------|---------|-----------|-------|
| `php artisan migrate` | Run database migrations | Container startup, manual updates | Always use `--force` in production |
| `php artisan config:cache` | Cache configuration files | Container startup | Improves performance |
| `php artisan route:cache` | Cache route definitions | Container startup | Reduces route resolution time |
| `php artisan queue:work` | Start background job processor | Container startup | Handles scheduled posts |
| `php artisan ninja:create-account` | Create user accounts | Initial setup, user management | Custom Postiz command |

**Container Overrides:**
```bash
# Run custom command
docker run --rm webgrip/postiz-application:latest php artisan --version

# Interactive shell
docker run --rm -it webgrip/postiz-application:latest /bin/bash

# Database migrations only
docker run --rm webgrip/postiz-application:latest php artisan migrate --force
```

### Sources
- "Postiz Docker Configuration" — https://docs.postiz.com/installation/docker — retrieved 2025-01-09
- "Laravel Artisan Documentation" — https://laravel.com/docs/11.x/artisan — retrieved 2025-01-09

<!-- ai-docs-metadata
{"last_audit":"2025-01-09","fingerprints":{"sources":{"https://docs.postiz.com/installation/docker":"sha256:pending","https://laravel.com/docs/11.x/artisan":"sha256:pending"},"sections":{"entrypoint":"sha256:x3y4z5a6"}}}
-->