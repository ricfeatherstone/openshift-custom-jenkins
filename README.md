# Openshift Custom Jenkins

A custom Jenkins image for OpenShift with the [Jenkins Client DSL Plugin](https://github.com/openshift/jenkins-client-plugin) installed.

Based on [this](https://blog.openshift.com/openshift-pipelines-jenkins-blue-ocean/) OpenShift Blog entry.


## Building

```
oc new-build jenkins:2~https://github.com/ricfeatherstone/openshift-custom-jenkins.git --name=jenkins-openshift-client
```


## Launching
```
oc new-app jenkins-ephemeral \
    -p JENKINS_IMAGE_STREAM_TAG=jenkins-openshift-client:latest \
    -p MEMORY_LIMIT=2Gi
```

