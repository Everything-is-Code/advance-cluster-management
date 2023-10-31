# Install the RHACM operator and create the CustomResourceDefinition (CRD) object MultiClusterHub

//TODO revisar argorepo 

You can use Kustomize templates to install the RHACM operator.

The directory structure looks like this:


```
//TODO poner tree
```

```
oc apply -k bootstrap/

applicationset.argoproj.io/advance-cluster-management created
```

```
watch oc get pod -n open-cluster-management

Every 2.0s: oc get pods -n open-cluster-management
NAME                                                             READY  STATUS   RESTARTS  AGE
multicluster-observability-operator-7564f7d4f-qgjdp              1/1    Running  0         110s
multicluster-operators-application-6586bd956d-lqfsn              3/3    Running  0         58s
multicluster-operators-channel-6d74647db7-kp4k7                  1/1    Running  0         45s
multicluster-operators-hub-subscription-5555c5d88c-zkbgj         1/1    Running  0         45s
multicluster-operators-standalone-subscription-75bf6c5684-xnk59  1/1    Running  0         45s
multicluster-operators-subscription-report-78b7fdf784-xn2q8      1/1    Running  0         45s
multiclusterhub-operator-848bcd6c8-dxbw9                         1/1    Running  0         110s
submariner-addon-cd889f6b9-7dn6b                                 1/1    Running  0         110s

```

Wait until the RHACM operator is installed. It can take up to 10 minutes for the MultiClusterHub custom resource status to display as Running in the status.phase field after you run this command:

```
$ oc get mch -o=jsonpath='{.items[0].status.phase}' -n open-cluster-management
Running
```

You now have RHACM and Argo installed!!!

# Integrate GitOps and RHACM
When we apply the bootstrap folder this is done automatically, worse if we want to do it manually we must execute the following

RHACM introduces a new gitopscluster resource kind, which connects to a placement resource to determine which clusters to import into Argo CD. This integration allows you to expand your fleet while Argo CD automatically begins working with your new clusters. This means if you leverage Argo CD ApplicationSets, your application payloads are automatically applied to your new clusters as they are registered by RHACM in your Argo CD instances.

![image](https://github.com/Everything-is-Code/advance-cluster-management/assets/10425803/3636b465-4c0a-4ada-8c56-964a62fffced)

Run the following command to perform the GitOps operator integration with RHACM:

```
oc apply -k bootstrap/03-acm-argocd-integration/
applicationset.argoproj.io/acm-argocd-integration created
```
