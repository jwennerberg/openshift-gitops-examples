# OpenShift GitOps Argo CD demo

## Bootstrap

Bootstrap the environment by installing the OpenShift GitOps Operator and a `cluster-config` Argo CD `Application`.

These steps needs to be done with `cluster-admin` rights.

0. Clone this repo
```
git clone https://github.com/jwennerberg/openshift-gitops-argocd-demo.git
cd openshift-gitops-argocd-demo/
```

1. Install OpenShift GitOps Operator
```
oc apply -k base/openshift-gitops-operator/
```

In OpenShift 4.7.x the OpenShift Pipelines (Tekton) Operator will also be installed automatically.

2. Configure cluster with Argo CD

Use the cluster wide OpenShift GitOps Argo CD instance to add cluster configuration.
The following Argo CD Application will create a number of namespaces and rolebindings for demo purposes as well as install the Sealed Secrets Operator.

```
oc apply -f argocd/cluster/argocd-app-cluster-config.yaml -n openshift-gitops
```


## Manage application environment with GitOps

With the cluster bootstrapped and configured the follwing can be done by a regular user with access only to a subset of namespaces.
The default rolebindings adds user `user1` as namespace admin.

1. Deploy application Argo CD instance

```
oc apply -f overlays/demo/argocd/argocd.yaml -n jwennerb-infra
```

2. Give Argo CD permission to manage our other namespaces

```
oc policy add-role-to-user edit system:serviceaccount:jwennerb-infra:argocd-argocd-application-controller -n jwennerb-dev
oc policy add-role-to-user edit system:serviceaccount:jwennerb-infra:argocd-argocd-application-controller -n jwennerb-stage
```

3. Create Argo CD Appliction manifest

```
oc apply -f overlays/demo/argocd/argocd-app-demoapp.yaml -n jwennerb-infra
```

4. Verify Argo CD instance

4.1. Get Argo CD hostname

```
oc get route argocd-server -o jsonpath='{.status.host}' -n jwennerb-infra
```

4.2. Extract default admin password

```
oc extract secret/argocd-cluster --to=- -n jwennerb-infra
```

