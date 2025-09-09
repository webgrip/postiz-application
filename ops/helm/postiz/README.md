# Postiz Helm Chart Deployment

This directory contains the Helm deployment configuration for Postiz using the official Helm chart from the Postiz project.

## Overview

This setup uses the official Postiz Helm chart as a dependency, following the documentation at:
- https://docs.postiz.com/installation/kubernetes-helm
- https://github.com/gitroomhq/postiz-helmchart

## Prerequisites

1. **Kubernetes cluster** with appropriate permissions
2. **Helm 3.x** installed
3. **Storage classes** configured in your cluster
4. **Ingress controller** (e.g., nginx-ingress) if using ingress
5. **Cert-manager** (optional, for automatic TLS certificates)

## Required Configuration

Before installing, you **must** update the following values in `values.yaml`:

### Domain Configuration
```yaml
postiz-app:
  ingress:
    hosts:
      - host: your-domain.com  # Replace with your actual domain
    tls:
      - hosts:
          - your-domain.com   # Replace with your actual domain
  env:
    FRONTEND_URL: "https://your-domain.com"
    NEXT_PUBLIC_BACKEND_URL: "https://your-domain.com/api"
```

### Storage Classes
```yaml
postiz-app:
  postgresql:
    primary:
      persistence:
        storageClass: "your-storage-class"  # e.g., "gp2", "standard", etc.
  redis:
    master:
      persistence:
        storageClass: "your-storage-class"  # e.g., "gp2", "standard", etc.
```

### Security Configuration
```yaml
postiz-app:
  postgresql:
    auth:
      password: "secure-password"  # Generate a secure password
  redis:
    auth:
      password: "secure-redis-password"  # Generate a secure password
  secrets:
    JWT_SECRET: "your-jwt-secret"  # Generate a secure JWT secret
```

## Installation

### 1. Update Dependencies

```bash
cd ops/helm/postiz
helm dependency update
```

### 2. Install the Chart

```bash
# Create namespace
kubectl create namespace postiz

# Install with custom values
helm upgrade --install postiz . \
  --namespace postiz \
  --values values.yaml
```

### 3. Alternative: Install Directly from OCI Registry

You can also install directly from the official OCI registry:

```bash
# Install directly from OCI registry
helm upgrade --install postiz \
  oci://ghcr.io/gitroomhq/postiz-helmchart/postiz-app \
  --version 1.0.5 \
  --namespace postiz \
  --create-namespace \
  --values values.yaml
```

## Configuration

### Environment Variables

The chart supports all standard Postiz environment variables. Key configurations include:

- **Database**: PostgreSQL with configurable credentials and persistence
- **Cache**: Redis with configurable credentials and persistence  
- **Authentication**: JWT-based with configurable secret
- **Social Media APIs**: Optional integration with X, LinkedIn, Reddit, GitHub
- **Storage**: Optional Cloudflare R2 integration
- **Email**: Optional Resend integration

### Secrets Management

Sensitive values are stored in Kubernetes Secrets. For production deployments, consider:

1. **External Secrets Operator** for integration with external secret stores
2. **Sealed Secrets** for encrypted secrets in Git
3. **Manual secret creation** before deployment

Example manual secret creation:
```bash
kubectl create secret generic postiz-secrets \
  --from-literal=JWT_SECRET="your-secure-jwt-secret" \
  --from-literal=DATABASE_PASSWORD="your-db-password" \
  --namespace postiz
```

## Monitoring and Maintenance

### Check Deployment Status
```bash
kubectl get pods -n postiz
kubectl get services -n postiz
kubectl get ingress -n postiz
```

### View Logs
```bash
kubectl logs -f deployment/postiz-postiz-app -n postiz
```

### Update Deployment
```bash
helm upgrade postiz . --namespace postiz --values values.yaml
```

## Uninstallation

```bash
helm uninstall postiz --namespace postiz
kubectl delete namespace postiz
```

## Troubleshooting

### Common Issues

1. **Storage Class Not Found**
   - Verify available storage classes: `kubectl get storageclass`
   - Update values.yaml with correct storage class name

2. **Ingress Not Working**
   - Verify ingress controller is running
   - Check ingress resource: `kubectl describe ingress -n postiz`
   - Verify DNS points to your cluster

3. **Database Connection Issues**
   - Check PostgreSQL pod status: `kubectl get pods -n postiz -l app.kubernetes.io/name=postgresql`
   - Verify database credentials in secrets

4. **Pod Startup Issues**
   - Check pod logs: `kubectl logs -f <pod-name> -n postiz`
   - Verify all required secrets are present
   - Check resource limits and requests

## Support

For issues with:
- **Postiz application**: https://github.com/gitroomhq/postiz-app
- **Official Helm chart**: https://github.com/gitroomhq/postiz-helmchart
- **Documentation**: https://docs.postiz.com