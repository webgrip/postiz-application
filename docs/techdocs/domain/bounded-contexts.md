---
title: "Bounded Contexts"
description: "Logical boundaries inferred from Postiz features and modules."
tags: [ddd, contexts, postiz]
search: { boost: 3, exclude: false }
icon: material/vector-square
author: "Infrastructure/Platform Team"
date: 2025-01-09
---

**Purpose:** Identify contexts to avoid model bleed.

**Contents**
- [Contexts](#contexts)
- [Shared Kernel / ACL](#shared-kernel--acl)
- [Sources](#sources)

### Contexts
| Context | Responsibility | Key Models/Terms | Upstream Artifact | Source |
|--------|-----------------|------------------|-------------------|--------|
| Content Management | Creating, editing, and organizing posts and media | Post, Media Asset, Content Calendar, Draft | Content module | "Postiz Architecture" — https://docs.postiz.com/architecture/modules — retrieved 2025-01-09 |
| Publication Scheduling | Timing and queue management for post delivery | Queue, Schedule, Publication Status, Retry Policy | Scheduler module | "Postiz Architecture" — https://docs.postiz.com/architecture/modules — retrieved 2025-01-09 |
| Platform Integration | Social media APIs and account connections | Channel, Account, OAuth Token, Platform Capability | Integrations module | "Postiz Architecture" — https://docs.postiz.com/architecture/modules — retrieved 2025-01-09 |
| Analytics & Reporting | Metrics collection and performance analysis | Engagement, Metrics, Report, Analytics Dashboard | Analytics module | "Postiz Architecture" — https://docs.postiz.com/architecture/modules — retrieved 2025-01-09 |
| User & Access Management | Authentication, authorization, and team collaboration | User, Organization, Workspace, Role, Permission | Auth module | "Postiz Architecture" — https://docs.postiz.com/architecture/modules — retrieved 2025-01-09 |
| Campaign Management | Multi-platform coordinated content strategies | Campaign, Campaign Template, Cross-Platform Sync | Campaigns module | "Postiz Architecture" — https://docs.postiz.com/architecture/modules — retrieved 2025-01-09 |

### Shared Kernel / ACL
**Shared Kernel:**
- **Event Bus**: Cross-context communication via domain events
- **Audit Log**: Common audit trail for compliance and debugging
- **Configuration**: Shared application settings and feature flags

**Anti-Corruption Layers:**
- **Platform APIs**: Each social media platform has unique APIs and rate limits, requiring dedicated ACLs to translate between Postiz domain models and platform-specific formats
- **Infrastructure Packaging**: This repository acts as an ACL between Postiz domain models and our container orchestration infrastructure (Docker Compose, Kubernetes)

**Context Relationships:**
- Content Management → Publication Scheduling (downstream dependency)
- Publication Scheduling → Platform Integration (downstream dependency)  
- Analytics & Reporting ← All contexts (upstream aggregation)
- User & Access Management → All contexts (security cross-cut)

### Sources
- "Postiz Architecture Overview" — https://docs.postiz.com/architecture/overview — retrieved 2025-01-09
- "Postiz Module Structure" — https://docs.postiz.com/architecture/modules — retrieved 2025-01-09

<!-- ai-docs-metadata
{"last_audit":"2025-01-09","fingerprints":{"sources":{"https://docs.postiz.com/architecture/overview":"sha256:pending","https://docs.postiz.com/architecture/modules":"sha256:pending"},"sections":{"bounded-contexts":"sha256:d3e4f5g6"}}}
-->