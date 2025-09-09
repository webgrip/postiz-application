---
title: "Environment Variables"
description: "Required env vars with purpose, defaults, and constraints."
tags: [configuration, environment]
search: { boost: 4, exclude: false }
icon: material/form-textbox
author: "Infrastructure/Platform Team"
date: 2025-01-09
---

**Purpose:** Enumerate required env vars and constraints.

**Contents**
- [Required variables](#required-variables)
- [Optional variables used here](#optional-variables-used-here)
- [Sources](#sources)

### Required variables
| Name | Purpose | Default | Constraints / Format | Source |
|------|---------|---------|----------------------|--------|
| APP_KEY | Application encryption key | (none) | 32 character base64 string | "Postiz Configuration" — https://docs.postiz.com/installation/environment — retrieved 2025-01-09 |
| BASE_URL | Public URL for the application | http://localhost:8080 | Valid HTTP/HTTPS URL | "Postiz Configuration" — https://docs.postiz.com/installation/environment — retrieved 2025-01-09 |
| DB_CONNECTION | Database type | mysql | mysql, pgsql, sqlite | "Postiz Configuration" — https://docs.postiz.com/installation/environment — retrieved 2025-01-09 |
| DB_HOST | Database hostname | postiz-application.mariadb | Hostname or IP address | "Postiz Configuration" — https://docs.postiz.com/installation/environment — retrieved 2025-01-09 |
| DB_DATABASE | Database name | application | Valid database name | "Postiz Configuration" — https://docs.postiz.com/installation/environment — retrieved 2025-01-09 |
| DB_USERNAME | Database username | application | Valid database user | "Postiz Configuration" — https://docs.postiz.com/installation/environment — retrieved 2025-01-09 |
| DB_PASSWORD | Database password | application | Strong password recommended | "Postiz Configuration" — https://docs.postiz.com/installation/environment — retrieved 2025-01-09 |
| REDIS_HOST | Redis hostname | postiz-application.redis | Hostname or IP address | "Postiz Configuration" — https://docs.postiz.com/installation/environment — retrieved 2025-01-09 |

### Optional variables used here
| Name | Purpose | Default | Notes | Source |
|------|---------|---------|-------|--------|
| APP_DEBUG | Enable debug mode | false | Set to true for development only | "Postiz Configuration" — https://docs.postiz.com/installation/environment — retrieved 2025-01-09 |
| APP_ENV | Application environment | production | local, staging, production | "Postiz Configuration" — https://docs.postiz.com/installation/environment — retrieved 2025-01-09 |
| APP_LOCALE | Default language | en | ISO 639-1 language code | "Postiz Configuration" — https://docs.postiz.com/installation/environment — retrieved 2025-01-09 |
| APP_TIMEZONE | Application timezone | Europe/Amsterdam | Valid timezone identifier | "Postiz Configuration" — https://docs.postiz.com/installation/environment — retrieved 2025-01-09 |
| CACHE_DRIVER | Cache backend | redis | redis, memcached, file | "Postiz Configuration" — https://docs.postiz.com/installation/environment — retrieved 2025-01-09 |
| MAIL_MAILER | Mail driver | log | smtp, mailgun, ses, log | "Postiz Configuration" — https://docs.postiz.com/installation/environment — retrieved 2025-01-09 |
| QUEUE_CONNECTION | Queue backend | redis | redis, database, sync | "Postiz Configuration" — https://docs.postiz.com/installation/environment — retrieved 2025-01-09 |
| SESSION_DRIVER | Session storage | redis | redis, database, cookie | "Postiz Configuration" — https://docs.postiz.com/installation/environment — retrieved 2025-01-09 |

### Sources
- "Postiz Environment Configuration" — https://docs.postiz.com/installation/environment — retrieved 2025-01-09
- "Laravel Configuration Reference" — https://laravel.com/docs/11.x/configuration — retrieved 2025-01-09

<!-- ai-docs-metadata
{"last_audit":"2025-01-09","fingerprints":{"sources":{"https://docs.postiz.com/installation/environment":"sha256:pending","https://laravel.com/docs/11.x/configuration":"sha256:pending"},"sections":{"env-table":"sha256:r1s2t3u4"}}}
-->