# Vertical Pod Autoscaler Metrics

## DEPRECATION NOTICE

From v2.9.0 onwards, `vericalpodautoscalers` will be removed from the list of default resources. This means that specifying that in the `--resource` flag will **not** generate metrics for the same. In order to generate `verticalpodautoscalers` metrics, you will have to explicitly specify it in `--custom-resource-state-config*` (either the inline yaml, or the configuration file), like so:
```yaml
# Using --resource=verticalpodautoscalers, we get the following output:
# HELP kube_verticalpodautoscaler_annotations (Deprecated since v2.9.0) Kubernetes annotations converted to Prometheus labels.
# TYPE kube_verticalpodautoscaler_annotations gauge
# kube_verticalpodautoscaler_annotations{namespace="default",verticalpodautoscaler="hamster-vpa",target_api_version="apps/v1",target_kind="Deployment",target_name="hamster"} 1
# A similar result can be achieved by specifying the following in --custom-resource-state-config:
kind: CustomResourceStateMetrics
spec:
  resources:
    - groupVersionKind:
        group: autoscaling.k8s.io
        kind: "VerticalPodAutoscaler"
        version: "v1"
      labelsFromPath:
        verticalpodautoscaler: [metadata, name]
        namespace: [metadata, namespace]
        target_api_version: [apiVersion]
        target_kind: [spec, targetRef, kind]
        target_name: [spec, targetRef, name]
      metrics:
        - name: "annotations"
          help: "Kubernetes annotations converted to Prometheus labels."
          each:
            type: Gauge
            gauge:
              path: [metadata, annotations]
# This will output the following metric:
# HELP kube_customresource_autoscaling_annotations Kubernetes annotations converted to Prometheus labels.
# TYPE kube_customresource_autoscaling_annotations gauge
# kube_customresource_autoscaling_annotations{customresource_group="autoscaling.k8s.io", customresource_kind="VerticalPodAutoscaler", customresource_version="v1", namespace="default",target_api_version="autoscaling.k8s.io/v1",target_kind="Deployment",target_name="hamster",verticalpodautoscaler="hamster-vpa"} 123
```
PS. The above configuration was tested on [this](https://github.com/kubernetes/autoscaler/blob/master/vertical-pod-autoscaler/examples/hamster.yaml) VPA configuration, with an added annotation (`foo: 123`).
***

| Metric name                                                                               |  Metric type  |  Labels/tags                                                                                                                                                                                                                                                                                                                           |  Status     |
| --------------------------------                                                          |  -----------  |  -------------------------------------------------------------                                                                                                                                                                                                                                                                         |  ------     |
| kube_verticalpodautoscaler_annotations                                                    |  Gauge        |  `annotation_app`=&lt;foo&gt; <br> `namespace`=&lt;namespace&gt; <br> `target_api_version`=&lt;api version&gt; <br> `target_kind`=&lt;target kind&gt; <br> `target_name`=&lt;target name&gt; <br> `verticalpodautoscaler`=&lt;vertical pod autoscaler name&gt;                                                                         |  DEPRECATED |
| kube_verticalpodautoscaler_spec_resourcepolicy_container_policies_minallowed              |  Gauge        |  `container`=&lt;container name&gt; <br> `namespace`=&lt;namespace&gt; <br> `resource`=&lt;cpu memory&gt; <br> `target_api_version`=&lt;api version&gt; <br> `target_kind`=&lt;target kind&gt; <br> `target_name`=&lt;target name&gt; <br> `unit`=&lt;core byte&gt; <br> `verticalpodautoscaler`=&lt;vertical pod autoscaler name&gt;  |  DEPRECATED |
| kube_verticalpodautoscaler_spec_resourcepolicy_container_policies_maxallowed              |  Gauge        |  `container`=&lt;container name&gt; <br> `namespace`=&lt;namespace&gt; <br> `resource`=&lt;cpu memory&gt; <br> `target_api_version`=&lt;api version&gt; <br> `target_kind`=&lt;target kind&gt; <br> `target_name`=&lt;target name&gt; <br> `unit`=&lt;core byte&gt; <br> `verticalpodautoscaler`=&lt;vertical pod autoscaler name&gt;  |  DEPRECATED |
| kube_verticalpodautoscaler_status_recommendation_containerrecommendations_lowerbound      |  Gauge        |  `container`=&lt;container name&gt; <br> `namespace`=&lt;namespace&gt; <br> `resource`=&lt;cpu memory&gt; <br> `target_api_version`=&lt;api version&gt; <br> `target_kind`=&lt;target kind&gt; <br> `target_name`=&lt;target name&gt; <br> `unit`=&lt;core byte&gt; <br> `verticalpodautoscaler`=&lt;vertical pod autoscaler name&gt;  |  DEPRECATED |
| kube_verticalpodautoscaler_status_recommendation_containerrecommendations_target          |  Gauge        |  `container`=&lt;container name&gt; <br> `namespace`=&lt;namespace&gt; <br> `resource`=&lt;cpu memory&gt; <br> `target_api_version`=&lt;api version&gt; <br> `target_kind`=&lt;target kind&gt; <br> `target_name`=&lt;target name&gt; <br> `unit`=&lt;core byte&gt; <br> `verticalpodautoscaler`=&lt;vertical pod autoscaler name&gt;  |  DEPRECATED |
| kube_verticalpodautoscaler_status_recommendation_containerrecommendations_uncappedtarget  |  Gauge        |  `container`=&lt;container name&gt; <br> `namespace`=&lt;namespace&gt; <br> `resource`=&lt;cpu memory&gt; <br> `target_api_version`=&lt;api version&gt; <br> `target_kind`=&lt;target kind&gt; <br> `target_name`=&lt;target name&gt; <br> `unit`=&lt;core byte&gt; <br> `verticalpodautoscaler`=&lt;vertical pod autoscaler name&gt;  |  DEPRECATED |
| kube_verticalpodautoscaler_status_recommendation_containerrecommendations_upperbound      |  Gauge        |  `container`=&lt;container name&gt; <br> `namespace`=&lt;namespace&gt; <br> `resource`=&lt;cpu memory&gt; <br> `target_api_version`=&lt;api version&gt; <br> `target_kind`=&lt;target kind&gt; <br> `target_name`=&lt;target name&gt; <br> `unit`=&lt;core byte&gt; <br> `verticalpodautoscaler`=&lt;vertical pod autoscaler name&gt;  |  DEPRECATED |
| kube_verticalpodautoscaler_labels                                                         |  Gauge        |  `label_app`=&lt;foo&gt; <br> `namespace`=&lt;namespace&gt; <br> `target_api_version`=&lt;api version&gt; <br> `target_kind`=&lt;target kind&gt; <br> `target_name`=&lt;target name&gt; <br> `verticalpodautoscaler`=&lt;vertical pod autoscaler name&gt;                                                                              |  DEPRECATED |
| kube_verticalpodautoscaler_spec_updatepolicy_updatemode                                   |  Gauge        |  `namespace`=&lt;namespace&gt; <br> `target_api_version`=&lt;api version&gt; <br> `target_kind`=&lt;target kind&gt; <br> `target_name`=&lt;target name&gt; <br> `update_mode`=&lt;foo&gt; <br> `verticalpodautoscaler`=&lt;vertical pod autoscaler name&gt;                                                                            |  DEPRECATED |

## Configuration

Vertical Pod Autoscalers(VPAs) are managed as custom resources.

To enable the Vertical Pod Autoscaler collector, please:

1. Ensure that the Vertical Pod Autoscaler CRDs are installed in the cluster. The CRDs are [here](https://github.com/kubernetes/autoscaler/blob/master/vertical-pod-autoscaler/deploy/vpa-beta2-crd.yaml).
2. Ensure that `verticalpodautoscalers` is included in list of `Resources` enabled using the flag `--resources` when `kube-state-metrics` is run (see below).

One of the [command line arguments](./docs/cli-arguments.md) for `kube-state-metrics` is `--resources`. If this flag is omitted, a default set of Resources is enabled. This default list does **not** include Vertical Pod Autoscalers.

To enable Vertical Pod Autoscalers, the `kube-state-metrics` flag `--resource` must be included when the binary is run and the list of resources must include `verticalpodautoscalers`.


### Examples

The following configures `kube-state-metrics` on the command line and in the `args` section of a Kubernetes manifest. Because neither command includes the `--resource` flag, the default set of resources will be include **but** metrics for Vertical Pod Autoscalers will **not** be included:

Shell:

```bash
kube-state-metrics \
--telemetry-port=8081 \
--kubeconfig=... \
--apiserver=...
```

Kubernetes:

```YAML
spec:
  template:
    spec:
      containers:
        - args:
          - --telemetry-port=8081
          - --kubeconfig=...
          - --apiserver=...
```

To include Vertical Pod Autoscaler metrics, you must include the `--resources` flag and to include the default resources, you must include the list of default resources **and** `verticalpodautoscalers`, i.e.:

Shell:

```bash
kube-state-metrics \
--telemetry-port=8081 \
--kubeconfig=... \
--apiserver=... \
--resources=certificatesigningrequests,configmaps,cronjobs,daemonsets,deployments,endpoints,horizontalpodautoscalers, ingresses,jobs,leases,limitranges,mutatingwebhookconfigurations,namespaces,networkpolicies,nodes,persistentvolumeclaims,persistentvolumes,poddisruptionbudgets,pods,replicasets,replicationcontrollers,resourcequotas,secrets,services,statefulsets,storageclasses,validatingwebhookconfigurations,verticalpodautoscalers,volumeattachments
```

Kubernetes:

```YAML
spec:
  template:
    spec:
      containers:
        - args:
          - --telemetry-port=8081
          - --kubeconfig=...
          - --apiserver=...
          - --resources=certificatesigningrequests,configmaps,cronjobs,daemonsets,deployments,endpoints,horizontalpodautoscalers, ingresses,jobs,leases,limitranges,mutatingwebhookconfigurations,namespaces,networkpolicies,nodes,persistentvolumeclaims,persistentvolumes,poddisruptionbudgets,pods,replicasets,replicationcontrollers,resourcequotas,secrets,services,statefulsets,storageclasses,validatingwebhookconfigurations,verticalpodautoscalers,volumeattachments
```

### Confirmation

To confirm that a `kube-state-metrics` process includes `verticalpodautoscalers`, you can:

Shell:

```bash
ps aux \
| grep kube-state-metrics \
| grep verticalpodautoscalers
```

Kubernetes: assuming your deployment is called `kube-state-metrics`:

```bash
DEPLOYMENT="kube-state-metrics"
NAMESPACE="default"

kubectl get deployment/${DEPLOYMENT} \
--namespace=${NAMESPACE} \
--output=jsonpath="{range .spec.template.spec.containers[?(@.name=='kube-state-metrics')].args[*]}{@}{'\n'}{end}"
```

Should include (among other `--flags`):

```console
--resources=certificatesigningrequests,configmaps,cronjobs,daemonsets,deployments,endpoints,horizontalpodautoscalers,ingresses,jobs,leases,limitranges,mutatingwebhookconfigurations,namespaces,networkpolicies,nodes,persistentvolumeclaims,persistentvolumes,poddisruptionbudgets,pods,replicasets,replicationcontrollers,resourcequotas,secrets,services,statefulsets,storageclasses,validatingwebhookconfigurations,verticalpodautoscalers,volumeattachments
```
