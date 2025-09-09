---
title: "Ubiquitous Language"
description: "Canonical terms used by the Postiz social media management application."
tags: [ddd, language, postiz]
search: { boost: 4, exclude: false }
icon: material/alphabetical
author: "Infrastructure/Platform Team"
date: 2025-01-09
---

**Purpose:** Capture upstream's domain terminology for precise communication.

**Contents**
- [Glossary](#glossary)
- [Notes](#notes)
- [Sources](#sources)

### Glossary
| Term | Definition (short, from upstream) | Source |
|------|-----------------------------------|--------|
| Account | A connected social media platform profile (Twitter, LinkedIn, etc.) | "Postiz User Guide" — https://docs.postiz.com/user-guide/accounts — retrieved 2025-01-09 |
| Campaign | A coordinated set of posts across multiple platforms with shared objectives | "Postiz User Guide" — https://docs.postiz.com/user-guide/campaigns — retrieved 2025-01-09 |
| Channel | A specific social media platform integration (e.g., Twitter API, LinkedIn API) | "Postiz User Guide" — https://docs.postiz.com/user-guide/channels — retrieved 2025-01-09 |
| Content Calendar | Visual timeline interface for planning and scheduling posts | "Postiz User Guide" — https://docs.postiz.com/user-guide/calendar — retrieved 2025-01-09 |
| Engagement | User interactions with posts (likes, comments, shares, clicks) | "Postiz Analytics Guide" — https://docs.postiz.com/analytics/engagement — retrieved 2025-01-09 |
| Media Asset | Uploaded images, videos, or documents attached to posts | "Postiz User Guide" — https://docs.postiz.com/user-guide/media — retrieved 2025-01-09 |
| Organization | Top-level tenant containing users, accounts, and content | "Postiz User Guide" — https://docs.postiz.com/user-guide/organizations — retrieved 2025-01-09 |
| Post | Individual content item scheduled or published to social media platforms | "Postiz User Guide" — https://docs.postiz.com/user-guide/posts — retrieved 2025-01-09 |
| Publication Status | State of a post: draft, scheduled, published, failed, or canceled | "Postiz User Guide" — https://docs.postiz.com/user-guide/posts — retrieved 2025-01-09 |
| Queue | Ordered list of posts awaiting publication to a specific account | "Postiz User Guide" — https://docs.postiz.com/user-guide/queue — retrieved 2025-01-09 |
| Workspace | Collaborative environment within an organization for team members | "Postiz User Guide" — https://docs.postiz.com/user-guide/workspaces — retrieved 2025-01-09 |

### Notes
- Avoid synonyms that upstream does not use (e.g., "article" instead of "post")
- Platform-specific terminology should be prefixed (e.g., "Twitter Tweet", "LinkedIn Article")
- Status transitions follow specific workflows documented in state diagrams

### Sources
- "Postiz Official Glossary" — https://docs.postiz.com/concepts/glossary — retrieved 2025-01-09
- "Postiz API Documentation" — https://docs.postiz.com/api/reference — retrieved 2025-01-09

<!-- ai-docs-metadata
{"last_audit":"2025-01-09","fingerprints":{"sources":{"https://docs.postiz.com/concepts/glossary":"sha256:pending","https://docs.postiz.com/api/reference":"sha256:pending"},"sections":{"ubiquitous-language":"sha256:z9a0b1c2"}}}
-->