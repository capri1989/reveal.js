# Ceph 

<hr>
Was ist neu in Nautilus und was kommt als Nächstes? 
</hr>

<hr>
<p>Kai Wagner | <a href="mailto:kwagner@suse.com">kwagner@suse.com</a></p>
<p>Chemnitzer Linuxtage 2019

</p>

---

### Content for today

* Introduction
* Management
* RADOS
* RGW
* RBD
* CephFS
* Container
* Community

---

# Introduction

--

### Who am I?

```
[capri] (capri@fulda.de): Kai Wagner
[capri] #ceph #ceph-dashboard #opensuse #suse #storage 
[capri] capri.suse.de :SUSE Employee 
[capri] idle 256189:02:41, signon: Thu Jan 26th,1989 23:26
[capri] End of WHOIS list.
```

--

<img src="images/ceph_stack.png" style="background:none; border:none; box-shadow:none;">

--

#### Release Schedule

<img src="images/release-schedule.png" style="background:none; border:none; box-shadow:none;">

* Stable, named release every 9 months
* Backports for 2 releases
* Upgrade up to 2 releases at a time
 * e.g Luminous -> Nautilus, Mimic -> Octopus

--

#### Ceph Priorities

* Usability and management

* Container ecosystem

* Performance

* Multi- and hyprid cloud

---

# Management

--

#### Ceph Dashboard

<img src="images/ceph-dashboard.png" style="background:none; border:none; box-shadow:none;">

--

#### Ceph Dashboard

* Community convergence in single built-in dashboard
 * Based on openATTIC and the dashboard prototype from Luminous
* Built-in and self-hosted
* Management functions
* Metric and monitoring
* Hardware/deplyoment management in progress...

--

#### Orchestrator Sandwich

<img src="images/orchestrator-sandwich.png" style="background:none; border:none; box-shadow:none;">

--

#### Orchestrator Sandwich

* Abstract deployment functions
 * Fetching node inventory
 * Creating or destroying daemon deployments
 * ... 
* Unified CLI for managing Ceph daemons
 * ceph orchestrator device ls [node]
 * ceph orchestrator osd create [flags] node device [device]
 * ...
* WIP - Enable dashboard GUI for deploying and managing daemons

--

#### PG Autoscaling

* Picking pg_num has historically been “black magic”
 * Limited/confusing guidance on what value(s) to choose
 * pg_num could be increased, but never decreased
* Nautilus: pg_num can be reduced
* Nautilus: pg_num can be automagically tuned in the background
 * Based on usage (how much data in each pool)
 * Administrator can optionally hint about future/expected usage
 * Ceph can either issue health warning or initiate changes itself

--

#### Device health metrics

* OSD and mon report underlying storage devices, scrape SMART metrics
* Failure prediction
 * Local mode: pretrained model in ceph-mgr predicts remaining life
 * Cloud mode: SaaS based service (free or paid) from ProphetStor
* Optional automatic mitigation
 * Raise health alerts (about specific failing devices, or looming failure storm)
 * Automatically mark soon-to-fail OSDs “out”

--

```
# ceph device ls 
DEVICE                                  HOST:DEV      DAEMONS    LIFE EXPECTANCY 
Crucial_CT1024M550SSD1_14160C164100     stud:sdd      osd.40     >5w             
Crucial_CT1024M550SSD1_14210C25EB65     cpach:sde     osd.18     >5w             
Crucial_CT1024M550SSD1_14210C25F936     stud:sde      osd.41     >8d             
INTEL_SSDPE2ME400G4_CVMD5442003M400FGN  cpach:nvme1n1 osd.10                  
INTEL_SSDPE2MX012T4_CVPD6185002R1P2QGN  stud:nvme0n1  osd.1                   
ST2000NX0253_S4608PDF                   cpach:sdo     osd.7                   
ST2000NX0253_S460971P                   cpach:sdn     osd.8                   
Samsung_SSD_850_EVO_1TB_S2RENX0J500066T cpach:sdb     mon.cpach  >5w  
```

--

#### Crash reports

* Previously crashes would manifest as a splat in a daemon log file, usually unnoticed...
* Now concise crash reports logged to /var/lib/ceph/crash/
 * Daemon, timestamp, version
 * Stack trace
* Reports are regularly posted to the mon/mgr
* ‘ceph crash ls’, ‘ceph crash info <id>’, ...
* If user opts in, telemetry module can phone home crashes to Ceph devs

---

# RADOS

---

# RGW

---

# RBD

---

# CephFS

---

# Container

---

# Community

---
