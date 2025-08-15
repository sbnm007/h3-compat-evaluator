# Certificate Management

This directory contains certificates for HTTP/3 testing.

## Directory Structure

- `public-cert/` - Public certificates for general use
- `final-cert/` - Production-ready certificates
- `certs/` - Development certificates

## Security Note

**Important**: This repository's .gitignore excludes certificate files for security. 
Certificate files should be managed separately and never committed to version control.

## Usage

Create Kubernetes secrets from certificates:

```bash
# For Angie server
kubectl create secret tls angie-cert --cert=public-cert/tls.crt --key=public-cert/tls.key

# For QUIC server  
kubectl create secret tls quic-cert --cert=final-cert/new-quic.crt --key=final-cert/new-quic.key
```

## Certificate Requirements

- TLS 1.3 compatible
- SAN (Subject Alternative Name) configured
- QUIC-compatible cipher suites
