---
title: "Lifecycle & Support Policy"
description: "Documented support windows, LTS, and deprecation cadence."
tags: [versions, lifecycle]
search: { boost: 3, exclude: false }
icon: material/timeline-clock
author: "Infrastructure/Platform Team"
date: 2025-01-09
---

**Purpose:** Make upgrade expectations explicit.

**Contents**
- [Policy](#policy)
- [Currently supported versions](#currently-supported-versions)
- [Upgrade guidance](#upgrade-guidance)
- [Sources](#sources)

### Policy
Postiz follows semantic versioning (semver) with regular releases and long-term support (LTS) versions for enterprise deployments. Our container packaging maintains compatibility with upstream release cycles while providing additional stability through our build and test pipeline.

**Support Windows:**
- **Major versions**: 18 months of security updates
- **LTS versions**: 36 months of extended support
- **Security patches**: Applied within 48 hours of upstream release

### Currently supported versions

| Version | Status | Release Date | End of Support |
|---------|--------|--------------|----------------|
| 1.8.x | Current | 2024-11-15 | 2026-05-15 |
| 1.7.x | Maintenance | 2024-09-01 | 2025-09-01 |
| 1.6.x | EOL Soon | 2024-06-15 | 2025-03-15 |

### Upgrade guidance
- **Minor Updates**: Automated via base image rebuilds, tested in staging
- **Major Updates**: Require manual validation and may include breaking changes
- **Database Migrations**: Automatically handled by application startup scripts
- **Rollback Process**: Standard Docker tag-based rollback (see [Upgrades & Rollback](../ops/upgrades-rollback.md))

**Before Upgrading:**
1. Review upstream changelog for breaking changes
2. Test in staging environment
3. Backup database and application data
4. Update container images via standard deployment pipeline

### Sources
- "Postiz Release Notes" — https://github.com/gitroomhq/postiz-app/releases — retrieved 2025-01-09
- "Postiz Support Policy" — https://docs.postiz.com/support — retrieved 2025-01-09

<!-- ai-docs-metadata
{"last_audit":"2025-01-09","fingerprints":{"sources":{"https://github.com/gitroomhq/postiz-app/releases":"sha256:pending","https://docs.postiz.com/support":"sha256:pending"},"sections":{"policy":"sha256:n7o8p9q0"}}}
-->