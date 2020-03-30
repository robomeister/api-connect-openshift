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

## Step 3 - Assign admin access to the tiller service account for the APICG project

```
oc policy add-role-to-user admin "system:serviceaccount:apicg:tiller"
```

## Step 4 - Assign any-uid SCC to pods running in the APICG project

```
oc adm policy add-scc-to-group anyuid system:serviceaccounts:apicg
```

## Step 5 - Install oc and kubectl command line executables

Helm uses the `kubectl` command under the covers, so it will need to be available along with the `oc` command:

```
https://github.com/openshift/origin/releases/download/v3.11.0/openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit.tar.gz
```

Extract the `oc` (if neccessary) and `kubectl` executables, placomg them in an executable path.


## Step 6 - Install Helm Client

```
# wget https://get.helm.sh/helm-v2.16.0-linux-amd64.tar.gz
# tar -zvxf helm-v2.16.0-linux-amd64.tar.gz
# cp linux-amd64/helm /usr/local/bin
# chmod 755 /usr/local/bin/helm
```

## Step 7 - Initialize Helm Client

```
 oc login <openshift endpoint> --username=username --password=password
 export TILLER_NAMESPACE=apicg
 helm init --client-only
 helm version
```

You should see the following:

```
Client: &version.Version{SemVer:"v2.16.0", GitCommit:"e13bc94621d4ef666270cfbe734aaabf342a49bb", GitTreeState:"clean"}
Server: &version.Version{SemVer:"v2.16.0", GitCommit:"e13bc94621d4ef666270cfbe734aaabf342a49bb", GitTreeState:"clean"}
```

