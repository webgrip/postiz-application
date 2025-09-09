---
title: "Application Overview"
description: "What Postiz is and what this repository packages."
tags: [overview, upstream, postiz]
search: { boost: 4, exclude: false }
icon: material/information
author: "Infrastructure/Platform Team"
date: 2025-01-09
---

**Purpose:** Summarize the app and packaging approach.

**Contents**
- [What is Postiz?](#what-is-postiz)
- [Core features](#core-features)
- [Supported versions & lifecycle](#supported-versions--lifecycle)
- [Sources](#sources)

### What is Postiz?
Postiz is an open-source social media management platform that enables users to schedule, publish, and analyze content across multiple social media platforms. This repository provides a containerized deployment using Docker Compose with MariaDB, Redis, and Nginx for a complete production-ready stack.

### Core features
- **Multi-Platform Publishing**: Schedule posts across Twitter, LinkedIn, Facebook, Instagram, and more
- **Content Calendar**: Visual planning interface for social media campaigns
- **Analytics & Insights**: Track engagement metrics and performance analytics
- **Team Collaboration**: Multi-user workspace with role-based permissions
- **Bulk Operations**: Import and schedule multiple posts at once
- **Custom Branding**: White-label options for agencies and enterprises
- **API Integration**: RESTful API for custom integrations and automation

### Supported versions & lifecycle
- **Current Version**: Based on Postiz v1.x (latest stable)
- **Update Cadence**: Monthly releases with security patches as needed
- **LTS Policy**: Follows upstream LTS policy with 12-month support windows
- **EOL Timeline**: Documented in [Lifecycle](lifecycle.md) section

### Sources
- "Postiz Official Website" — https://postiz.com — retrieved 2025-01-09
- "Postiz GitHub Repository" — https://github.com/gitroomhq/postiz-app — retrieved 2025-01-09
- "Postiz Documentation" — https://docs.postiz.com — retrieved 2025-01-09

<!-- ai-docs-metadata
{"last_audit":"2025-01-09","fingerprints":{"sources":{"https://postiz.com":"sha256:pending","https://github.com/gitroomhq/postiz-app":"sha256:pending","https://docs.postiz.com":"sha256:pending"},"sections":{"overview":"sha256:j3k4l5m6"}}}
-->