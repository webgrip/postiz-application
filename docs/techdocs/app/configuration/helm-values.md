---
title: "Helm — Used Values"
description: "Only the values from values.yaml that this repo actually uses."
tags: [helm, configuration]
search: { boost: 2, exclude: false }
icon: material/helm
author: "Infrastructure/Platform Team"
date: 2025-01-09
---

**Purpose:** Keep Helm docs tight and truthful.

**Contents**
- [Values we set](#values-we-set)
- [Sources](#sources)

### Values we set
| Key | Example | Notes / Constraints | Source |
|-----|---------|---------------------|--------|
| image.repository | webgrip/postiz-application | Must use org-owned images | "Chart README" — https://github.com/webgrip/postiz-helm-chart/blob/main/README.md — retrieved 2025-01-09 |
| image.tag | 1.8.2 | Pinned versions only, no latest | "Chart README" — https://github.com/webgrip/postiz-helm-chart/blob/main/README.md — retrieved 2025-01-09 |
| service.type | ClusterIP | Internal services only | "Chart README" — https://github.com/webgrip/postiz-helm-chart/blob/main/README.md — retrieved 2025-01-09 |
| ingress.enabled | true | Required for external access | "Chart README" — https://github.com/webgrip/postiz-helm-chart/blob/main/README.md — retrieved 2025-01-09 |
| persistence.enabled | true | Required for data retention | "Chart README" — https://github.com/webgrip/postiz-helm-chart/blob/main/README.md — retrieved 2025-01-09 |

### Sources
- "Postiz Helm Chart README" — https://github.com/webgrip/postiz-helm-chart/blob/main/README.md — retrieved 2025-01-09

<!-- ai-docs-metadata
{"last_audit":"2025-01-09","fingerprints":{"sources":{"https://github.com/webgrip/postiz-helm-chart/blob/main/README.md":"sha256:pending"},"sections":{"helm-values":"sha256:n9o0p1q2"}}}
-->