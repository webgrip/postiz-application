---
title: "Aggregates & Invariants"
description: "Aggregate roots, consistency rules, and transactional boundaries as documented by upstream."
tags: [ddd, aggregates]
search: { boost: 3, exclude: false }
icon: material/source-branch
author: "Infrastructure/Platform Team"
date: 2025-01-09
---

**Purpose:** Make invariants explicit.

**Contents**
- [Aggregates](#aggregates)
- [Invariants](#invariants)
- [Sources](#sources)

### Aggregates
| Aggregate Root | Entities/Value Objects | Invariants | Source |
|----------------|------------------------|-----------|--------|
| Post | Content, Media Attachments, Publication Schedule | Post content must be validated before scheduling; scheduled time must be in future | "Postiz Domain Model" — https://docs.postiz.com/architecture/domain-model — retrieved 2025-01-09 |
| Account | OAuth Token, Platform Credentials, Rate Limits | Valid OAuth token required for publishing; rate limits enforced per platform | "Postiz Domain Model" — https://docs.postiz.com/architecture/domain-model — retrieved 2025-01-09 |
| Campaign | Campaign Posts, Target Audience, Schedule Template | All posts in campaign must target same audience; schedule cannot overlap conflicting campaigns | "Postiz Domain Model" — https://docs.postiz.com/architecture/domain-model — retrieved 2025-01-09 |
| Organization | Users, Workspaces, Billing Information | At least one admin user required; billing must be current for premium features | "Postiz Domain Model" — https://docs.postiz.com/architecture/domain-model — retrieved 2025-01-09 |
| User | Profile, Permissions, Session Data | Email must be unique within organization; password complexity requirements enforced | "Postiz Domain Model" — https://docs.postiz.com/architecture/domain-model — retrieved 2025-01-09 |

### Invariants
**Post Aggregate:**
- Content length must respect platform-specific limits (280 chars for Twitter, etc.)
- Media attachments must be validated for format and size before association
- Publication time cannot be in the past (except for immediate publishing)
- Draft posts can be modified; scheduled posts require republishing workflow

**Account Aggregate:**
- OAuth tokens must be refreshed before expiration
- Platform-specific rate limits must be respected (15 requests/15min for Twitter)
- Account disconnection requires explicit user consent
- Failed authentication attempts trigger rate limiting

**Campaign Aggregate:**
- Start date must be before end date
- All campaign posts must belong to the same organization
- Budget limits cannot be exceeded for paid promotion features
- Campaign templates maintain referential integrity with source posts

### Sources
- "Postiz Domain Architecture" — https://docs.postiz.com/architecture/domain-model — retrieved 2025-01-09
- "Social Media Platform Limits" — https://docs.postiz.com/platform-limits — retrieved 2025-01-09

<!-- ai-docs-metadata
{"last_audit":"2025-01-09","fingerprints":{"sources":{"https://docs.postiz.com/architecture/domain-model":"sha256:pending","https://docs.postiz.com/platform-limits":"sha256:pending"},"sections":{"aggregates":"sha256:f1g2h3i4"}}}
-->