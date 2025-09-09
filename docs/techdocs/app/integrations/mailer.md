---
title: "Mailer Integration"
description: "Email configuration, SMTP setup, and notification handling."
tags: [email, smtp, notifications]
search: { boost: 3, exclude: false }
icon: material/email
author: "Infrastructure/Platform Team"
date: 2025-01-09
---

**Purpose:** Document email integration and notification requirements.

**Contents**
- [Supported backends](#supported-backends)
- [Connection configuration](#connection-configuration)
- [Notification types](#notification-types)
- [Testing & troubleshooting](#testing--troubleshooting)
- [Sources](#sources)

### Supported backends
Postiz supports multiple email backends for notifications and alerts:

| Backend | Status | Use Case | Notes |
|---------|--------|----------|-------|
| SMTP | ✅ Recommended | Production | Most flexible, works with any provider |
| Mailgun | ✅ Supported | High-volume | API-based, good deliverability |
| Amazon SES | ✅ Supported | AWS environments | Cost-effective for large volumes |
| Log | ⚠️ Development | Development/testing | Writes emails to log files |
| Sendmail | ❌ Not recommended | Legacy systems | Security and reliability concerns |

### Connection configuration
Email connection is configured via environment variables:

```bash
# SMTP Configuration (recommended)
MAIL_MAILER=smtp
MAIL_HOST=smtp.example.com
MAIL_PORT=587
MAIL_USERNAME=postiz@example.com
MAIL_PASSWORD=your_smtp_password
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS=postiz@example.com
MAIL_FROM_NAME="Postiz Notifications"

# Mailgun Configuration
MAIL_MAILER=mailgun
MAILGUN_DOMAIN=mg.example.com
MAILGUN_SECRET=your_mailgun_key

# Amazon SES Configuration
MAIL_MAILER=ses
AWS_ACCESS_KEY_ID=your_aws_key
AWS_SECRET_ACCESS_KEY=your_aws_secret
AWS_DEFAULT_REGION=us-east-1
```

### Notification types
| Notification | Purpose | Frequency | Recipients |
|-------------|---------|-----------|------------|
| Post Failed | Publishing failure alert | Immediate | Post author, workspace admins |
| Account Disconnected | OAuth token expired/revoked | Immediate | Account owner, workspace admins |
| Quota Warning | API rate limit approaching | Daily digest | Workspace admins |
| Campaign Complete | Campaign finished successfully | End of campaign | Campaign managers |
| Security Alert | Suspicious login activity | Immediate | Account owner |
| Backup Status | Database backup success/failure | Daily | System administrators |

### Testing & troubleshooting
**Test Email Configuration:**
```bash
# Send test email via CLI
make run CMD="php artisan mail:test admin@example.com"

# Check mail queue status
make run CMD="php artisan queue:work --queue=mail --once"
```

**Log Analysis:**
```bash
# View mail logs
make logs | grep -i mail

# Check Laravel mail logs
make run CMD="tail -f storage/logs/laravel.log | grep -i mail"
```

**Common Issues:**
| Problem | Symptom | Solution |
|---------|---------|----------|
| SMTP authentication failed | "Authentication failed" in logs | Verify MAIL_USERNAME and MAIL_PASSWORD |
| Connection timeout | Emails not sending, timeout errors | Check MAIL_HOST and MAIL_PORT, firewall rules |
| Emails marked as spam | Low deliverability | Configure SPF, DKIM, DMARC records |
| Queue not processing | Emails stuck in queue | Restart queue worker: `php artisan queue:restart` |

### Sources
- "Postiz Email Configuration" — https://docs.postiz.com/installation/email — retrieved 2025-01-09
- "Laravel Mail Documentation" — https://laravel.com/docs/11.x/mail — retrieved 2025-01-09

<!-- ai-docs-metadata
{"last_audit":"2025-01-09","fingerprints":{"sources":{"https://docs.postiz.com/installation/email":"sha256:pending","https://laravel.com/docs/11.x/mail":"sha256:pending"},"sections":{"mailer":"sha256:j5k6l7m8"}}}
-->