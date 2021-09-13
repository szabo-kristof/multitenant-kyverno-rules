# Multitenant Kyverno policies

This Helm chart deploys Kyverno policies defined for Multitenant Clusters. 


## Policies

| Name                                             | defined in                                | g/v/m    | a/e     | applied on    | description  |
| -------------                                    | ----------                                | -------- | -----   | ------------  | -------------|
| admin-require-annotations-or-io-type   | [annotations-adm…](/multitenant-kyverno-rules/templates/annotations-admin.yaml)| validate | enforce | Namespace     | Basic housekeeping to check for annotations (admins can override) |   
| admin-require-labels-or-io-type        | [labels-adm…](/multitenant-kyverno-rules/templates/labels-admin.yaml)| validate | enforce | Namespace     | Basic housekeeping to check for labels (admins can override) |   
| certificate-expiry-max-100days              | [certificate-exp…](/multitenant-kyverno-rules/templates/certificate-expiry-max-100days.yaml) | validate | enforce | Namespace     | k8s managed non letsencrypt certificates have to be renewed in every 100day | 
| block-updates-deletes-for-protected-network | [protect.yaml](/multitenant-kyverno-rules/templates/protect.yaml)| validate | enforce | NetworkPolicy | NetworkPolicies with `protected: "true"` label cannot be deleted |
| disallow-loadbalancer-services              | [disallow-inf…](/multitenant-kyverno-rules/templates/disallow-infa-services.yaml) | validate | enforce | Service       | Only allowed ClusterRoles can create LoadBalancer type services |
| disallow-nodeport-services                  | [disallow-inf…](/multitenant-kyverno-rules/templates/disallow-infa-services.yaml) | validate | enforce | Service       | NodePort type services cannot be created on the cluster | 
| disallow-privileged-containers              | [disallow-pri…](/multitenant-kyverno-rules/templates/disallow-privileged-containers.yaml) | validate | audit   | Pod           | The fields spec.containers[*].securityContext.privileged  and spec.initContainers[*].securityContext.privileged must not be set to true |
| generate-ns-quota                           | [generate-def…](/multitenant-kyverno-rules/templates/generate-default-reuests-limits.yaml) | generate |         | NameSpace     | All new Namespaces get resource quotas defined | 
| generate-default-access                     | [generate-def…](/multitenant-kyverno-rules/templates/generate-default-access.yaml) | generate |         | NameSpace     | Default RoleBindings can be created for namespaces | 
| generate-net-allow-instana-access           | [generate-def…](/multitenant-kyverno-rules/templates/generate-default-network-policies.yaml) | generate |         | NameSpace     | All new Namespaces get a protected NetworkPolicy that enables Instana communication | 
| generate-net-default-deny-egress            | [network.yaml](/multitenant-kyverno-rules/templates/generate-default-network-policies.yamll) | generate |         | NameSpace     | All new Namespaces get a protected NetworkPolicy that disables default egress | 
| generate-net-default-deny-ingress           | [network.yaml](/multitenant-kyverno-rules/templates/generate-default-network-policies.yaml) | generate |         | NameSpace     | All new Namespaces get a protected NetworkPolicy that disables default ingress | 
| require-requests-limits                     | [require-memor…](/multitenant-kyverno-rules/templates/require-memory-requests-limits.yaml) | validate | enforce | Pod           | This policy validates that all containers have something specified for memory and CPU requests and memory limits |
| memory-requests-equal-limits                | [require-memor…](/multitenant-kyverno-rules/templates/require-memory-requests-equal-limits.yaml) | validate | audit   | Pod           | This policy checks that all containers in a given Pod have memory requests equal to limits | 
| require-annotations                         | [annotations.yaml](/multitenant-kyverno-rules/templates/labels.yaml) | validate | enforce | Namespace     | Basic housekeeping checks (annotations) | 
| require-labels                              | [labels.yaml](/multitenant-kyverno-rules/templates/labels.yaml) | validate | enforce | Namespace     | Basic housekeeping checks (labels) | 


Note: the labels and annotations to be validated are configurable through values.yaml.

# Values

#TODO

# Appendix

Query Multitenant ClusterPolicies on a cluster

```
k get ClusterPolicy -l myns.io/origin=myns
```

## testing

```
kubectl krew install kyverno 
cd test
k kyverno apply ./policies --resource Certificate.yml
```
