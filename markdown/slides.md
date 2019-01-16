# Ceph management with openATTIC Salt and DeepSea

<hr>
<p>Kai Wagner | <a href="mailto:kwagner@suse.com">kwagner@suse.com</a></p>
<p>Lars Marowsky-Brée | <a href="mailto:lmb@suse.com">lmb@suse.com</a></p>
<p>SOC All-Hands

</p>

---

### Content for today

* Introduction
* Ceph
* Salt and DeepSea
* Prometheus and Grafana
* openATTIC 3.x
* Ceph Manager Dashboard
* SES 6
* Live Demo

---

# Introduction

--

### Who are we?

```
[capri] (capri@fulda.de): Kai Wagner
[capri] #ceph #ceph-dashboard #opensuse #suse #storage 
[capri] capri.suse.de :SUSE Employee 
[capri] idle 262744:02:41, signon: Thu Jan 26th,1989 23:26
[capri] End of WHOIS list.
```

```
[lmb] (lmb@berlin.de): Lars Marowsky-Brée
[lmb] #ceph #suse #storage 
[lmb] lmb.suse.de :SUSE Employee 
[lmb] idle, signon: $$$ $$$ $$,$$$$ 
[lmb] End of WHOIS list.
```

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

--

### SOC Integration

* Create pools for use by glance, cinder, cinder-backup and nova
* Create users for glance, cinder and cinder-backup
* Can be modified with "prefix" option

--

### CLI Example

```
salt-run --out=yaml openstack.integrate
salt-run --out=yaml openstack.integrate prefix=other
```

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
* RBD snapshot management
* Ceph Object Gateway management (RGW Admin Ops API)
* Support Ceph Luminous features (e.g. pool compression)
* Web-based configuration
* Detailed feedback to the user on how to resolve config issues

---

#### openATTIC Architecture

<img src="images/openattic-architecture.png" style="background:none; border:none; box-shadow:none;">

---

# Ceph Manager Dashboard

--

### Dashboard V1

--

### Initial implementation

* Added in Ceph Luminous
* Ceph health status, logs, performance metrics
* List of nodes, OSDs
* RBD images, mirroring status, iSCSI daemon status
* Python Backend (CherryPy)
* Javascript UI (Rivets.JS)

--

<img src="images/dashboardv1_frontpage.png" style="background:none; border:none; box-shadow:none;">

--

### Added after Luminous
  
* RGW details
* MON list
* Perf counters
* Config settings browser

--

### Dashboard v1 Limitations

* "read-only" - no management functionality
* No built-in authentication system
* Limited functionality of Rivet.JS

---

# Dashboard v2 retrospective 

--

* Jan. 2018: Initial discussions with Sage and John
  * POC of Ceph Mgr Dashboard converted to Angular
* Feb. 22nd 2018: Dashboard v2 development branch created
* Mar. 06th 2018: Milestone 1 merged - feature parity with Dashboard v1

--

### Dashboard v2 overview

* Modular Python backend (CherryPy), RESTful API
* WebUI (Angular 5), inspired by / derived from openATTIC UI
* Basic username/password authentication
* All features from Dashboard v1 from current ``master`` branch
  * Additional management features

--

### New backend improvements

* Asynchronous tasks management
* Browseable REST API
* Block device (RBD) management
* RGW bucket and user management
* Ceph pool management

--

### New UI improvements

* A usage bar component
* Tooltips
* Asynchronous task manager
* Erasure code profile management
* RGW user and bucket management
* Block device (RBD) management
* Ceph pool management
* User Role Management

--

### ...there is more 

* Grafana proxy
* Further Ceph pool management
* Clusterwide Ceph OSD management
* Configuration settings editor
* Localization
* User permissions (based on pages)
* RBD mirroring

--

### Upcoming/WIP 

* Support for managing RBD QoS
* Profiles to set cluster's rebuild performance
* Runner to enable/disable storage enclosure LED
* iSCSI/NFS management

---

### SES 6

* SLE 15 SP1
* Ceph Nautilus
* DeepSea based deyployment
* Ceph Manager Dashboard (replacement for openATTIC)


---

### Live Demo

<a href="http://dilbert.com/strip/2000-12-30" target="_blank"><img src="images/ceph-mgr-dashboard.jpg" /></a>
