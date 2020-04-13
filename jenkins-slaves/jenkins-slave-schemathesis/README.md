# jenkins-slave-schemathesis
Provides a docker image of the python runtime and schemathesis for use as a Jenkins slave.

## What is Schemathesis?
https://github.com/kiwicom/schemathesis <br/>
*Description and explanation of why this is useful as a jenkins slave*

"Schemathesis is a tool for testing your web applications built with Open API / Swagger specifications. It reads the application schema and generates test cases which will ensure that your application is compliant with its schema. The application under test could be written in any language, the only thing you need is a valid API schema in a supported format." - From the linked URL

This Jenkins agent allows for an easy integration into a pipeline where you can insure that your backend is meeting what is defined in your API contact. Otherwise the build will fail and you will be left with a compliant backend.

## Build local
`docker build -t jenkins-slave-schemathesis .`

## Run local
For local running and experimentation run `docker run -i -t jenkins-slave-schemathesis /bin/bash` and have a play once inside the container.

## Build in OpenShift
```bash
oc process -f ../../.openshift/templates/jenkins-slave-generic-template.yml \
    -p NAME=jenkins-slave-python \
    -p SOURCE_CONTEXT_DIR=jenkins-slaves/jenkins-slave-python \
    -p DOCKERFILE_PATH=Dockerfile \
    | oc create -f -
```
For all params see the list in the `../../.openshift/templates/jenkins-slave-generic-template.yml` or run `oc process --parameters -f ../../.openshift/templates/jenkins-slave-generic-template.yml`.

## Jenkins
Add a new Kubernetes Container template called `jenkins-slave-python` (if you've build and pushed the container image locally) and specify this as the node when running builds. If you're using the template attached; the `role: jenkins-slave` is attached and Jenkins should automatically discover the slave for you. Further instructions can be found [here](https://docs.openshift.com/container-platform/3.7/using_images/other_images/jenkins.html#using-the-jenkins-kubernetes-plug-in-to-run-jobs). Python installation commands are slightly modified from the SCL versions, which can be found [here](https://github.com/sclorg/s2i-python-container/tree/master/3.6).
