# Drone on Google Container Engine (minimal example)

This directory contains an example of the simplest viable Drone deployment
on [Google Container Engine](https://cloud.google.com/container-engine/).
We eschew some important things like HTTPS in the name of conciseness.

## Prep work

There are a few things you'll need to do prior to using these manifests.
How to do them is beyond the scope of this example, but here's a rough
run-down (you can do most of this from their web console):

### Create and format a Disk

* Create a Disk to hold your SQLite DB. This can be pretty small.
* You'll want to mount the Disk to a Compute Engine instance, then SSH in
  and ``mkfs.ext4`` the disk to format it.
* Un-mount the disk and destroy the Compute Engine instance you used
  for the formatting.
* Take note of the zone you created the disk in. You'll need to make sure
  that your Container Engine cluster (which you are about to create)
  is in the same zone.
  
### Create the Container Engine cluster

In the same zone as your DB Disk, create your GKE cluster. You can start
with a single node if you'd like.

See the GKE [Getting started guide](https://cloud.google.com/container-engine/docs/before-you-begin)
for guidance on this.

### Load the manifests

Making sure that your ``kubectl`` client is configured correctly (see the
[GKE docs](https://cloud.google.com/container-engine/docs/before-you-begin)), 
load these manifests:

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


### Stuck? Need help?

We've glossed over quite a few details, for the sake of brevity. If you
have questions, post them to our [Help!](https://discuss.drone.io/c/help)
category on the Drone Discussion site.
