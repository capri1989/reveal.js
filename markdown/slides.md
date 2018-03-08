# Ceph management with openATTIC

<hr>
<p>Sebastian Wagner | <a href="mailto:swagner@suse.com">swagner@suse.com</a></p>
<p>Kai Wagner | <a href="mailto:kwagner@suse.com">kwagner@suse.com</a></p>
<p>Chemnitzer Linuxtage 2018</p>

---

### Content for today

* Ceph
* Salt and DeepSea
* Prometheus and Grafana
* openATTIC 3.x
* Outlook
* Live Demo

---

# Ceph

--

### Ceph Overview

* A distributed storage system

* Object, block, and file storage in one unified system

* Designed for performance, reliability and scalability

--

### Ceph Motivation Principles

* Everything must scale / horizontally
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

* Automatic discovery, deployment, configuration and life cycle 
* Initial support for importing other Ceph clusters (deployed via ceph-deploy)
* RADOS Gateway deployment (for single site deployments)
* CephFS MDS deployment and CephFS creation
* Sharing CephFS or S3 buckets via NFS Ganesha

--

### ... there is more

* iSCSI target management via lrbd
* Deployment and configuration of Prometheus and Grafana 
* Deployment and configuration of openATTIC 

---

# Prometheus and Grafana

--

### Prometheus and Grafana

* Prometheus collects and stores time series data
* Grafana makes it fit for human consumption
* Usually just exposed via openATTIC dashboard
* Standalone dashboard still accessible

---

# openATTIC

--

### openATTIC Goals

* Open Source Ceph management & monitoring GUI

* A tool that admins actually want to use

* That scales without becoming overwhelming

* Still should allow changes to be made elsewhere, without becoming inconsistent

--

### openATTIC Features

* Stateless - no information about Ceph is stored locally
* Dashboard and performance graphs (Prometheus / Grafana)
* Basic OSD Management - manage cluster-wide OSD flags
* Pool and RBD management (create, delete, edit)
* Node view and monitoring
* NFS share management (NFS Ganesha)

--

### ...more features

* iSCSI target management (lrbd)
* Ceph Object Gateway management (RGW Admin Ops API)
* Support Ceph Luminous features (e.g. pool compression)
* Web-based configuration
* Detailed feedback to the user on how to resolve configuration issues

---

#### openATTIC Architecture

<img src="images/openattic-architecture.png" style="background:none; border:none; box-shadow:none;">


