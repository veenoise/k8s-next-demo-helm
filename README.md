# k8s-next-demo-helm

Helm chart for deploying a Next.js demo application on Kubernetes.

## Prerequisites

- Kubernetes 1.19+
- Helm 3+
- [Infisical Operator](https://infisical.com/docs/integrations/platforms/kubernetes) installed in the cluster (for secrets management)
- [Gateway API](https://gateway-api.sigs.k8s.io/) CRDs installed (if using HTTPRoute)

## Installation

```bash
helm install k8s-next-demo . -f dev-values.yaml
```

## Configuration

| Parameter | Description | Default |
|---|---|---|
| `replicaCount` | Number of replicas | `1` |
| `image.repository` | Container image repository | `quay.io/veenoise/k8s-next-demo` |
| `image.tag` | Image tag | `develop` |
| `image.pullPolicy` | Image pull policy | `IfNotPresent` |
| `serviceAccount.create` | Whether to create a ServiceAccount | `true` |
| `service.type` | Kubernetes Service type | `ClusterIP` |
| `service.port` | Service port | `80` |
| `ingress.enabled` | Enable Ingress resource | `false` |
| `httpRoute.enabled` | Enable Gateway API HTTPRoute | `true` |
| `httpRoute.hostnames` | HTTPRoute hostnames | `dev.k8s-next-demo.sabihinmolang.eu.org` |
| `autoscaling.enabled` | Enable HPA | `false` |
| `autoscaling.minReplicas` | Minimum replicas | `1` |
| `autoscaling.maxReplicas` | Maximum replicas | `10` |
| `autoscaling.targetCPUUtilizationPercentage` | CPU target | `80` |

### Example: Using Ingress instead of HTTPRoute

```bash
helm install k8s-next-demo . \
  --set httpRoute.enabled=false \
  --set ingress.enabled=true \
  --set ingress.hosts[0].host=example.com \
  --set ingress.hosts[0].paths[0].path=/
```

## Development

```bash
# Lint the chart
helm lint .

# Render templates locally
helm template . -f dev-values.yaml

# Install with a dry run
helm install --dry-run --debug k8s-next-demo . -f dev-values.yaml
```

## Secrets

Secrets are managed via [Infisical](https://infisical.com/). The chart creates an `InfisicalStaticSecret` custom resource that syncs secrets from your Infisical project into a Kubernetes `Secret` named `k8s-next-demo-helm-secret`.

## License

MIT
