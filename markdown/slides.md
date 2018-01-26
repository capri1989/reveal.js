# Ceph management with openATTIC

<hr>
<p>Kai Wagner | <a href="mailto:kwagner@suse.com">kwagner@suse.com</a></p>
<p>FOSDEM 2018</p>

---

### The Components of openATTIC

* Ceph
* Salt and DeepSea
* openATTIC
* Prometheus and Grafana

---

# Ceph

--

### Ceph Overview

* A distributed storage system

* Object, block, and file storage in one unified system

* Designed for performance, reliability and scalability

--

### Ceph Motivation Principles

* Everything must scale / scale-out
* No single point of failure (SPOF)
* Commodity (off-the-shelf) hardware
* Self-manage (whenever possible)
* Open source (LGPL)

--

#### Ceph Components 

<img src="images/ceph_stack.png" style="background:none; border:none; box-shadow:none;">

---

# Salt

--

### Why did we choose Salt?

* Massive scalability and resilient environments
* Parallel execution
* Speed
* Easy to get started 
* Active and still growing community

---

# DeepSea

--
 
DeepSea is a collection of Salt files for deploying, managing and automating all aspects of a Ceph cluster

--

### Current Status

* Automatic discovery, deployment, configuration and life cycle management of Ceph clusters
* Initial support for importing other Ceph clusters ( deployed via ceph-deploy)
* RADOS Gateway deployment (for single site deployments)
* CephFS MDS deployment and CephFS creation
* Sharing CephFS or S3 buckets via NFS Ganesha

--

### ... there is more

* iSCSI target management via lrbd
* Deployment and configuration of Prometheus and Grafana 
* Deployment and configuration of openATTIC 
