# Cluster Autoscaling

OpenShift ships an operator that can manage deployment of the Cluster Autoscaler component.

The operator is installed by default and visible here:

```sh
kubectl get deployments -n openshift-cluster-api cluster-autoscaler-operator
NAME                          DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
cluster-autoscaler-operator   1         1         1            1           1h
```

The operator watches for `ClusterAutoscaler` resources to know how to deploy the autoscaler.

Let's deploy and configure an autoscaler for our cluster.

Here is a sample configuration [file](../assets/cluster-autoscaler.yaml) for a `ClusterAutoscaler` object.

By editing the cluster autoscaler operator, users are able to cap growth of the
cluster.  The configuration caps the cluster from scaling beyond 24 nodes, 128
cores, and 256Gi of memory, and 16 gpus.


```sh
kubectl create -f https://raw.githubusercontent.com/openshift/cluster-autoscaler-operator/master/examples/clusterautoscaler.yaml
```

Let's verify the autoscaler is deployed by the operator:

```sh
kubectl get clusterautoscaler
NAME      AGE
default   2m
kubectl get deployments -n openshift-cluster-api
NAME                             DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
cluster-autoscaler-default       1         1         1            0           3m
```

Let's deploy a job to demonstrate autoscaling.

Next: [Machine Health Checks](03-machine-health-checks.md)