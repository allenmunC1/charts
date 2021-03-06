<p align="center"><img src="https://raw.githubusercontent.com/kedacore/keda/master/images/keda-logo-transparent.png" width="300"/></p>
<p style="font-size: 25px" align="center"><b>Kubernetes-based Event Driven Autoscaling</b></p>

KEDA allows for fine grained autoscaling (including to/from zero) for event driven Kubernetes workloads.  KEDA serves as a Kubernetes Metrics Server and allows users to define autoscaling rules using a dedicated Kubernetes custom resource definition.

KEDA can run on both the cloud and the edge, integrates natively with Kubernetes components such as the Horizontal Pod Autoscaler, and has no external dependencies.

---
<p align="center">
We are a Cloud Native Computing Foundation (CNCF) sandbox project.

<img src="https://raw.githubusercontent.com/kedacore/keda/master/images/logo-cncf.svg" height="75px">
</p>

---

## TL;DR

```console
helm repo add kedacore https://kedacore.github.io/charts
helm repo update

kubectl create namespace keda
helm install keda kedacore/keda --namespace keda
```

## Introduction

This chart bootstraps KEDA infrastructure on a Kubernetes cluster using the Helm package manager.

As part of that, it will install all the required Custom Resource Definitions (CRD).

## Installing the Chart

To install the chart with the release name `keda`:

```console
$ kubectl create namespace keda
$ helm install keda kedacore/keda --namespace keda
```

## Uninstalling the Chart

To uninstall/delete the `keda` Helm chart:

```console
helm uninstall keda
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following table lists the configurable parameters of the Promitor chart and
their default values.

| Parameter                              | Description                               | Default                                         |
|:---------------------------------------|:------------------------------------------|:------------------------------------------------|
| `image.keda.repository`                | Image name of KEDA operator               | `docker.io/kedacore/keda`                 |
| `image.keda.tag`                       | Image tag of KEDA operator. Optional, given app version of Helm chart is used by default | ``                 |
| `image.metricsApiServer.repository`    | Image name of KEDA Metrics API Server        | `docker.io/kedacore/keda-metrics-apiserver` |
| `image.metricsApiServer.tag`           | Image tag of KEDA Metrics API Server. Optional, given app version of Helm chart is used by default | ``                 |
| `watchNamespace`                       | Policy to use to pull Docker images       | `` |
| `operator.name`                        | Name of the KEDA operator | `keda-operator` |
| `imagePullSecrets`                     | Name of secret to use to pull images to use to pull Docker images | `[]` |
| `additionalLabels`                     | Additional labels to apply to KEDA workloads | `` |
| `podAnnotations.keda`                  | Pod annotations for KEDA operator | `` |
| `podAnnotations.metricsAdapter`        | Pod annotations for KEDA Metrics Adapter | `` |
| `podDisruptionBudget`                  | Capability to configure [Pod Disruption Budget](https://kubernetes.io/docs/tasks/run-application/configure-pdb/)       | `{}` |
| `rbac.create`                          | Specifies whether RBAC should be used | `true`                                        |
| `serviceAccount.create`                | Specifies whether a service account should be created       | `true`                                        |
| `serviceAccount.name`                  | The name of the service account to use. If not set and create is true, a name is generated.      | `keda-operator` |
| `serviceAccount.annotations`           | Annotations to add to the service account | `{}` |
| `podIdentity.activeDirectory.identity` | Identity in Azure Active Directory to use for Azure pod identity | `` |
| `grpcTLSCertsSecret`                   | Name of the secret that will be mounted to the /grpccerts path on the Pod to communicate over TLS with external scaler(s) (recommended).  | ``|
| `hashiCorpVaultTLS`                    | Name of the secret that will be mounted to the /vault path on the Pod to communicate over TLS with HashiCorp Vault (recommended). | `` |
| `logging.operator.level`               | Logging level for KEDA Operator. Allowed values are 'debug', 'info' & 'error'. | `info`                                        |
| `logging.operator.timeFormat`          | Logging time format for KEDA Operator. Allowed values are 'epoch', 'millis', 'nano', or 'iso8601'. | `epoch` |
| `logging.metricServer.level`           | Logging level for Metrics Server.Policy to use to pull Docker images. Allowed values are '0' for info, '4' for debug, or an integer value greater than 0, specified as string | `0` |
| `securityContext`                      | Security context of the pod ([docs](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)) | `{}` |
| `podSecurityContext`                   | Pod security context of the pod ([docs](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)) | `{}` |
| `resources`                            | Manage resource request & limits of KEDA workload ([docs](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/)) | `{}` |
| `nodeSelector`                         | Node selector for pod scheduling ([docs](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/)) | `{}` |
| `tolerations`                          | Tolerations for pod scheduling ([docs](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/)) | `{}` |
| `affinity`                             | Affinity for pod scheduling ([docs](https://kubernetes.io/docs/tasks/configure-pod-container/assign-pods-nodes-using-node-affinity/)) | `{}` |
| `priorityClassName`                    | Pod priority for KEDA Operator and Metrics Adapter ([docs](https://kubernetes.io/docs/concepts/configuration/pod-priority-preemption/)) | `` |
| `env`                                  | Additional environment variables that will be passed onto KEDA operator and metrics api service | `` |
| `service.portHttp`                     | Service HTTP port for KEDA Metric Server service | `80`                                        |
| `service.portHttpTarget`               | Service HTTP port for KEDA Metric Server container | `8080`                                        |
| `service.portHttps`                    | HTTPS port for KEDA Metric Server service | `443`                                        |
| `service.portHttpsTarget`              | HTTPS port for KEDA Metric Server container | `6443`                                        |

Specify each parameter using the `--set key=value[,key=value]` argument to
`helm install`. For example:

```console
$ helm install keda kedacore/keda --namespace keda \
               --set image.keda.tag=<different tag from app version>
               --set image.metricsApiServer.tag=<different tag from app version>
```

Alternatively, a YAML file that specifies the values for the above parameters can
be provided while installing the chart. For example,

```console
helm install keda kedacore/keda --namespace keda -f values.yaml
```