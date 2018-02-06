# Ceph management with openATTIC

<hr>
<p>Kai Wagner | <a href="mailto:kwagner@suse.com">kwagner@suse.com</a></p>
<p>Ceph Day Germany 2018</p>

---

### Content for today

* openATTIC retrospective
* Salt and DeepSea
* Prometheus and Grafana
* openATTIC 3.x
* Outlook
* Live Demo

---

### openATTIC retrospective

--

### First commit Jan. 2011

--

### Started as local storage management Web-UI

<img src="images/openattic-1.x.png" style="background:none; border:none; box-shadow:none;">

--

##### Switched from XML-RPC/ExtJS to REST/Angularjs

<img src="images/openattic-newv2.png" style="background:none; border:none; box-shadow:none;">

--

##### First Ceph management commit in Feb. 2014

<img src="images/openattic-new-feature.png" style="background:none; border:none; box-shadow:none;">

--

### Crush Map Editor

<img src="images/openattic-crush-map.png" style="background:none; border:none; box-shadow:none;">

--

### Inital Ceph Monitoring

<img src="images/openattic-v2-monitoring.png" style="background:none; border:none; box-shadow:none;">

--

##### Since 2017 we're focusing on Ceph only

<img src="images/openattic-v3-dashboard.png" style="background:none; border:none; box-shadow:none;">

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

* Stateless – no information about Ceph is stored locally
* Dashboard and performance graphs (Prometheus / Grafana)
* Basic OSD Management - Manage cluster-wide osd flags
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

---

# Outlook

--

### This is openATTIC!

<img src="images/openattic-login.png" style="background:none; border:none; box-shadow:none;">

--

### openATTIC goes upstream!

<img src="images/upstream-login.png" style="background:none; border:none; box-shadow:none;">
