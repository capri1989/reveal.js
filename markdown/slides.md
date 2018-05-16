# Ceph - The distributed storage solution

<hr>
<p>Kai Wagner | <a href="mailto:kwagner@suse.com">kwagner@suse.com</a></p>
<p>openSUSE Conference Prague 2018</p>

---

### Content for today

* Introduction
* Different types of storage
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

---

# Different types of storage

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

### The two different ways of scaling

--

### Scale-Up

<img src="images/scale-up.jpg" style="background:none; border:none; box-shadow:none;">

--

### Scale-out

<img src="images/scale-out.jpg" style="background:none; border:none; box-shadow:none;">

--

### There's more!

--

### Single application to rule them all

<img src="images/application.jpg" style="background:none; border:none; box-shadow:none;">

--

### Way to many options

<img src="images/lollipops.jpg" style="background:none; border:none; box-shadow:none;">

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

#### Ceph Components 

<img src="images/ceph_stack.png" style="background:none; border:none; box-shadow:none;">

