# Postiz Docker Setup

This repository provides a Docker Compose setup for running [Postiz](https://postiz.com) - an open-source social media scheduling tool. This setup uses a custom Dockerfile that extends the official Postiz Docker image, allowing for customization while following the [official documentation](https://docs.postiz.com/installation/docker-compose).

## Overview

Postiz is an alternative to Buffer, Hypefury, and other social media scheduling tools. It supports posting to:
- X (Twitter)
- LinkedIn  
- Instagram
- Facebook
- YouTube
- TikTok
- Reddit
- Discord
- Slack
- Mastodon
- Bluesky
- And more...

## Architecture

This setup uses:
- **Custom Application Dockerfile**: Built from the official `ghcr.io/gitroomhq/postiz-app:latest` image
- **PostgreSQL 17**: Database backend (required by Postiz)
- **Redis**: Caching and session storage

```
Internet → http://localhost:5000 → Custom Postiz Build → PostgreSQL + Redis
```

## Prerequisites

- Docker and Docker Compose installed
- At least 2GB RAM and 2 vCPUs recommended
- Network access to pull Docker images

## Quick Start

1. **Clone and navigate to the repository:**
   ```bash
   git clone <repository-url>
   cd postiz-application
   ```

2. **Start the services:**
   ```bash
   make start
   ```

3. **Access Postiz:**
   - Open your browser to [http://localhost:5000](http://localhost:5000)
   - Complete the initial setup and create your account

4. **View logs (optional):**
   ```bash
   make logs
   # Or for specific service:
   make logs SERVICE=postiz
   ```

5. **Stop the services:**
   ```bash
   make stop
   ```

## Architecture

The Docker setup includes:

```
Internet → http://localhost:5000 → Postiz App
                                      ↓
                                  PostgreSQL
                                      ↓
                                    Redis
```

### Services

- **postiz**: Custom application built from official `ghcr.io/gitroomhq/postiz-app:latest` base image
- **postiz-postgres**: PostgreSQL 17 database
- **postiz-redis**: Redis 7.2 for caching and job queues

## Custom Dockerfile Approach

This setup uses a custom Dockerfile (`ops/docker/application/Dockerfile`) that extends the official Postiz image:

```dockerfile
# Use official Postiz image as base
FROM ghcr.io/gitroomhq/postiz-app:latest

# Copy any custom application files if needed
# COPY src/ ./src/

# The official Postiz image already has everything needed
# Just expose the port and use the default entrypoint
EXPOSE 5000
```

### Benefits:
- **Customizable**: Add your own files, configurations, or modifications
- **Version Control**: Your customizations are tracked in your repository  
- **Extensible**: Easy to add custom features while maintaining Postiz compatibility
- **Official Base**: Still uses the official, maintained Postiz image as foundation

### Adding Custom Code:
To add custom application code, uncomment and modify the COPY line:
```dockerfile
COPY src/ ./src/
```

This approach allows you to extend Postiz functionality while maintaining compatibility with official updates.

## Configuration

The application is configured through environment variables in the `docker-compose.yml` file:

### Core Settings
- `MAIN_URL`: Main application URL (http://localhost:5000)
- `FRONTEND_URL`: Frontend URL (http://localhost:5000)
- `NEXT_PUBLIC_BACKEND_URL`: Backend API URL (http://localhost:5000/api)
- `JWT_SECRET`: Secret for JWT tokens (change in production!)
- `DATABASE_URL`: PostgreSQL connection string
- `REDIS_URL`: Redis connection string

### Security
- `NOT_SECURED=true`: Allows HTTP in development (disable for production)
- `DISABLE_REGISTRATION=false`: Allows new user registration

### Social Media Integration

To connect social media platforms, you'll need to obtain API credentials from each platform and add them to the environment variables:

```yaml
# X (Twitter) API
X_API_KEY: "your-api-key"
X_API_SECRET: "your-api-secret"

# LinkedIn API  
LINKEDIN_CLIENT_ID: "your-client-id"
LINKEDIN_CLIENT_SECRET: "your-client-secret"

# Reddit API
REDDIT_CLIENT_ID: "your-client-id"
REDDIT_CLIENT_SECRET: "your-client-secret"

# Add more platforms as needed...
```

See the [Postiz Providers documentation](https://docs.postiz.com/providers) for setup instructions for each platform.

## Storage

The setup uses local storage for uploads:
- `postiz-uploads`: Stores uploaded media files
- `postiz-config`: Application configuration
- `postgres-volume`: Database data
- `postiz-redis-data`: Redis data

All data persists between container restarts.

## Available Commands

Using the provided Makefile:

```bash
# Start services
make start

# Stop services  
make stop

# View logs (all services)
make logs

# View logs for specific service
make logs SERVICE=postiz

# Enter the main container
make enter

# Show help
make help
```

## Development vs Production

### Development Setup (Current)
- Uses HTTP (NOT_SECURED=true)
- Binds to localhost only (127.0.0.1:5000)
- Uses default passwords
- Registration enabled

### Production Considerations
1. **Use HTTPS**: Set up reverse proxy (nginx/Caddy) with SSL certificates
2. **Change secrets**: Update JWT_SECRET and database passwords
3. **Secure network**: Don't expose database/redis ports
4. **Backups**: Implement database backup strategy
5. **Monitoring**: Add logging and monitoring solutions

## Troubleshooting

### Services won't start
1. Check if ports 5000, 5432, 6379 are available:
   ```bash
   netstat -tlnp | grep -E ':(5000|5432|6379)'
   ```

2. Check service health:
   ```bash
   docker compose ps
   ```

3. View service logs:
   ```bash
   make logs SERVICE=postiz-postgres
   make logs SERVICE=postiz-redis
   make logs SERVICE=postiz
   ```

### Application not accessible
1. Ensure services are running:
   ```bash
   docker compose ps
   ```

2. Check application logs:
   ```bash
   make logs SERVICE=postiz
   ```

3. Verify port binding:
   ```bash
   curl http://localhost:5000/health
   ```

### Database connection issues
1. Check PostgreSQL health:
   ```bash
   docker compose exec postiz-postgres pg_isready -U postiz-user -d postiz-db-local
   ```

2. Verify database is accepting connections:
   ```bash
   docker compose logs postiz-postgres
   ```

## Security Considerations

### Development Warning
The current setup includes `NOT_SECURED=true` which disables secure cookie requirements. This is suitable for development but **should not be used in production**.

### Production Security
1. Remove `NOT_SECURED=true`
2. Use HTTPS with valid SSL certificates
3. Change default passwords and secrets
4. Implement proper firewall rules
5. Regular security updates

## Updating

To update to the latest Postiz version:

```bash
# Stop services
make stop

# Pull latest images
docker compose pull

# Start with new images
make start
```

## Support

- **Official Documentation**: [docs.postiz.com](https://docs.postiz.com)
- **GitHub Repository**: [gitroomhq/postiz-app](https://github.com/gitroomhq/postiz-app)
- **Discord Community**: [discord.postiz.com](https://discord.postiz.com)

## License

This Docker setup is provided as-is. Postiz is licensed under AGPL-3.0. See the [official repository](https://github.com/gitroomhq/postiz-app) for full license details.