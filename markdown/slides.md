# BlueStore - A New Architecture

<hr>
<p>Kai Wagner | <a href="mailto:kwagner@suse.com">kwagner@suse.com</a></p>

---

### Content for today

* Introduction
* Why BlueStore?
* Architecture
* BlueStore Model
* Features

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
[capri] idle 258153:01:13, signon: Thu Jan 26th,1989 23:26
[capri] End of WHOIS list.
```

---

# Why BlueStore?

--

* Ceph stores objects as files on a filesystem (FileStore)
* No atomicity (Btrfs workaround)
* Write-ahead journaling
* Not ideal to store objects as files on a POSIX filesystem
 * rbdudata.382e5017b81.0000000000000000_head_58A36D34_1

---

# Architecture

--

<img src="images/ceph-bluestore.png" style="width: 40%; height: 40%; background:none; border:none; box-shadow:none;">

--

<img src="images/ceph-inside-bluestore.png" style="width: 50%; height: 50%; background:none; border:none; box-shadow:none;">

--

<img src="images/ceph-inside-bluestore-explained.png" style="background:none; border:none; box-shadow:none;">

--

#### So what do we store in RocksDB?

* Objects Metadata
* Write-ahead log
* Ceph omap data
* Allocator metadata

---

# BlueStore Model

--

<img src="images/bluestore-on-disk.png" style="background:none; border:none; box-shadow:none;">

--

#### Up to three devices

* main - stores the object data 
* db - stores the metadata as will fit
* WAL - stores just the journal

--

<img src="images/bluestore-on-disk-advanced.png" style="background:none; border:none; box-shadow:none;">

---

# Features

* Good performance
* No more double write penalty
* Deployment flexibility
* Raw block usage
* Inline compression and full data checksums

---

# Credits

Special thanks to Sébastien Han for the pictures and the informative blog post
