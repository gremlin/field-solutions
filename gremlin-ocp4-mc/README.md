# gremlin-ocp4-mc
OpenShift 4 Machine Config for Gremlin Chaos Engineering Platform

# What is this?
Gremlin Chaos Engineering Platform (https://www.gremlin.com/) is a way to help
test your Kubernetes cluster to determine its resiliance by doing nasty things
to it.

Red Hat® OpenShift® is a hybrid cloud, enterprise Kubernetes
application platform.  The current release is OpenShift Container Platform 4.x (OCP 4.x)

Gremlin requires adding a few SELinux modules to each of your cluster nodes
as detailed here: https://www.gremlin.com/docs/infrastructure-layer/install-openshift4/
Doing this by hand is a bit onerous for any reasonably large clusters.
Additionally, OCP 4.x recommends using Machine Configs to make node changes
https://docs.openshift.com/container-platform/4.6/post_installation_configuration/machine-configuration-tasks.html

This project is to create a usuable Machine Config based on the required settings.

# How it does it
This repo contains a templated machine config.  This template is modified in the designated location
by taking the base64 encoded SELinux modules required found at 

https://github.com/gremlin/selinux-policies/blob/master/policies/gremlin-openshift4.cil

This is done as a straight cut and paste of the results of the following commands:

RHEL: 
`curl -s https://raw.githubusercontent.com/gremlin/selinux-policies/master/policies/gremlin-openshift4.cil | base64 -w0 > gremlin-openshift4.cil.b64`

MacOS: 
`curl -s https://raw.githubusercontent.com/gremlin/selinux-policies/master/policies/gremlin-openshift4.cil | base64 -o gremlin-openshift4.cil.b64`

(You may need to use `set maxmempattern=2000000` or something in your .vimrc file)
You can also try using `:r gremlin-openshift4.cil.b64` at the correct spot in the file.  Make sure it is on the
correct line and there is no space!

You will need (should) change the name of the Machine Config to match the Node label as well as updating the node label
to match the node types you wish to update.

This also includes a completed worker Machine Config for nodes labelled as `worker`  It is current as of 2020/12/07
so if there have been any updates to the gremlin-openshift4.cil after that date, it will not be reflected.

To apply to Openshift 4.1 - 4.5 cluster:

`oc create -f 95-worker-gremlin-semodule.yaml`

To apply to Openshift 4.6+ cluster:

`oc create -f 96-worker-gremlin-semodule.yaml`


# Support
This software is available under the [Apache 2.0 License](LICENSE). 

It is delivered as Open Source, we will do our best to respond to issues posted in this Github Repo. 
