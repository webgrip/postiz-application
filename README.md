# Postiz Application Docker Setup

## Badges

[![Template Sync](https://img.shields.io/github/actions/workflow/status/webgrip/application-template/sync-template-files.yml?label=template%20sync&style=flat-square)](https://github.com/webgrip/application-template/actions/workflows/sync-template-files.yml)
[![License](https://img.shields.io/github/license/webgrip/application-template?style=flat-square)](LICENSE)
[![Conventional Commits](https://img.shields.io/badge/Conventional%20Commits-1.0.0-orange.svg?style=flat-square)](https://www.conventionalcommits.org)
[![SemVer](https://img.shields.io/badge/semver-2.0.0-blue?style=flat-square)](https://semver.org)
[![Dockerized](https://img.shields.io/badge/containerized-docker-2496ED?logo=docker&logoColor=white&style=flat-square)](https://www.docker.com/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen?style=flat-square)](https://github.com/webgrip/application-template/issues)

> Docker setup for [Postiz](https://postiz.com) - an open-source social media scheduling tool using the official Docker images.

---

## Quick Start

Get Postiz running with Docker in minutes:

```bash
# Start the services
make start

# Access Postiz at http://localhost:5000
# Complete setup and create your account

# View logs
make logs

# Stop when done
make stop
```

## What is Postiz?

Postiz is an open-source alternative to Buffer, Hypefury, and other social media scheduling tools. It supports:

- **Social Platforms**: X (Twitter), LinkedIn, Instagram, Facebook, YouTube, TikTok, Reddit, Discord, Slack, Mastodon, Bluesky, and more
- **AI Features**: AI-powered content generation and optimization
- **Team Collaboration**: Multi-user support with role-based permissions  
- **Analytics**: Comprehensive performance tracking and insights
- **Self-Hosted**: Full control over your data and deployment

## Architecture

This Docker setup provides a complete Postiz environment:

```
Internet → http://localhost:5000 → Postiz App (official image)
                                      ↓
                                  PostgreSQL 17
                                      ↓
                                    Redis 7.2
```

### Services

- **postiz**: Main application using `ghcr.io/gitroomhq/postiz-app:latest`
- **postiz-postgres**: PostgreSQL database for persistent storage
- **postiz-redis**: Redis for caching and background job queues

## Configuration

The setup follows the [official Postiz Docker documentation](https://docs.postiz.com/installation/docker-compose) with sensible defaults for local development:

### Core Features
- ✅ **Web Interface**: Complete Postiz UI at http://localhost:5000
- ✅ **API Access**: RESTful API for integrations
- ✅ **File Storage**: Local file storage for media uploads
- ✅ **Database**: PostgreSQL with persistent volumes
- ✅ **Background Jobs**: Redis-powered job processing
- ✅ **Health Checks**: Built-in service health monitoring

### Social Media Integration

To connect social media platforms, you'll need API credentials from each service. See the [Providers Documentation](https://docs.postiz.com/providers) for setup guides:

- **X (Twitter)**: API v2 keys and tokens
- **LinkedIn**: OAuth client credentials  
- **Facebook/Instagram**: Meta developer app
- **YouTube**: Google API credentials
- **And more...**

## Available Commands

| Command | Description |
|---------|-------------|
| `make start` | Start all services in background |
| `make stop` | Stop and remove containers |
| `make logs` | Follow logs for all services |
| `make logs SERVICE=postiz` | Follow logs for specific service |
| `make enter` | Enter the main container |
| `make help` | Show all available commands |

## Documentation

- **📖 [Complete Docker Setup Guide](docs/DOCKER_SETUP.md)** - Detailed setup and configuration
- **🔧 [Troubleshooting Guide](docs/DOCKER_SETUP.md#troubleshooting)** - Common issues and solutions
- **🔒 [Security Considerations](docs/DOCKER_SETUP.md#security-considerations)** - Development vs production
- **📚 [Official Postiz Docs](https://docs.postiz.com)** - Full Postiz documentation

## Security Note

This setup includes `NOT_SECURED=true` for development convenience. **Do not use this configuration in production.** For production deployments:

1. Remove `NOT_SECURED=true`  
2. Use HTTPS with valid SSL certificates
3. Change default passwords and secrets
4. Implement proper access controls

See the [Security Considerations](docs/DOCKER_SETUP.md#security-considerations) section for full details.

## Updating

To update to the latest Postiz version:

```bash
make stop
docker compose pull
make start
```

## Encrypted Secrets (Optional)

For advanced deployments with encrypted secrets:

```bash
# Initialize encryption
make init-encrypt

# Add secrets to ops/secrets/postiz-application-secrets/values.dec.yaml
# Then encrypt them
make encrypt-secrets SECRETS_DIR=./ops/secrets/postiz-application-secrets
```

## Template Synchronization

This repository can automatically sync certain files to application repositories. To enable template sync, add the `application` topic to your repository.

**Synced Files**: GitHub workflows, configuration files, developer tooling

For details, see the [Template Sync Documentation](docs/techdocs/template-sync.md).

## Contributing

Contributions welcome! Please:

1. Open an issue describing the change
2. Use Conventional Commits for branch + commit messages
3. Add / adjust tests where behavior changes
4. Update docs (README / TechDocs / ADRs) when altering architecture

## Support

- **Official Postiz Documentation**: [docs.postiz.com](https://docs.postiz.com)
- **GitHub Repository**: [gitroomhq/postiz-app](https://github.com/gitroomhq/postiz-app)
- **Discord Community**: [discord.postiz.com](https://discord.postiz.com)

## License

Distributed under the terms of the MIT license. See `LICENSE` for details.

*This Docker setup uses the official Postiz images. Postiz itself is licensed under AGPL-3.0.*
