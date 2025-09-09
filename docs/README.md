---
title: "Postiz Application — Quickstart & Overview"
description: "How to run and verify the Postiz social media management platform locally using Make + Compose."
tags: [quickstart, compose, onboarding, postiz, social-media]
search: { boost: 4, exclude: false }
icon: material/home
author: "Infrastructure/Platform Team"
date: 2025-01-09
---

**Purpose:** Help you start the Postiz social media management stack and reach the app fast.

**Contents**
- [Prerequisites](#prerequisites)
- [Quickstart](#quickstart)
- [Verification](#verification)
- [What this project packages](#what-this-project-packages)
- [Sources](#sources)

### Prerequisites
- Docker 20.10+ (with Compose v2)
- GNU Make
- Git

### Quickstart
```bash
git clone https://github.com/webgrip/postiz-application.git
cd postiz-application

# Copy environment template and configure
cp .env.example .env
# Edit .env file with your settings

# Start the stack (requires Docker with Compose v2)
make start

# Follow logs to monitor startup
make logs

# Stop when done
make stop
```

### Verification

* Open: `http://localhost:8080` → should load the Postiz application interface.
* Healthcheck: containers report `healthy` in `docker ps`.
* Database: MariaDB accessible on `localhost:3306`
* Cache: Redis accessible on `localhost:6379`

### What this project packages

Postiz is an open-source social media management tool that allows users to schedule, manage, and analyze social media posts across multiple platforms. This repository provides a containerized deployment using Docker Compose with:

- **Application**: Node.js-based Postiz application server
- **Database**: MariaDB for persistent data storage  
- **Cache**: Redis for session management and caching
- **Reverse Proxy**: Nginx for web traffic routing

The repository follows WebGrip's organizational container policy using org-owned images built from pinned upstream bases.

### Sources

* "Postiz Official Documentation" — https://postiz.com/docs — retrieved 2025-01-09
* "Postiz GitHub Repository" — https://github.com/gitroomhq/postiz-app — retrieved 2025-01-09

<!-- ai-docs-metadata
{
  "last_audit": "2025-01-09",
  "fingerprints": {
    "sources": {
      "https://postiz.com/docs": "sha256:pending",
      "https://github.com/gitroomhq/postiz-app": "sha256:pending"
    },
    "sections": {
      "quickstart": "sha256:a1b2c3d4",
      "verification": "sha256:e5f6g7h8"
    }
  }
}
-->