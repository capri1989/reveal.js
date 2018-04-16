# Dashboard v2

<hr>
<p>Kai Wagner | <a href="mailto:kwagner@suse.com">kwagner@suse.com</a></p>
<p>Ceph Day London 2018</p>

---

### Content for today

* Introduction
* History
  * openATTIC
  * Dashboard v1
* Dashboard v2
* Live Demo

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
[capri] idle 256189:02:41, signon: Thu Jan 26th,1989 23:26
[capri] End of WHOIS list.
```

---

# History

---

### openATTIC

--

* 2011: openATTIC was founded

--

<img src="images/openattic-1.x.png" style="background:none; border:none; box-shadow:none;">

--

* 2014: Added initial Ceph support

--

<img src="images/openattic-crush-map.png" style="background:none; border:none; box-shadow:none;">

--

* Feb. 2016: Collaboration with SUSE started

--

<img src="images/openattic-v2-monitoring.png" style="background:none; border:none; box-shadow:none;">

--

* Nov. 2016: openATTIC and team acquired by SUSE

--

<img src="images/openattic-team-2016.jpg" style="background:none; border:none; box-shadow:none;">

--

* 2017: openattic 3.x focuses on Ceph only

--

<img src="images/openattic-v3-dashboard.png" style="background:none; border:none; box-shadow:none;">

---

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

### Dashboard v2 Limitations

* "read-only" - no management funtionality
* No built-in authentication system
* Limited funtionality of Rivet.JS

---

# Dashboard v2 retrospective 

--

* Jan. 2018: Initial discussions with Sage and John
  * POC of Ceph Mgr Dashboard converted to Angular
* Feb. 22nd 2018: Dashboard v2 development branch created
* Mar. 06th 2018: Milestone 1 merged - feature partiy with Dashboard v1

--

### Submitted over 80 pull requests

--

### More than 60 pull requests have been merged already

--

### Lots of groundwork/foundation

--

### Dashboard v2 overview

* Modular Python backend (CherryPy), RESTful API
* WebUI (Angular 5), inspired by / derived from openATTIC UI
* Basic username/password authentication
* All features from Dashboard v1 from current ``master`` branch

--

### New backend improvements

* Asynchronous tasks management
* Browseable REST API
* Creating Pool via the REST API
* Creating RBDs via the REST API

--

### New UI improvements

* A usage bar component
* Tool tips
* Asynchronous task manager
* Erasure code profile management

--

### Upcoming/WIP

* Block device (RBD) management
* RGW user and bucket management
* Ceph pool management
* Configuration settiongs editor
* Localization

---

### Live Demo

<a href="http://dilbert.com/strip/2000-12-30" target="_blank"><img src="images/openattic-login.png" /></a>
