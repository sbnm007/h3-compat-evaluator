# QUIC Server

Custom QUIC/HTTP3 server implementation using aioquic library.

## Files

- `quic.yaml` - Kubernetes Pod and Service configuration

## Features

- aioquic-based HTTP/3 server
- Custom Docker image: `sbnm007/quic-demo:1.0.0`
- UDP LoadBalancer service
- TLS certificate integration

## Usage

```bash
# Create certificate secret
kubectl create secret tls quic-cert --cert=../../certificates/final-cert/new-quic.crt --key=../../certificates/final-cert/new-quic.key

# Deploy
kubectl apply -f quic.yaml
```

## Configuration

- **Port**: 4433 (internal), 443 (LoadBalancer)
- **Protocol**: UDP (QUIC)
- **Host**: `::` (IPv6 any)
- **Certificate Mount**: `/certs`
