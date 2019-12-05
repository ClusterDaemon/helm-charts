# Piraeus Operator Helm Chart

## Installing

From this directory run the following commands:

```
helm repo add bitnami https://charts.bitnami.com/bitnami # for LINSTOR's etcd dependancy
helm install --dep-up --name=etcd-hello ../piraeus-operator
```

## Deleting

Deleting a deployment must be done in this specific order to allow resources
to be reclaimed safely!

```
kubectl delete piraeusnodesets.piraeus.linbit.com etcd-hello-piraeus-operator
kubectl delete piraeuscontrollersets.piraeus.linbit.com etcd-hello-piraeus-operator
helm delete etcd-hello --purge
```
