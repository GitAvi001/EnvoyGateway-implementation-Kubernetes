# Envoy Gateway Implementation in Kubernetes

This is a minimal implementation demonstrating Envoy Gateway as an API gateway in Kubernetes, featuring multiple backend services.

## Contents

- **Gateway Configuration**
  - `demo-gateway-class.yaml`: Defines the GatewayClass for Envoy Gateway
  - `demo-gateway.yaml`: Configures listeners and ports for HTTP routing
  - `demo-http-route.yaml`: Routing rules for Nginx backend
  - `demo-http-route-echoservice.yaml`: Routing rules for echoserver backend

- **Backend Services**
  - Nginx deployment/service (`demo-deployment-nginx.yaml`, `demo-service.yaml`)
  - Echoserver deployments/services (v1/v2 with canary testing examples)

## Quick Start

1. **Install Envoy Gateway**
   
```bash
1. helm install eg oci://docker.io/envoyproxy/gateway-helm -n envoy-gateway-system --create-namespace
2. kubectl wait --timeout=5m -n envoy-gateway-system deployment/envoy-gateway --for=condition=Available
```

2. **Apply configurations**
```bash
1. kubectl apply -f demo-gateway-class.yaml
2. kubectl apply -f demo-gateway.yaml
3. kubectl apply -f demo-http-route.yaml # For Nginx backend
```

3. **Verify deployment**
```bash
1. export GATEWAY_HOST=$(kubectl get gateway/demo-gateway -o jsonpath='{.status.addresses.value}')
2. curl -H "Host: www.example.com" http://$GATEWAY_HOST
```

## Key Features in this implementation

- Multi-backend routing configuration
- Canary deployment examples for echoserver
- Gateway API v1 resources implementation
- Clean separation of control plane and data plane

## Testing Different Backends

For echoserver implementation:
```bash
1. kubectl apply -f demo-deployment-echoserver.yaml
2. kubectl apply -f demo-http-route-echoservice.yaml
```

## Documentation

- [Envoy Gateway Official Docs](https://gateway.envoyproxy.io/docs/)
- [Kubernetes Gateway API Specification](https://gateway-api.sigs.k8s.io)
- [Kubernetes Gateway API Concepts](https://kubernetes.io/docs/concepts/services-networking/gateway/)
- [Gateway API GitHub Repository](https://github.com/kubernetes-sigs/gateway-api)



