---
title: "Containerization & Compose Policy"
description: "Org-owned images from pinned upstream bases; official app image preferred, source-build only if unsupported or unavailable."
tags: [operations, containerization, docker, compose, policy]
search: { boost: 4, exclude: false }
icon: material/steam
author: "Infrastructure/Platform Team"
date: 2025-01-09
---

**Purpose:** Record our image and Compose policies for this repo.

**Contents**
- [Policy](#policy)
- [Rationale](#rationale)
- [Consequences](#consequences)
- [Sources](#sources)

## Policy
- Use **org-owned images** derived from **pinned upstream bases** (no `latest`).
- Prefer **official upstream app image**; fall back to source-build only when upstream image is unavailable or missing required features.
- Compose uses **only our images**; tag pinning enforced.
- Healthchecks and ports match upstream guidance.

## Rationale
**Security & Reproducibility:** Org-owned images provide security scanning, SBOM generation, and vulnerability management. Pinned base images ensure reproducible builds and controlled updates.

**Supply Chain Control:** By building from known upstream sources, we maintain visibility into dependencies and can respond quickly to security issues.

**Operational Consistency:** Standardized image patterns across all applications simplify deployment, monitoring, and troubleshooting procedures.

## Consequences

**Pros:**
- Enhanced security posture through controlled image supply chain
- Reproducible deployments across environments  
- Centralized vulnerability management and patching
- Clear upgrade and rollback procedures

**Cons:**
- Additional maintenance overhead for image rebuilds
- Potential lag behind upstream releases
- Custom build infrastructure requirements
- CVE patches require base image bumping and rebuild cycle

**Upgrade Cadence:** Monthly base image updates; security patches within 48 hours of upstream release.

## Sources
- "Docker Official Images Best Practices" — https://docs.docker.com/docker-hub/official_images/ — retrieved 2025-01-09
- "Postiz Docker Documentation" — https://github.com/gitroomhq/postiz-app/blob/main/docker/README.md — retrieved 2025-01-09

<!-- ai-docs-metadata
{
  "last_audit": "2025-01-09",
  "fingerprints": {
    "sources": {
      "https://docs.docker.com/docker-hub/official_images/": "sha256:pending",
      "https://github.com/gitroomhq/postiz-app/blob/main/docker/README.md": "sha256:pending"
    },
    "sections": {
      "policy": "sha256:b2c3d4e5"
    }
  }
}
-->