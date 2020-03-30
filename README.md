# Installing APIC Gateway into OpenShift using HELM without cluster-admin


## Step 1 - Create APICG Project

```
oc new-project apicg
```

## Step 2 - Install Tiller in APICG project

```
export TILLER_NAMESPACE=apicg
oc process -f https://github.com/openshift/origin/raw/master/examples/helm/tiller-template.yaml -p TILLER_NAMESPACE="${TILLER_NAMESPACE}" -p HELM_VERSION=v2.16.0 | oc create -f -
```

This template will create the service account `system:serviceaccount:apicg:tiller`.  


## Step x - Install Helm Client

```
# wget https://get.helm.sh/helm-v2.16.0-linux-amd64.tar.gz
# tar -zvxf helm-v2.16.0-linux-amd64.tar.gz
# cp linux-amd64/helm /usr/local/bin
# chmod 755 /usr/local/bin/helm
```
