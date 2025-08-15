# H3 Compatibility Evaluator

A comprehensive evaluation framework for testing HTTP/3 (QUIC) compatibility across different server implementations and load balancers in Kubernetes environments.

## Overview

This repository contains configurations and tools for evaluating HTTP/3 compatibility with various components:

- **Angie Web Server** - HTTP/3 capable web server
- **QUIC Server** - Custom QUIC/HTTP3 implementation using aioquic
- **MetalLB** - Load balancer for bare metal Kubernetes clusters
- **Various Proxy Solutions** - HAProxy, Envoy, Emissary Ingress

## Repository Structure

```
├── servers/
│   ├── angie/              # Angie web server configurations
│   └── quic/               # Custom QUIC server setup
├── load-balancers/
│   ├── metal-lb/           # MetalLB configurations
│   ├── haproxy/            # HAProxy configurations
│   └── envoy/              # Envoy proxy configurations
├── ingress/
│   ├── emissary/           # Emissary Ingress configurations
│   └── gateway-api/        # Kubernetes Gateway API configs
├── certificates/
│   ├── public-cert/        # Public certificates
│   ├── final-cert/         # Final certificates for production
│   └── certs/              # Development certificates
├── tests/
│   ├── clients/            # HTTP/3 client implementations
│   ├── scripts/            # Test automation scripts
│   └── pcap/               # Network capture files
├── tools/
│   ├── nghttp3/            # nghttp3 library
│   ├── ngtcp2/             # ngtcp2 library
│   └── openssl/            # OpenSSL with QUIC support
└── docs/                   # Documentation and evaluation results
```

## Quick Start

### Prerequisites

- Kubernetes cluster (tested with k3s)
- kubectl configured
- Docker for building custom images
- OpenSSL with QUIC support

### Deploy HTTP/3 Servers

```bash
# Deploy Angie HTTP/3 server
kubectl apply -f servers/angie/

# Deploy QUIC server
kubectl apply -f servers/quic/

# Deploy MetalLB
kubectl apply -f load-balancers/metal-lb/
```

### Create Certificates

```bash
# Create secrets for Angie
kubectl create secret tls angie-cert --cert=certificates/public-cert/tls.crt --key=certificates/public-cert/tls.key

# Create secrets for QUIC server
kubectl create secret tls quic-cert --cert=certificates/final-cert/new-quic.crt --key=certificates/final-cert/new-quic.key
```

## Testing HTTP/3 Compatibility

### Client Testing

```bash
# Test with curl (HTTP/3)
curl --http3 https://your-domain:443/

# Test with custom Python client
python tests/clients/k8s-angie-http3-client.py
```

### Performance Evaluation

- Network latency analysis
- Throughput comparison (HTTP/2 vs HTTP/3)
- Connection establishment time
- Multiplexing efficiency

## Components Evaluated

### Web Servers
- **Angie**: Enterprise-grade web server with HTTP/3 support
- **Custom QUIC Server**: aioquic-based implementation

### Load Balancers
- **MetalLB**: Bare metal load balancer
- **HAProxy**: Traditional load balancer with QUIC support
- **Envoy Proxy**: Modern proxy with HTTP/3 capabilities

### Ingress Controllers
- **Emissary Ingress**: Ambassador-based ingress
- **Gateway API**: Next-generation ingress specification

## Results and Findings

[Documentation of compatibility testing results, performance benchmarks, and recommendations]

## Contributing

1. Fork the repository
2. Create a feature branch
3. Add your HTTP/3 implementation or test case
4. Submit a pull request

## License

MIT License - see LICENSE file for details
