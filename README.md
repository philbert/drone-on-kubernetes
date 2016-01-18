# Drone on Kubernetes

This repository contains example [Kubernetes](http://kubernetes.io/) 
manifests for Drone. Each subdirectory is a different setup, as
described below:

* ``gke-minimal-example`` - The most stripped down example that we could
  manage for Drone running on 
  [Google Container Engine](https://cloud.google.com/container-engine/)
  (hosted Kubernetes).
  
## The State of Drone on Kubernetes

Everything works great on Kubernetes as of 1.3.x. There is only one issue
to work around:

* ["/sys/fs/docker: read-only file system" during docker publish step](https://github.com/drone/drone/issues/1352)

If you see /sys/fs errors, check the issue above for details. The root
cause is a Docker bug that will be fixed in Docker 1.10. 
  
## Contributions welcome!

If you have anything to add or improve, please don't hesitate to send
pull requests.

## License

The contents of this repository are licensed under the Apache 2.0 License.
