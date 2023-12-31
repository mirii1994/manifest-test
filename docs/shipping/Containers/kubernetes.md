---
id: Kubernetes
title: Kubernetes
overview: Kubernetes, also known as K8s, is an open-source system for automating deployments, scaling, and managing containerized applications. Integrate your Kubernetes system with Logz.io to monitor your logs, metrics, and traces, gain observability into your environment, and be able to identify and resolve issues with just a few clicks. 
product: ['logs', 'metrics', 'tracing']
os: ['windows', 'linux']
filters: ['Containers', 'Most Popular']
logo: https://logzbucket.s3.eu-west-1.amazonaws.com/logz-docs/shipper-logos/kubernetes.svg
logs_dashboards: []
logs_alerts: ['1AZRkKc64I12yxAMf2Wyny','6H7dfFOPUaHVMIjxdOMASx','1F6zSL5me5XJt9Lrjw3vxU','2dQHLx0WxmKmLk1kc67Ags','3dyFejyivMaZFdudbwKGRG']
logs2metrics: []
metrics_dashboards: ['7nILXHYFZbThgTSMObUxkw','5TGD77ZKuTiZUXtiM51m6V','6pY6DKD0oQJL4sO7bW728','5kkUAuEwA0Ygvlgm9iXTHY','53g5kSILqoj1T10U1jnvKV','5e1xRaDdQnOvs5LCuwKCh5','7Cy6DUN78jlKUtMCsbt6GC','29HGYsE3kgFEdgJbalTqeY','1Hij49FKdnAKVJTjOmpDbH']
metrics_alerts: ['5Ng398K19vXP9197bRV1If']
drop_filter: []
---





The logzio-monitoring Helm Chart ships your Kubernetes telemetry (logs, metrics, traces and security reports) to your Logz.io account.

## Prerequisites 

1. [Helm](https://helm.sh/)

Add Logzio-helm repository

```sh
helm repo add logzio-helm https://logzio.github.io/logzio-helm && helm repo update
```
{@include: ../../_include/general-shipping/k8s-all-data.md}

## Send your logs

```sh
helm install -n monitoring \
--set logs.enabled=true \
--set logzio-fluentd.secrets.logzioShippingToken="<<LOG-SHIPPING-TOKEN>>" \
--set logzio-fluentd.secrets.logzioListener="<<LISTENER-HOST>>" \
--set logzio-fluentd.env_id="<<CLUSTER-NAME>>" \
logzio-monitoring logzio-helm/logzio-monitoring
```


| Parameter | Description |
| --- | --- |
| `<<LOG-SHIPPING-TOKEN>>` | Your [logs shipping token](https://app.logz.io/#/dashboard/settings/general). |
| `<<LISTENER-HOST>>` | Your account's [listener host](https://app.logz.io/#/dashboard/settings/manage-tokens/data-shipping?product=logs). |
| `<<CLUSTER-NAME>>` | The cluster's name, to easily identify the telemetry data for each environment. |


For log shipping troubleshooting, see our [user guide](https://docs.logz.io/user-guide/kubernetes-troubleshooting/).

## Send your deploy events logs

This integration sends data about deployment events in the cluster, and how they affect the cluster's resources. 
Currently supported resource kinds are `Deployment`, `Daemonset`, `Statefulset`, `ConfigMap`, `Secret`, `Service Account`, `Cluster Role` and `Cluster Role Binding`.

```sh
helm install --namespace=monitoring \
--set secrets.logzioShippingToken='<<LOG-SHIPPING-TOKEN>>' \
--set secrets.logzioListener='<<LISTENER-HOST>>' \
--set secrets.env_id='<<CLUSTER-NAME>>' \
--set secrets.customListener='<<CUSTOM-HOST>>' \
logzio-k8s-events logzio-helm/logzio-k8s-events
```


| Parameter | Description |
| --- | --- |
| `<<LOG-SHIPPING-TOKEN>>` | Your [logs shipping token](https://app.logz.io/#/dashboard/settings/general). |
| `<<LISTENER-HOST>>` | Your account's [listener host](https://app.logz.io/#/dashboard/settings/manage-tokens/data-shipping?product=logs). |
| `<<CLUSTER-NAME>>` | The cluster's name, to easily identify the telemetry data for each environment. |
| `<<CUSTOM-HOST>>` | (*optional*) HTTP/s listener endpoint that receives JSON input, overrides the Logz.io listener. |

### Deployment Events Versioning

In order to add an indication for the versioning in our K8S 360 and Service Overview UI, the following annotation should be added to the metadata of each resource you'd like to track its versioning. 
Commit URL structure: `https://github.com/<account>/<repository>/commit/<commit-hash>`

Example: `https://github.com/logzio/logzio-k8s-events/commit/069c75c95caeca58dd0776405bb8dfb4eed3acb2`

```yaml
metadata:
  annotations:
    logzio/commit_url: ""  
```


For log shipping troubleshooting, see our [user guide](https://docs.logz.io/user-guide/kubernetes-troubleshooting/).

## Send your Metrics

```sh
helm install -n monitoring \
--set metricsOrTraces.enabled=true \
--set logzio-k8s-telemetry.metrics.enabled=true \
--set logzio-k8s-telemetry.secrets.MetricsToken="<<PROMETHEUS-METRICS-SHIPPING-TOKEN>>" \
--set logzio-k8s-telemetry.secrets.ListenerHost="https://<<LISTENER-HOST>>:8053" \
--set logzio-k8s-telemetry.secrets.p8s_logzio_name="<<CLUSTER-NAME>>" \
--set logzio-k8s-telemetry.secrets.env_id="<<CLUSTER-NAME>>" \
logzio-monitoring logzio-helm/logzio-monitoring
```

| Parameter | Description |
| --- | --- |
| `<<PROMETHEUS-METRICS-SHIPPING-TOKEN>>` | Your [metrics shipping token](https://app.logz.io/#/dashboard/settings/manage-tokens/data-shipping). |
| `<<CLUSTER-NAME>>` | The cluster's name, to easily identify the telemetry data for each environment. |
| `<<LISTENER-HOST>>` | Your account's [listener host](https://app.logz.io/#/dashboard/settings/manage-tokens/data-shipping?product=logs). |


For metrics shipping troubleshooting, see our [user guide](https://docs.logz.io/user-guide/infrastructure-monitoring/troubleshooting/k8-helm-opentelemetry-troubleshooting.html).



## Send your traces

```sh
helm install -n monitoring \
--set metricsOrTraces.enabled=true \
--set logzio-k8s-telemetry.traces.enabled=true \
--set logzio-k8s-telemetry.secrets.TracesToken="<<TRACING-SHIPPING-TOKEN>>" \
--set logzio-k8s-telemetry.secrets.LogzioRegion="<<LOGZIO_ACCOUNT_REGION_CODE>>" \
--set logzio-k8s-telemetry.secrets.env_id="<<CLUSTER-NAME>>" \
logzio-monitoring logzio-helm/logzio-monitoring
```

| Parameter | Description |
| --- | --- |
| `<<TRACES-SHIPPING-TOKEN>>` | Your [traces shipping token](https://app.logz.io/#/dashboard/settings/manage-tokens/data-shipping?product=tracing). |
| `<<CLUSTER-NAME>>` | The cluster's name, to easily identify the telemetry data for each environment. |
| `<<LOGZIO_ACCOUNT_REGION_CODE>>` | Name of your Logz.io traces region e.g `us`, `eu`... |


For traces shipping troubleshooting, see our [user guide]([https://docs.logz.io/user-guide/kubernetes-troubleshooting/](https://docs.logz.io/user-guide/distributed-tracing/tracing-troubleshooting.html)).


## Send traces with SPM

```sh
helm install -n monitoring \
--set metricsOrTraces.enabled=true \
--set logzio-k8s-telemetry.traces.enabled=true \
--set logzio-k8s-telemetry.secrets.TracesToken="<<TRACING-SHIPPING-TOKEN>>" \
--set logzio-k8s-telemetry.secrets.LogzioRegion="<<LOGZIO_ACCOUNT_REGION_CODE>>" \
--set logzio-k8s-telemetry.secrets.env_id="<<CLUSTER-NAME>>" \
--set logzio-k8s-telemetry.spm.enabled=true \
--set logzio-k8s-telemetry.secrets.SpmToken=<<SPM-METRICS-SHIPPING-TOKEN>> \
logzio-monitoring logzio-helm/logzio-monitoring
```

| Parameter | Description |
| --- | --- |
| `<<TRACES-SHIPPING-TOKEN>>` | Your [traces shipping token](https://app.logz.io/#/dashboard/settings/manage-tokens/data-shipping). |
| `<<CLUSTER-NAME>>` | The cluster's name, to easily identify the telemetry data for each environment. |
| `<<LISTENER-HOST>>` | Your account's [listener host](https://app.logz.io/#/dashboard/settings/manage-tokens/data-shipping?product=logs). |
| `<<LOGZIO_ACCOUNT_REGION_CODE>>` | Name of your Logz.io traces region e.g `us`, `eu`... |
| `<<SPM-METRICS-SHIPPING-TOKEN>>` | Your [span metrics shipping token](https://app.logz.io/#/dashboard/settings/manage-tokens/data-shipping). |


## Scan your cluster for security vulnerabilities

```sh
helm install -n monitoring \
--set securityReport.enabled=true \
--set logzio-trivy.env_id="<<CLUSTER-NAME>>" \
--set logzio-trivy.secrets.logzioShippingToken="<<LOG-SHIPPING-TOKEN>>" \
--set logzio-trivy.secrets.logzioListener="<<LISTENER-HOST>>" \
```

| Parameter | Description |
| --- | --- |
| `<<LOG-SHIPPING-TOKEN>>` | Your [logs shipping token](https://app.logz.io/#/dashboard/settings/general). |
| `<<LISTENER-HOST>>` | Your account's [listener host](https://app.logz.io/#/dashboard/settings/manage-tokens/data-shipping?product=logs). |
| `<<CLUSTER-NAME>>` | The cluster's name, to easily identify the telemetry data for each environment. |


## Modifying the configuration for logs

You can see a full list of the possible configuration values in the [logzio-fluentd Chart folder](https://github.com/logzio/logzio-helm/tree/master/charts/fluentd#configuration).

If you would like to modify any of the values found in the `logzio-fluentd` folder, use the `--set` flag with the `logzio-fluentd` prefix.

For instance, if there is a parameter called `someField` in the `logzio-telemetry`'s `values.yaml` file, you can set it by adding the following to the `helm install` command:

```sh
--set logzio-fluentd.someField="my new value"
```
You can add `log_type` annotation with a custom value, which will be parsed into a `log_type` field with the same value.


### Modifying the configuration for metrics and traces

You can see a full list of the possible configuration values in the [logzio-telemetry Chart folder](https://github.com/logzio/logzio-helm/tree/master/charts/logzio-telemetry).

If you would like to modify any of the values found in the `logzio-telemetry` folder, use the `--set` flag with the `logzio-k8s-telemetry` prefix.

For instance, if there is a parameter called `someField` in the `logzio-telemetry`'s `values.yaml` file, you can set it by adding the following to the `helm install` command:


```sh
--set logzio-k8s-telemetry.someField="my new value"
```

## Sending telemetry data from eks on fargate

To ship logs from pods running on Fargate, set the `fargateLogRouter.enabled` value to `true`. Doing so will deploy a dedicated `aws-observability` namespace and a `configmap` for the Fargate log router. For more information on EKS Fargate logging, please refer to the [official AWS documentation]((https://docs.aws.amazon.com/eks/latest/userguide/fargate-logging.html).

```shell
helm install -n monitoring \
--set logs.enabled=true \
--set logzio-fluentd.fargateLogRouter.enabled=true \
--set logzio-fluentd.secrets.logzioShippingToken="<<LOG-SHIPPING-TOKEN>>" \
--set logzio-fluentd.secrets.logzioListener="<<LISTENER-HOST>>" \
--set metricsOrTraces.enabled=true \
--set logzio-k8s-telemetry.metrics.enabled=true \
--set logzio-k8s-telemetry.secrets.MetricsToken="<<PROMETHEUS-METRICS-SHIPPING-TOKEN>>" \
--set logzio-k8s-telemetry.secrets.ListenerHost="https://<<LISTENER-HOST>>:8053" \
--set logzio-k8s-telemetry.secrets.p8s_logzio_name="<<CLUSTER-NAME>>" \
--set logzio-k8s-telemetry.traces.enabled=true \
--set logzio-k8s-telemetry.secrets.TracesToken="<<TRACES-SHIPPING-TOKEN>>" \
--set logzio-k8s-telemetry.secrets.LogzioRegion="<<LOGZIO_ACCOUNT_REGION_CODE>>" \
logzio-monitoring logzio-helm/logzio-monitoring
```

| Parameter | Description |
| --- | --- |
| `<<LOG-SHIPPING-TOKEN>>` | Your [logs shipping token](https://app.logz.io/#/dashboard/settings/manage-tokens/data-shipping?product=logs). |
| `<<LISTENER-HOST>>` | Your account's [listener host](https://app.logz.io/#/dashboard/settings/manage-tokens/data-shipping?product=logs). |
| `<<PROMETHEUS-METRICS-SHIPPING-TOKEN>>` | Your [metrics shipping token](https://app.logz.io/#/dashboard/settings/manage-tokens/data-shipping?product=metrics). |
| `<<P8S-LOGZIO-NAME>>` | The name for the environment's metrics, to easily identify the metrics for each environment. |
| `<<CLUSTER-NAME>>` | The name for your environment's identifier, to easily identify the telemetry data for each environment. |
| `<<ENV-TAG>>` | Your custom name for the environment's metrics, to easily identify the metrics for each environment. |
| `<<TRACES-SHIPPING-TOKEN>>` | Replace `<<TRACING-SHIPPING-TOKEN>>` with the [token](https://app.logz.io/#/dashboard/settings/manage-tokens/data-shipping?product=tracing) of the account you want to ship to. |
| `<<LOGZIO_ACCOUNT_REGION_CODE>>` | Name of your Logz.io traces region e.g `us` or `eu`. You can find your region code in the [Regions and URLs](https://docs.logz.io/user-guide/accounts/account-region.html#regions-and-urls) table. |

## Handling image pull rate limit

In certain situations, such as with spot clusters where pods/nodes are frequently replaced, you may encounter the pull rate limit for images fetched from Docker Hub. This could result in the following error: `You have reached your pull rate limit. You may increase the limit by authenticating and upgrading: https://www.docker.com/increase-rate-limits`.

To address this issue, you can use the `--set` commands provided below in order to access an alternative image repository:

```shell
--set logzio-k8s-telemetry.image.repository=ghcr.io/open-telemetry/opentelemetry-collector-releases/opentelemetry-collector-contrib
--set logzio-k8s-telemetry.prometheus-pushgateway.image.repository=public.ecr.aws/logzio/prom-pushgateway
--set logzio-fluentd.image=public.ecr.aws/logzio/logzio-fluentd
--set logzio-fluentd.daemonset.init.containerImage=public.ecr.aws/docker/library/busybox
--set logzio-trivy.image=public.ecr.aws/logzio/trivy-to-logzio
```
