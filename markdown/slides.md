# Ceph - The distributed storage solution

<hr>
<p>Kai Wagner | <a href="mailto:kwagner@suse.com">kwagner@suse.com</a></p>
<p>openSUSE Conference Prague 2018</p>

---

### Content for today

* Introduction
* Isn't a single NAS system enough?
* What's Ceph?

---

# Introduction

--

### Who am I?

```
[capri] (capri@fulda.de): Kai Wagner
[capri] #ceph #ceph-dashboard #opensuse #suse #storage 
[capri] capri.suse.de :SUSE Employee 
[capri] is using modes +v
[capri] is using a secure connection
[capri] idle 257053:01:13, signon: Thu Jan 26th,1989 23:26
[capri] End of WHOIS list.
```

--

<img src="images/listen-to-me.jpg" style="background:none; border:none; box-shadow:none;">

--

<img src="images/its-not-a-joke.jpg" style="width: 60%; height: 60%; background:none; border:none; box-shadow:none;">

--

### So let's start!

---

# Isn't a single NAS system enough?

--

### Single hard drive

--

<img src="images/single-hard-drive.jpg" style="background:none; border:none; box-shadow:none;">

--

### Multiple hard drives

<img src="images/multiple-disks.jpg" style="background:none; border:none; box-shadow:none;">

--

### Where to put them?

--

### Dedicated server needed!

<img src="images/server.jpg" style="background:none; border:none; box-shadow:none;">

--

### Racks....we need more racks!

<img src="images/datacenter.jpg" style="background:none; border:none; box-shadow:none;">

--

### There's more!

--

### Single application to rule them all

<img src="images/application.jpg" style="background:none; border:none; box-shadow:none;">

--

### Way to many options

<img src="images/lollipops.jpg" style="background:none; border:none; box-shadow:none;">

--

### The two different ways of scaling

--

### Scale-Up

<img src="images/scale-up.jpg" style="background:none; border:none; box-shadow:none;">

--

### Scale-out

<img src="images/scale-out.jpg" style="background:none; border:none; box-shadow:none;">

--

### One Administrator is enough

<img src="images/single-admin.jpg" style="height: 50%; width: 50%; background:none; border:none; box-shadow:none;">

--

### We need a team!

<img src="images/multiple-admins.jpg" style="background:none; border:none; box-shadow:none;">

--

### There must be a better way to solve this

--

### ...and of course there is

--

<img src="images/ceph.jpg" style="height: 50%; width: 50%">

---

# What's Ceph?

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

Ceph trades everything off for consistency

--

#### Ceph Architecture

<img src="images/ceph_stack.png" style="background:none; border:none; box-shadow:none;">

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

### Small cluster

<img src="images/cluster.png" style="background:none; border:none; box-shadow:none;">

--

### Write data

<img src="images/cluster-write.png" style="background:none; border:none; box-shadow:none;">

--

### Read data

<img src="images/cluster-read.png" style="background:none; border:none; box-shadow:none;">

---

<img src="images/questions.jpg" style="background:none; border:none; box-shadow:none;">

