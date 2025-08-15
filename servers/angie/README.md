# Angie HTTP/3 Server

Angie web server configuration for HTTP/3 compatibility testing.

## Files

- `angie.yaml` - Complete Kubernetes deployment with ConfigMap, Deployment, and Service

## Features

- HTTP/3 (QUIC) support
- TLS 1.3 configuration
- HTTP/2 fallback
- Multiple test endpoints
- Sidecar containers for testing tools

## Usage

```bash
# Create certificate secret
kubectl create secret tls angie-cert --cert=../../certificates/public-cert/tls.crt --key=../../certificates/public-cert/tls.key

# Deploy
kubectl apply -f angie.yaml
```

## Testing Endpoints

- `/` - Basic HTTP/3 response
- `/service` - Proxy to QUIC service
- `/pod` - HTTP/3 upstream proxy
- `/test-h3` - Protocol detection endpoint
