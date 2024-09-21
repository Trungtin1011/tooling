# swagger-ui-helm

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

## Introduction

This [Helm](https://github.com/kubernetes/helm) chart installs [swagger-ui](https://github.com/swagger-ui-api/swagger-ui) in a Kubernetes cluster.

## Prerequisites

- Kubernetes cluster
- Helm 3.0.0+
- StorageClass or PersistentVolume

## Installation

### Add Helm repository

```bash
helm repo add
helm repo update
```

### Configure the chart

The following items can be set via `--set` flag during installation or configured by editing the `values.yaml` directly (need to download the chart first).

#### Configure the way how to expose swagger-ui service:

- **Ingress**: The ingress controller must be installed in the Kubernetes cluster.
- **ClusterIP**: Exposes the service on a cluster-internal IP. Choosing this value makes the service only reachable from within the cluster.
- **NodePort**: Exposes the service on each Node’s IP at a static port (the NodePort). You’ll be able to contact the NodePort service, from outside the cluster, by requesting `NodeIP:NodePort`.
- **LoadBalancer**: Exposes the service externally using a cloud provider’s load balancer.

### Install the chart

Install the swagger-ui helm chart with a release name `my-release`:

```bash
helm install my-release
```

## Uninstallation

To uninstall/delete the `my-release` deployment:

```bash
helm delete my-release
```

## Important Configurations

The following table lists the most improtant configurable parameters of the swagger-ui-helm chart and the default values.

| Parameter                                           | Description                                                             | Default                                           |
| --------------------------------------------------- | ----------------------------------------------------------------------- | ------------------------------------------------- |
| **Swagger UI deployment**                           |                                                                         |                                                   |
| `swaggerui.replicaCount`                            | Number of replicas                                                      | `1`                                               |
| `swaggerui.imagePullSecrets`                        | List of names of secrets containing docker registry credentials         | `[]`                                              |
| `swaggerui.image.repository`                        | swagger-ui Image name                                                   | `swaggerapi/swagger-ui`                           |
| `swaggerui.image.tag`                               | swagger-ui Image tag                                                    | `v5.11.2`                                         |
| `swaggerui.image.pullPolicy`                        | swagger-ui Image pull policy                                            | `IfNotPresent`                                    |
| `swaggerui.apikey`                                  | API Key value                                                           | `""`                                              |
| `swaggerui.base_url`                                | The base URL of the web application                                     | `/`                                               |
| `swaggerui.enable_cors`                             | Enable CORS support                                                     | `true`                                            |
| `swaggerui.allow_embedding`                         | Allow/disallow embedding                                                | `false`                                           |
| `swaggerui.disable_validation`                      | Disable Swagger validator                                               | `false`                                           |
| `swaggerui.extraEnv`                                | Additional environment variable                                         | `[]`                                              |
| `swaggerui.resources`                               | Swagger UI container's CPU/Memory resource requests/limits              | `{}`                                              |
| `swaggerui.swagger_config_local`                    | Existing secret that contains swagger.json file                         | `""`                                              |
| `swaggerui.swagger_config_public`                   | URL to a swagger.json file on an external host                          | `https://petstore.swagger.io/v2/swagger.json`     |
| `swaggerui.swagger_config_s3.enabled`               | Configs for swagger.json file that stored in private AWS S3 bucket      | `false`                                           |
| `swaggerui.swagger_config_s3.s3_bucket_uri`         | Bucket URI of S3 bucket                                                 | `s3://bucket_name`                                |
| `swaggerui.swagger_config_s3.s3_config_object`      | S3 object for Swagger config                                            | `swagger.json`                                    |
| `swaggerui.swagger_config_s3.irsa`                  | IAM Role For Service Account - For EKS-based cluster                    | `arn:aws:iam::account_id:role/irsa_name`          |
| `swaggerui.swagger_config_s3.aws_region`            | AWS Region for configuring AWS connection                               | `ap-southeast-1`                                  |
| `swaggerui.swagger_config_s3.image.repository`      | Image to run initContainer                                              | `amazon/aws-cli`                                  |
| `swaggerui.swagger_config_s3.image.pullPolicy`      | Image pull policy                                                       | `IfNotPresent`                                    |
| `swaggerui.swagger_config_s3.image.tag`             | Image tag for initContainer                                             | `2.15.15`                                         |
| **Swagger UI Validator**                            |                                                                         |                                                   |
| `localValidation.enabled`                           | Whether to enable local validator for Swagger resources                 | `false`                                           |
| `localValidation.imagePullSecrets`                  | List of names of secrets containing docker registry credentials         | `[]`                                              |
| `localValidation.image.repository`                  | Swagger validator image repository                                      | `swaggerapi/swagger-validator-v2`                 |
| `localValidation.image.pullPolicy`                  | Swagger validator image pull policy                                     | `IfNotPresent`                                    |
| `localValidation.image.tag`                         | Swagger validator image tag                                             | `v2.1.4`                                          |
| `localValidation.resources`                         | Swagger validator container's CPU/Memory resource requests/limits       | `{}`                                              |
| `localValidation.extraEnv`                          | Swagger validator extra environment variables                           | `[]`                                              |
| `localValidation.containerSecurityContext`          | Swagger validator Container security contexts                           | `{}`                                              |
| **Service**                                         |                                                                         |                                                   |
| `swaggerui.service.type`                            | Type of service for swagger-ui frontend                                 | `ClusterIP`                                       |
| `swaggerui.service.port`                            | Port to expose service                                                  | `8080`                                            |
| `swaggerui.service.annotations`                     | Service annotations                                                     | `{}`                                              |
| `swaggerui.service.externalIPs`                     | List of IPs for externally access.                                      | `[]`                                              |
| `swaggerui.service.loadBalancerIP`                  | LoadBalancerIP if service type is `LoadBalancer`                        | `""`                                              |
| `swaggerui.service.loadBalancerSourceRanges`        | Address that are allowed when svc is `LoadBalancer`                     | `[]`                                              |
| **Ingress**                                         |                                                                         |                                                   |
| `swaggerui.ingress.enabled`                         | Enables Ingress                                                         | `false`                                           |
| `swaggerui.ingress.annotations`                     | Ingress annotations                                                     | `{}`                                              |
| `swaggerui.ingress.path`                            | Path to access frontend                                                 | `/`                                               |
| `swaggerui.ingress.host`                            | Ingress host                                                            | `chart-example.local`                             |
| `swaggerui.ingress.tls`                             | Ingress TLS configuration                                               | `[]`                                              |
| `swaggerui.ingress.className`                       | Ingress ClassName                                                       | `""`                                              |
| `swaggerui.ingress.pathType`                        | Ingress Path type                                                       | `ImplementationSpecific`                          |
| **LivenessProbe and ReadinessProbe**                |                                                                         |                                                   |
| `swaggerui.livenessProbe.enabled`                   | Whether to enable Liveness Probe settings                               | `true`                                            |
| `swaggerui.livenessProbe.httpGet.path`              | Liveness Probe HTTP path                                                | `/`                                               |
| `swaggerui.livenessProbe.httpGet.port`              | Liveness Probe HTTP port                                                | `http`                                            |
| `swaggerui.livenessProbe.initialDelaySeconds`       | Liveness Probe initialDelaySeconds                                      | `30`                                              |
| `swaggerui.livenessProbe.periodSeconds`             | Liveness Probe periodSeconds                                            | `30`                                              |
| `swaggerui.livenessProbe.timeoutSeconds`            | Liveness Probe timeoutSeconds                                           | `10`                                              |
| `swaggerui.livenessProbe.failureThreshold`          | Liveness Probe failureThreshold                                         | `3`                                               |
| `swaggerui.livenessProbe.successThreshold`          | Liveness Probe successThreshold                                         | `1`                                               |
| `swaggerui.readinessProbe.enabled`                  | Whether to enable Rediness Probe settings                               | `true`                                            |
| `swaggerui.readinessProbe.httpGet.path`             | Rediness Probe HTTP path                                                | `/`                                               |
| `swaggerui.readinessProbe.httpGet.port`             | Rediness Probe HTTP port                                                | `http`                                            |
| `swaggerui.readinessProbe.initialDelaySeconds`      | Rediness Probe initialDelaySeconds                                      | `30`                                              |
| `swaggerui.readinessProbe.periodSeconds`            | Rediness Probe periodSeconds                                            | `30`                                              |
| `swaggerui.readinessProbe.timeoutSeconds`           | Rediness Probe timeoutSeconds                                           | `10`                                              |
| `swaggerui.readinessProbe.failureThreshold`         | Rediness Probe failureThreshold                                         | `3`                                               |
| `swaggerui.readinessProbe.successThreshold`         | Rediness Probe successThreshold                                         | `1`                                               |
| `swaggerui.podAnnotations`                          | Extra annotations for Swagger UI pods                                   | `{}`                                              |
| `swaggerui.podLabels`                               | Extra labels for Swagger UI pods                                        | `{}`                                              |
| `swaggerui.podSecurityContext`                      | Swagger UI Pods Security Context                                        | `{}                                               |
| `swaggerui.containerSecurityContext`                | Swagger UI Container Security Context                                   | `{}`                                              |
| `swaggerui.nodeSelector`                            | Node labels selector for Swagger UI pods assignment                     | `{}`                                              |
| `swaggerui.affinity`                                | Affinity for Swagger UI pods assignment                                 | `{}`                                              |
| `swaggerui.tolerations`                             | Tolerations for Swagger UI pods assignment                              | `[]`                                              |


## License

[Apache License 2.0](/LICENSE)
