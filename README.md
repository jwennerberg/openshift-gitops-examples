# OpenShift GitOps Argo CD demo

## Bootstrap

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

2. Install Sealed Secrets Operator
```
oc apply -k base/sealed-secrets-operator/
```


