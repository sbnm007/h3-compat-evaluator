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
│   ├── angie/              # Angie web server (angie.yaml)
│   ├── quic/               # Custom QUIC server (quic.yaml) 
│   └── python/             # Python server implementation (server.py)
├── load-balancers/
│   ├── metal-lb/           # MetalLB configs (metal-lb.yaml, metal-crd.yaml)
│   ├── haproxy/            # HAProxy configuration (ha-proxy.yaml)
│   └── envoy/              # Envoy proxy config (envoy-proxy-lb.yaml)
├── ingress/
│   ├── emissary/           # Emissary Ingress (emissary.yaml)
│   └── gateway-api.yaml    # Kubernetes Gateway API configuration
├── certificates/
│   ├── public-cert/        # Public certificates (tls.crt, tls.key)
│   ├── final-cert/         # Production certificates (new-quic.crt, new-quic.key)
│   ├── certs/              # Development certificates (quic.crt, quic.key)
│   └── *.crt, *.key        # Additional certificate files and configs
├── tests/
│   ├── clients/            # HTTP/3 test client (k8s-angie-http3-client.py)
│   ├── pcap/               # Network captures (*.pcap files, analysis dumps)
│   ├── pods/               # Test pods (curl-pod.yaml)
│   └── web/                # Web test files (index.html, webtransport-test.html)
├── tools/
│   ├── cleanup/            # Service cleanup scripts (clean-emissary)
│   ├── nghttp3/            # HTTP/3 library source code
│   ├── ngtcp2/             # QUIC library source code
│   └── openssl/            # OpenSSL with QUIC support source
└── setup.sh               # Automated deployment script
```

## Quick Start

### Prerequisites

- Kubernetes cluster (tested with k3s)
- kubectl configured
- Docker for building custom images
- OpenSSL with QUIC support

### Deploy HTTP/3 Servers

```bash
# Quick setup with automated script
./setup.sh

# Or deploy manually:
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
- **Python Server**: Development server for testing (servers/python/)

### Load Balancers
- **MetalLB**: Bare metal load balancer
- **HAProxy**: Traditional load balancer with QUIC support
- **Envoy Proxy**: Modern proxy with HTTP/3 capabilities

### Ingress Controllers
- **Emissary Ingress**: Ambassador-based ingress
- **Gateway API**: Next-generation ingress specification
