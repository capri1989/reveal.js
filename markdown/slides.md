## Scale-out storage for Kubernetes with Rook, Ceph and CSI

<hr>
<p>Marc Koderer | <a href="mailto:marc.koderer@sap.com">marc.koderer@sap.com</a></p>
<p>Kai Wagner | <a href="mailto:kwagner@suse.com">kwagner@suse.com</a></p>
<p></p>

---

### Content for today

* Introduction
* What is Ceph?
* What is Rook?
 * Architecture 
* Kubernetes storage integration
 * Native
 * CSI is the future!
* Demo

---

## Introduction

--

### Who are we?

```
[capri] (capri@fulda.de): Kai Wagner
[capri] #ceph #ceph-dashboard #opensuse #suse #storage 
[capri] capri.suse.de :SUSE Employee 
[capri] idle 256189:02:41, signon: Thu Jan 26th,1989 23:26
[capri] End of WHOIS list.
```

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

### Ceph core compontens

--

### MON

* Tracks & Monitor the health of the cluster

* Maintains a master copy of the cluster map

* Consensus for distributed decision making

* MONs DO NOT serve data to clients

--

### OSD

* Object Storage Deamon

* Store the actual data as objects on physical disks

* Serve stored data to clients

* Replication mechanism included

* Minimum of 3 OSDs recommended for data replication

--

### Pools

* Logical container for storage objects

* Replicated or erasure coding

* Dedicated CRUSH rules

--

### Placement groups

* Helper to balance the data across the OSDs

* One PG typically spans several OSDs

* One OSD typically serves many PGs

* Recommended ~150 PGs per OSD

--

### CRUSH map 

* Controlled Replication Under Scalable Hashing

* MONs maintain the CRUSH map

* Topology of any environment could be build (row,rack,host,az...)

--

### That sounds to complicated

--

### Isn't there a Web-UI?

--

<img src="images/ceph-dashboard.png" style="background:none; border:none; box-shadow:none;">

---

## What is Rook?

--

* Operator for running Storage in Kubernetes
 * Extends Kubernetes with custom types
 * Automation for Deployment, Scaling, Upgrading, etc for the Storage.
* Framework for many storage providers / solutions
 * Common Types for, e.g., Storage Selection shared
* Open Source (Apache 2.0)
* Hosted by CNCF

--

### Architecture

<img src="images/rook-architecture.png" style="background:none; border:none; box-shadow:none;">

---

## Kubernetes storage integration

--

### Native

--

### CSI is the future!

---

## Demo

