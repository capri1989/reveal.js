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

* Automatic discovery, deployment, configuration and life cycle 
* Initial support for importing other Ceph clusters ( deployed via ceph-deploy)
* RADOS Gateway deployment (for single site deployments)
* CephFS MDS deployment and CephFS creation
* Sharing CephFS or S3 buckets via NFS Ganesha

--

### ... there is more

* iSCSI target management via lrbd
* Deployment and configuration of Prometheus and Grafana 
* Deployment and configuration of openATTIC 

---

# openATTIC

--

### openATTIC Goals

* Open Source Ceph management & monitoring GUI tool

* A tool that admins actually want to use

* That scales without becoming overwhelming

* Still should allow changes to be made elsewhere, without becoming inconsistent

--

### openATTIC - Ceph features in 2.0.x

* Ceph Cluster Status Dashboard (Performance Graphs, Health Status)
* Pool management (view/create/delete) 
* Pool monitoring
* Manage EC profiles
* RBD management (view/create/delete/map)
* RBD monitoring
* View OSDs and their details
* View cluster nodes & roles (via DeepSea)
* CRUSH map “editor”
* Support for managing multiple Ceph clusters

--

### openATTIC - Notable changes in 3.x

* Major code refactoring – Ceph-only focus
* Stateless – no information about Ceph is stored locally
* Simplified installation (single package, less dependencies)
* Nagios/Icinga & PNP4Nagios replaced by Prometheus & Grafana
* Usability improvements
* Notification system
* More robust error handling 
* Web-based configuration
* Detailed feedback to the user on how to resolve configuration issues

--

### openATTIC - New Ceph features in 3.x

* New dashboards and performance graphs (Prometheus / Grafana)
* Ceph Object Gateway management (RGW Admin Ops API)
* iSCSI target management (lrbd)
* NFS share management (NFS Ganesha)
* Support Ceph Luminous features (e.g. pool compression)
* Improved Pool and RBD management
* Manage cluster-wide OSD flags
* Node monitoring

---

# Prometheus and Grafana

--

### Prometheus and Grafana

* Prometheus collects and stores time series data
* Grafana makes it fit for human consumption
* Usually just exposed via openATTIC dashboard
* Standalone dashboard still accessible

---

#### openATTIC Architecture

<img src="images/openattic-architecture.png" style="background:none; border:none; box-shadow:none;">

---

# Live Demo

