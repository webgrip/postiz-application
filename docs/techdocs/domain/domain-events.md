---
title: "Domain Events"
description: "Notable events that Postiz documents (webhooks, audit logs, callbacks)."
tags: [ddd, events]
search: { boost: 3, exclude: false }
icon: material/flash
author: "Infrastructure/Platform Team"
date: 2025-01-09
---

**Purpose:** Clarify observable domain signals.

**Contents**
- [Events](#events)
- [Delivery/Contracts](#deliverycontracts)
- [Sources](#sources)

### Events
| Name | When it fires | Payload essentials | Consumers | Source |
|------|----------------|--------------------|-----------|--------|
| PostScheduled | Post is queued for future publication | post_id, scheduled_time, platform | Queue workers, analytics | "Postiz Events Documentation" — https://docs.postiz.com/api/webhooks — retrieved 2025-01-09 |
| PostPublished | Post successfully published to platform | post_id, platform, published_url | Analytics, notifications | "Postiz Events Documentation" — https://docs.postiz.com/api/webhooks — retrieved 2025-01-09 |
| PostFailed | Post publication failed | post_id, platform, error_message | Error handlers, notifications | "Postiz Events Documentation" — https://docs.postiz.com/api/webhooks — retrieved 2025-01-09 |
| AccountConnected | Social media account linked | account_id, platform, user_id | Audit log, welcome flow | "Postiz Events Documentation" — https://docs.postiz.com/api/webhooks — retrieved 2025-01-09 |
| CampaignCompleted | All campaign posts processed | campaign_id, total_posts, success_count | Reporting, notifications | "Postiz Events Documentation" — https://docs.postiz.com/api/webhooks — retrieved 2025-01-09 |

### Delivery/Contracts
**Event Bus:** Internal events use Laravel's event system with Redis queue backend
**Webhooks:** External consumers can subscribe to events via HTTP webhooks
**Retry Policy:** Failed webhook deliveries retry with exponential backoff (1m, 5m, 15m, 1h)
**Idempotency:** All events include unique event_id for deduplication

### Sources
- "Postiz Webhooks Documentation" — https://docs.postiz.com/api/webhooks — retrieved 2025-01-09

<!-- ai-docs-metadata
{"last_audit":"2025-01-09","fingerprints":{"sources":{"https://docs.postiz.com/api/webhooks":"sha256:pending"},"sections":{"domain-events":"sha256:r3s4t5u6"}}}
-->