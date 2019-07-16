## Scale-out storage for Kubernetes with Rook, Ceph and CSI

<hr>
<p>Marc Koderer | <a href="mailto:marc.koderer@sap.com">marc.koderer@sap.com</a></p>
<p>Kai Wagner | <a href="mailto:kwagner@suse.com">kwagner@suse.com</a></p>
<p></p>

---

### Content for today

* What is Ceph?
* What is Rook?
 * Architecture 
* CSI is the future!
* Demo

---

## What is Ceph?

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

<img src="images/ceph_stack.png" style="background:none; border:5px; box-shadow:none;">

--

Ceph trades everything off for consistency

--

### Other access methods

* iSCSI
* NFS
* Samba/CIFS

--

If this sounds too complicated, there's something for you!

--

<img src="images/ceph-dashboard.png" style="background:none; border:none; box-shadow:none;">

---

## What is Rook?

<img src="images/rook-logo.png" style="background:none; border:5px; box-shadow:none;">

--

#### Operator for running Storage in Kubernetes

 * Extends Kubernetes with custom types
* Framework for many storage providers / solutions
* Open Source (Apache 2.0)
* Hosted by CNCF

--

Rook turns distributed storage systems into:

* Self-manageable
* Self-scalable
* Self-healable storage services

--

...it also automates the tasks of a storage administrator:

* Deployment
* Bootstraping
* Configuration
* Provisioning
* Scaling/Upgrading/Migration ...

--

### Concepts of Rook

* Operator
 * Bootstrap and monitor the storage cluster
 * Starts and monitors Ceph monitor pods and a daemonset for OSDs
 * Manages CRDs for pools, object stores (S3/Swift) and file systems
 * Creates the Rook agents

--

* Agent
 * Deployed on every Kubernetes node
 * Configures the Flexvolume plugin
 * Handles all storage operations required on the nodes 

--

### Architecture

<img src="images/rook-architecture.png" style="background:none; border:none; box-shadow:none;">

--

<img src="images/rook-architecture02.png" style="background:none; border:none; box-shadow:none;">

---

### CSI is the future!

* Container storage interface
* Successor of flexvolume

Note: 

* Out of the tree module in kubernetes - you can modify and release it without a need to do a k8s release
* Flexvolume had to run on the hosts directly and with root rights and you needed all the binaries
* CSI is the onle supported and active mainted module atm - flexvolume won't get any new features

---

## Demo

