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
Postgresql image : `https://catalog.redhat.com/software/containers/rhel8/postgresql-10/5ba0ae0ddd19c70b45cbf4cd`
```
oc new-app \
    -e POSTGRESQL_USER=superset \
    -e POSTGRESQL_PASSWORD=superset \
    -e POSTGRESQL_DATABASE=supersetdb \
    postgresql:10
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
### install Open Data Hub
Follow the instruction `https://opendatahub.io/docs/getting-started/quick-installation.html` to installl Open Data Hub Operator, then instantiate an Open Data Hub deployment

Open Data Hub install superset, configure the security. You can configure/customize superset through configmaps :
- superset-config to configure cpu/ram requests/limits and secrets names of superset and superset_db pods
- superset-config.py to customize 

### login to superset
get the route 
```
⇣ ❯ oc get route | grep superset
superset   superset-demo-superset-helm.apps.cluster-z7f6f.z7f6f.sandbox644.opentlc.com          superset   http
```
Use the url to access to superset using the Openshift credentials

