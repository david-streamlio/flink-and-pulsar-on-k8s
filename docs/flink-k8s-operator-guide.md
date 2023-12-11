# Getting Started with Pulsar and Flink on K8s

This guide will demonstrate how to develop an Apache Flink application that consumes data from Apache Pulsar, and
deploy it inside a Kubernetes environment.

--------------------
Prerequisites
--------------------
- A running Kubernetes cluster at version >= 1.24 with access configured to it using kubectl and helm.

- You must have appropriate permissions to list, create, edit and delete pods in your cluster. You can verify that you
  can list these resources by running: `kubectl auth can-i <list|create|edit|delete> pods`.

- You must have Kubernetes DNS configured in your cluster.


--------------------
Flink K8s Operator Installation Guide
--------------------
The Flink Kubernetes Operator extends the Kubernetes API with the ability to manage and operate Flink Deployments. The operator features the following amongst others:

![overview.svg](images%2Foverview.svg)

- Deploy and monitor Flink Application and Session deployments 
- Upgrade, suspend and delete deployments 
- Full logging and metrics integration 
- Flexible deployments and native integration with Kubernetes tooling

This section cover the process of installing the K8s operator for Apache Flink inside your local environment.

## Step 1: Install the Flink Kubernetes Operator
The easiest way to install the Kubernetes Operator for Apache Flink is to use the Helm chart.

```
export RELEASE_NAME=flink-kubernetes-operator
export FLINK_OPERATOR_K8S_NS=flink-operator
helm repo add flink-operator-repo https://downloads.apache.org/flink/flink-kubernetes-operator-1.7.0

helm install $RELEASE_NAME flink-operator-repo/flink-kubernetes-operator --namespace $FLINK_OPERATOR_K8S_NS --create-namespace
```

The arguments used in the helm install command are as follows:

- `$RELEASE_NAME`: This is the name given to the release of the Helm chart. It can be any name you choose and is used
  to identify the specific instance of the application being deployed.

- `flink-operator-repo/flink-kubernetes-operator`: This specifies the chart repository and the name of the chart to be installed. In
  this case, we are installing the `flink-kubernetes-operator` chart from the `flink-operator-repo` repository.

- `--namespace`: This specifies the namespace where the Flink Operator will be deployed. A namespace provides a logical
  boundary for resources and helps isolate different components and applications within a Kubernetes cluster.

## Step 2: Verify the Installation of the Flink K8s Operator
In order to confirm that the helm chart was deployed properly, you can run the following command. If everything is
installed correctly, you should see output similar to that shown below:

```
helm status --namespace $FLINK_OPERATOR_K8S_NS $RELEASE_NAME

NAME: flink-kubernetes-operator
LAST DEPLOYED: Mon Dec  4 17:17:59 2023
NAMESPACE: flink-operator
STATUS: deployed
REVISION: 1
TEST SUITE: None
```


You can also confirm that the K8s operator itself was deployed by checking the status of the components in the K8s
namespace using the following command: `kubectl -n $FLINK_OPERATOR_K8S_NS get all`

The output should look similar to this:
```
NAME                                          READY   STATUS    RESTARTS   AGE
pod/flink-kubernetes-operator-f4bbff6-frw5t   2/2     Running   0          44s

NAME                                     TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)   AGE
service/flink-operator-webhook-service   ClusterIP   10.152.183.147   <none>        443/TCP   44s

NAME                                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/flink-kubernetes-operator   1/1     1            1           44s

NAME                                                DESIRED   CURRENT   READY   AGE
replicaset.apps/flink-kubernetes-operator-f4bbff6   1         1         1       44s
```


-------------------
References
-------------------
1. https://nightlies.apache.org/flink/flink-kubernetes-operator-docs-main/
2. https://nightlies.apache.org/flink/flink-kubernetes-operator-docs-main/docs/try-flink-kubernetes-operator/quick-start/