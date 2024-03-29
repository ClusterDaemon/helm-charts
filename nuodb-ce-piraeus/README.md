# nuodb-ce-piraeus-helm

A NuoDB CE Helm Chart for OpenShift with support for both ephemeral and
persistent storage, backed by the Piraeus Datastore.


# https://blog.openshift.com/getting-started-helm-openshift/
# https://blog.openshift.com/from-templates-to-openshift-helm-charts/
# https://www.mirantis.com/blog/install-kubernetes-apps-helm/


Requirements: OKD or OpenShift v3.11 Cluster running with Helm.


Node Labeling
-------------

Before running the NuoDB Community Edition (CE) persistent storage template,
you must first label the nodes you want to run NuoDB pods.

The first label, "nuodb.com/zone", constrains on which nodes NuoDB pods are
permitted to run. For example:

  oc label node <node-name> nuodb.com/zone=east

Note: the label value, in this example "east", can be any value.

Next, label one of these nodes as your storage node, where you provide persistent
storage for your database. Ensure there is sufficient disk space on this node if using the local storage class, otherwise use the Piraeus storage class (detailed below). To create this label:

  oc label node <node-name> nuodb.com/node-type=storage

Storage Classes and Volumes
---------------------------

NuoDB uses persistent storage for the Admin Service pods and the storage managers. For each of these you need to define a storage class.

The storage class for the Admin Service pod is configurable by the template parameters.
Enter the storage class name you wish to provision for the Admin Service pods.

The storage class for the storage manager is predefined within the `local-disk-class.yaml`  file, along with the volume used by the storage manager. To create these:

  oc create -f local-disk-class.yaml

Alternatively, to back the storage manager with a durable Piraeus Datastore, you may use the following storage class instead:

  oc create -f sc-piraeus-two-replicas.yaml
	

Example of deploying a NuoDB CE application using Helm
------------------------------------------------------

oc new-project nuodb

# Install the NuoDB CE Helm chart
helm install <path-to>/nuodb-ce-helm

# Get the Helm Release Name
helm list --namespace nuodb --output yaml | grep  "^  Name:" | awk '{print $2}')
echo "Helm Release Name: $RELEASE_NAME"


echo "Helm Release Status:"
helm status $RELEASE_NAME


