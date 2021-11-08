# Local development of the operator-pipelines

Running the Tekton pipelines demands Openshift cluster. 

## Prerequisites

1. Optionally, if there is no access to existing Openshift Cluster-
In local environment, the Red Hat CodeReady Containers (CRC) can be used as one.
Install [CodeReady Containers](https://code-ready.github.io/crc/#installation_gsg)
2. Install [OpenShift Pipelines](https://docs.openshift.com/container-platform/4.7/cicd/pipelines/installing-pipelines.html)
3. Install the [tkn](https://console-openshift-console.apps-crc.testing/command-line-tools) CLI for your cluster version

## Initial Setup

The following steps assume you have access to a local CRC test cluster and have
logged in via the `oc` CLI (as developer).

1. Login to your cluster with `oc` CLI. 

a). In existing OpenShift Cluster: 

To get the credentials on existing Openshift Cluster, click on your username in
right top corner, and choose `copy login command`.

b). in CodeReady Containers, use command:
```bash
crc console --credentials
```

2. Create a test project in your cluster

```bash
oc new-project playground
```

3. Grant the `privileged` SCC to the default `pipeline` service account.

The `buildah` task requires the `privileged` security context constraint in order to call
`newuidmap`/`newgidmap`. This is only necessary because `runAsUser:0` is defined in
`templates/crc-pod-template.yml`.

```bash
oc adm policy add-scc-to-user privileged -z pipeline
```

4. Create pipelines and tasks as specified in the [README](../README.md)

## Running a Pipeline

> :warning: It may be necessary to pass the following `tkn` CLI arg to avoid permission issues with the default CRC PersistentVolumes.

```bash
--pod-template templates/crc-pod-template.yml
```