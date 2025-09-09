# Postiz Helm Chart

This directory contains the Helm chart configuration for deploying Postiz, an open-source social media scheduling tool, on Kubernetes.

## Overview

This chart uses the **official Postiz Helm chart** from [gitroomhq/postiz-helmchart](https://github.com/gitroomhq/postiz-helmchart) as a dependency. It provides a complete, production-ready deployment including:

- **Postiz Application**: The main social media scheduling application
- **PostgreSQL**: Database for storing posts, users, and configuration
- **Redis**: Cache and session storage
- **Ingress**: External access with TLS termination
- **Persistent Storage**: Data persistence for database and uploads

## Prerequisites

Before deploying Postiz, ensure you have:

- **Kubernetes cluster** (v1.19+)
- **Helm** (v3.0+)
- **Ingress controller** (e.g., NGINX, Traefik)
- **Cert-manager** (optional, for automatic TLS certificates)
- **StorageClass** configured for persistent volumes

## Quick Start

### 1. Update Dependencies

First, download the official Postiz chart:

```bash
cd ops/helm/postiz
helm dependency update
```

### 2. Configure Values

Copy the example values and customize for your environment:

```bash
cp values.yaml my-values.yaml
```

Edit `my-values.yaml` and update the following **REQUIRED** fields:

```yaml
postiz-app:
  ingress:
    hosts:
      - host: your-domain.com  # Replace with your domain
    tls:
      - hosts:
          - your-domain.com    # Replace with your domain
  
  env:
    FRONTEND_URL: "https://your-domain.com"
    NEXT_PUBLIC_BACKEND_URL: "https://your-domain.com/api"
  
  postgresql:
    auth:
      password: "your-secure-db-password"    # Generate a secure password
    primary:
      persistence:
        storageClass: "your-storage-class"   # e.g., "gp2", "standard"
  
  redis:
    auth:
      password: "your-secure-redis-password" # Generate a secure password
    master:
      persistence:
        storageClass: "your-storage-class"   # e.g., "gp2", "standard"
  
  secrets:
    JWT_SECRET: "your-jwt-secret"            # Generate: openssl rand -base64 32
```

### 3. Deploy

Deploy Postiz to your Kubernetes cluster:

```bash
# Create namespace
kubectl create namespace postiz

# Install the chart
helm upgrade --install postiz . \
  --namespace postiz \
  --values my-values.yaml
```

### 4. Access the Application

Once deployed, access Postiz at your configured domain (e.g., `https://your-domain.com`).

## Configuration

### Database Configuration

The chart includes PostgreSQL by default. For production deployments, consider:

```yaml
postiz-app:
  postgresql:
    enabled: true
    auth:
      password: "secure-password"
    primary:
      persistence:
        enabled: true
        size: 20Gi
        storageClass: "fast-ssd"
```

### External Database

To use an external PostgreSQL database:

```yaml
postiz-app:
  postgresql:
    enabled: false
  secrets:
    DATABASE_URL: "postgresql://user:password@external-host:5432/postiz"
```

### Redis Configuration

Redis is used for caching and sessions:

```yaml
postiz-app:
  redis:
    enabled: true
    auth:
      password: "secure-password"
    master:
      persistence:
        enabled: true
        size: 10Gi
```

### Social Media Integration

Configure API keys for social platforms:

```yaml
postiz-app:
  secrets:
    # Twitter/X
    X_API_KEY: "your-x-api-key"
    X_API_SECRET: "your-x-api-secret"
    
    # LinkedIn
    LINKEDIN_CLIENT_ID: "your-linkedin-client-id"
    LINKEDIN_CLIENT_SECRET: "your-linkedin-client-secret"
    
    # Reddit
    REDDIT_CLIENT_ID: "your-reddit-client-id"
    REDDIT_CLIENT_SECRET: "your-reddit-client-secret"
    
    # GitHub
    GITHUB_CLIENT_ID: "your-github-client-id"
    GITHUB_CLIENT_SECRET: "your-github-client-secret"
```

### High Availability

For production deployments with high availability:

```yaml
postiz-app:
  replicaCount: 3
  
  autoscaling:
    enabled: true
    minReplicas: 2
    maxReplicas: 10
    targetCPUUtilizationPercentage: 70
  
  resources:
    limits:
      cpu: 2000m
      memory: 2Gi
    requests:
      cpu: 1000m
      memory: 1Gi
  
  postgresql:
    primary:
      persistence:
        size: 100Gi
        storageClass: "fast-ssd"
  
  redis:
    master:
      persistence:
        size: 20Gi
        storageClass: "fast-ssd"
```

## Monitoring and Observability

The chart includes basic health checks and resource monitoring. For production environments, consider integrating with:

- **Prometheus** for metrics collection
- **Grafana** for visualization  
- **ELK Stack** for log aggregation

## Backup and Recovery

### Database Backup

Set up regular PostgreSQL backups:

```bash
# Create a backup job
kubectl create job postiz-backup --from=cronjob/postgresql-backup -n postiz
```

### Persistent Volume Backup

Ensure your storage provider supports snapshots and configure regular backups of persistent volumes.

## Troubleshooting

### Common Issues

**Pod stuck in Pending state:**
```bash
kubectl describe pod <pod-name> -n postiz
# Check for resource constraints or storage class issues
```

**Database connection errors:**
```bash
kubectl logs deployment/postiz-postiz-app -n postiz
# Verify DATABASE_URL secret is correctly configured
```

**Ingress not accessible:**
```bash
kubectl get ingress -n postiz
kubectl describe ingress postiz-postiz-app -n postiz
# Check ingress controller and DNS configuration
```

### Useful Commands

```bash
# Check all resources
kubectl get all -n postiz

# View application logs
kubectl logs -f deployment/postiz-postiz-app -n postiz

# Access PostgreSQL
kubectl exec -it sts/postiz-postgresql -n postiz -- psql -U postiz

# Access Redis
kubectl exec -it sts/postiz-redis-master -n postiz -- redis-cli

# Port forward for local testing
kubectl port-forward svc/postiz-postiz-app 8080:80 -n postiz
```

## Uninstallation

To completely remove Postiz:

```bash
# Uninstall the Helm release
helm uninstall postiz -n postiz

# Remove persistent volume claims (optional - this will delete all data)
kubectl delete pvc -l app.kubernetes.io/instance=postiz -n postiz

# Remove namespace
kubectl delete namespace postiz
```

## Security Considerations

- Change default passwords for PostgreSQL and Redis
- Use strong JWT secrets (32+ characters)
- Enable TLS/SSL for all external communications
- Configure network policies to restrict pod-to-pod communication
- Regularly update container images
- Use Kubernetes secrets for sensitive configuration

## Support

For issues with this Helm chart configuration:
1. Check the [Postiz documentation](https://docs.postiz.com)
2. Review the [official Helm chart](https://github.com/gitroomhq/postiz-helmchart)
3. Create an issue in this repository

For Postiz application issues:
- Visit the [Postiz GitHub repository](https://github.com/gitroomhq/postiz-app)
- Join the [Postiz Discord community](https://discord.postiz.com)