# Openshift Jenkins Update Pipeline

An OpenShift Pipeline to update the image used by Jenkins i.e to add new
plugins or configuration to an existing deployment

[OpenShift Jenkins](https://github.com/openshift/jenkins)
[OpenShift Jenkins Pipeline DSL](https://github.com/openshift/jenkins-client-plugin) 

The update pipeline performs the following steps

1.  Create and launch an new S2I build for the custom Jenkins image
2.  Launch and expose a test instance of the custom image for validation
and wait for approval to roll out. 
3.  Delete the launched test instance
4.  Update the Jenkins deployment to use the tagged 'stable' image and
rollout 


## Setup

First create a project and bootstrap the OpenShift Jenkins 2 image to 
include the [OpenShift Pipeline DSL Plugin](https://github.com/openshift/jenkins-client-plugin) 
before launching the Jenkins Persistent template with the custom image.

```
oc new-project cicd
oc new-build jenkins:2~https://github.com/ricfeatherstone/openshift-jenkins-update-pipeline.git \
    --name=jenkins-bootstrap
oc new-app jenkins-persistent \
    -p NAMESPACE=$(oc project -q) \
    -p JENKINS_IMAGE_STREAM_TAG=jenkins-bootstrap:latest \
    -p MEMORY_LIMIT=2Gi \
    -e OVERRIDE_PV_CONFIG_WITH_IMAGE_CONFIG=true \
    -e OVERRIDE_PV_PLUGINS_WITH_IMAGE_PLUGINS=true \
    -e OPENSHIFT_JENKINS_JVM_ARCH=x86_64
```


## Launching the Update Pipeline

```
oc new-build jenkins:2~https://github.com/ricfeatherstone/openshift-jenkins-update-pipeline.git \
  --context-dir=pipelines \
  --strategy=pipeline \
  --name=jenkins-pipeline
```
