# Drone on Google Container Engine (minimal example)

This directory contains an example of the simplest viable Drone deployment
on [Google Container Engine](https://cloud.google.com/container-engine/).
We eschew nice things like HTTPS in the name of conciseness.

## Prep work

There are a few things you'll need to do prior to using these manifests.
How to do them is beyond the scope of this example, but here's a rough
run-down:

### Create and format a Disk

* Create a Disk to hold your SQLite DB. This can be pretty small.
* You'll want to mount it to a Compute Engine instance and format it with
  ext4. Un-mount and you can destroy the Compute Engine instance you used
  for the formatting.
* Take note of the zone you created the disk in. You'll need to make sure
  that your Container Engine cluster is in the same zone.
  
### Create the Container Engine cluster

In the same zone as your DB Disk, create your GKE cluster. You can start
with a single node if you'd like.

## Load the manifests

Making sure that your ``kubectl`` client is configured correctly (see the
GKE docs), load these manifests:

```
kubectl create -f drone.service.yaml
```

This will create a service, which is going to leave with you an IP that
points at a front-facing load balancer. Use ``kubectl get svc`` to find
the external IP. You can stop to point a DNS entry now or do it later.

```
kubectl create -f drone.controller.yaml
```

This will create a Replication Controller, which will make sure there's one
Drone pod running on your cluster at all times. You can verify that it started
correctly by doing something like:

```
kubectl get pods
```

You should see a single pod in a "Running" state. If there were issues,
note the name of the pod and look at the logs:

```
kubectl logs -f droneio-a123
```

Where ``droneio-a123`` is the pod name.
