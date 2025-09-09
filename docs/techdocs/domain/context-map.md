---
title: "Context Map"
description: "Relationships among contexts; upstream and packaging."
tags: [ddd, mapping]
search: { boost: 2, exclude: false }
icon: material/map
author: "Infrastructure/Platform Team"
date: 2025-01-09
---

**Purpose:** Visualize dependencies and translation boundaries.

**Contents**
- [Map](#map)
- [Notes](#notes)
- [Sources](#sources)

### Map
```mermaid
flowchart LR
  CM[Content Management] -- "content events" --> PS[Publication Scheduling]
  PS -- "publication requests" --> PI[Platform Integration]
  PI -- "engagement data" --> AR[Analytics & Reporting]
  UAM[User & Access Management] -- "authz decisions" --> CM
  UAM -- "authz decisions" --> PS
  UAM -- "authz decisions" --> PI
  CamM[Campaign Management] -- "campaign events" --> CM
  CamM -- "schedule coordination" --> PS
  CM -- "ACL" --> INFRA[Infrastructure Packaging]
  PS -- "ACL" --> INFRA
  PI -- "ACL" --> INFRA
```

### Notes
- **Content Management** → **Publication Scheduling**: Publisher/Consumer relationship via domain events
- **Publication Scheduling** → **Platform Integration**: Command/Response pattern for API calls  
- **Analytics & Reporting**: Shared Kernel for cross-context reporting
- **User & Access Management**: Shared Kernel providing security context to all bounded contexts
- **Infrastructure Packaging**: Acts as Anti-Corruption Layer between Postiz domain and our container orchestration

### Sources
- "Postiz Context Integration" — https://docs.postiz.com/architecture/context-integration — retrieved 2025-01-09

<!-- ai-docs-metadata
{"last_audit":"2025-01-09","fingerprints":{"sources":{"https://docs.postiz.com/architecture/context-integration":"sha256:pending"},"sections":{"context-map":"sha256:v7w8x9y0"}}}
-->