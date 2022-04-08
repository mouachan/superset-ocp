# superset-ocp

## Description
Showcase the ability to deploy Superset application through helm and operator

## Using helm
### login to openshift
```
oc login <OCP_API_URL> -u <username> -p <password>
```
### create a new namespace
```
oc new-project demo-superset-helm
```
### create postgresql database
```
oc new-app \
    -e POSTGRESQL_USER=superset \
    -e POSTGRESQL_PASSWORD=superset \
    -e POSTGRESQL_DATABASE=supersetdb \
    postgresql:10.0
```
### add a service account
```
cd helm
oc apply -f scc.yaml
```
### allows users to run with any UID and any GID 
```
oc adm policy add-scc-to-user anyuid -z superset -n demo-superset-helm 
```
### install superset 
```
helm upgrade --install --values my-values.yaml superset superset/superset --debug
```
wait until all the resources are created.
expose superset :
```
oc expose svc/superset
```
### login to superset
get the route 
```
⇣ ❯ oc get route | grep superset
superset   superset-demo-superset-helm.apps.cluster-z7f6f.z7f6f.sandbox644.opentlc.com          superset   http
```
Use the url to access to superset using admin/admin credential, the OAUTH need to be configured into the chart helm before deployment

## delete superset 
```
helm delete superset
```

## Using opendatahub
### create a new namespace
```
oc new-project demo-superset-operator
```
### install ope datahub
Follow the instruction to installl Open Data Hub operator, then instantiate a deployment  Open Data Hub : https://opendatahub.io/docs/getting-started/quick-installation.html

Open Data Hub install superset, configure the security to use openshift OAUTH and allows users to run with any UID   

### login to superset
get the route 
```
⇣ ❯ oc get route | grep superset
superset   superset-demo-superset-helm.apps.cluster-z7f6f.z7f6f.sandbox644.opentlc.com          superset   http
```
Use the url to access to superset using the Openshift credentials

