<div><img alt="ceph logo" width="35%" src="images/ceph-logo.png" style="background:none; border:none; box-shadow:none;"></div>

<hr>
State of Ceph 
</hr>

<hr>
<p>Kai Wagner | <a href="mailto:kwagner@suse.com">kwagner@suse.com</a></p>
<p>openSUSE Conference 2019

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

<img src="images/ceph_stack.png" style="background:none; border:none; box-shadow:none;">

Note:
* Ceph is a unified storage solution which provides block, file and object storage

--

#### Release Schedule

<img src="images/release-schedule.png" style="background:none; border:none; box-shadow:none;">

* Stable, named release every 9 months
* Backport for 2 releases
* Upgrade up to 2 releases at a time
 * e.g Luminous -> Nautilus, Mimic -> Octopus

Survey - http://tiny.cc/ceph-release

Note:
* Current release cycle is every 9 Month
* Upgrade support n+2
* Max support between releases limited to 18 month

--

## Ceph Themes

* Usability
* Quality
* Performance
* Multi-site
* Ecosystem

Note:
* We have five principals we're focusing on upstream atm
* Usability - Ceph still is quite complex and the principal here is to make it easier to consume - orchestrator API, upgrade automation
* Quality - Telemetry and Crash reports, Doc and Test suite
* Performance - All-flash wanted but the current OSD stack was developed against spinners - new project called CRIMSON will target this gap
* Multi-site -  RGW S3 management, tiering, sync to clouds etc
* Ecosystem - Kubernetes + Rook, OpenStack integration, Analytics 


---

# Management

--

#### Ceph Dashboard

<img src="images/ceph-dashboard.png" style="background:none; border:none; box-shadow:none;">

Note:
* First read-only version was released with Luminous
* Thanks to the openATTIC team we now have the first version of a usable dashboard whith added value for the user

--

#### Ceph Dashboard

* Community convergence in single built-in dashboard
 * Based on openATTIC and the dashboard prototype from Luminous
* Built-in and self-hosted
* Management functions
* Metric and monitoring
* Hardware/deplyoment management in progress...

--

#### New functionality

* Support for multiple users / roles
* SSO (SAMLv2) for user authentication
* Auditing support
* New landing page, showing more metrics and health info
* I18N support
* REST API documentation with Swagger API

--

#### New management features included

* OSD management (mark as down/out, change OSD settings, recovery profiles)
* Cluster config settings editor
* Ceph Pool management (create/modify/delete)
* ECP management
* RBD mirroring configuration
* Embedded Grafana Dashboards (derived from Ceph Metrics)

--

#### . . . more features

* CRUSH map viewer
* NFS Ganesha management
* iSCSI target management (via ceph-iscsi)
* RBD QoS configuration
* Ceph Manager (ceph-mgr) module management
* Prometheus alert Management

--

#### Orchestrator Sandwich

<img src="images/orchestrator-sandwich.png" style="background:none; border:none; box-shadow:none;">

Note:
* Orchestrator abstraction
* the idea behind is to have an abstraction layer that can be used by any other mgr plugin so we have a unified way of handling it
* currently rook, ansible, DeepSea (salt) and ssh
* That's the baseline for a unified ceph-cli

--

#### Orchestrator Sandwich

* Abstract deployment functions
 * Fetching node inventory
 * Creating or destroying daemon deployments
 * Blinking device LEDs
 * ... 
* Unified CLI for managing Ceph daemons
 * ceph orchestrator device ls [node]
 * ceph orchestrator osd create [flags] node device [device]
 * ...
* WIP - Enable dashboard GUI for deploying and managing daemons

Note:
* We're super excited about that and we're only a few steps away to manage services in the dashboard as well

--

#### PG Autoscaling

* Picking pg_num has historically been “black magic”
 * Limited/confusing guidance on what value(s) to choose
 * pg_num could be increased, but never decreased
* Nautilus: pg_num can be reduced
* Nautilus: pg_num can be automagically tuned in the background
 * Based on usage (how much data in each pool)
 * Ceph can either issue health warning or initiate changes itself

Note:
* One of the biggest RADOS features in Nautilus
* PG calculation and the selection of it was always black magic
* PGs could be increased but never decreased
* Nautilus could do this for you now
* Could display just a warning or it could modify the pg_num on it's own if the PG num differes by *3

--

#### Device health metrics

* OSD and mon report underlying storage devices, scrape SMART metrics
* Failure prediction
 * Local mode: pretrained model in ceph-mgr predicts remaining life
 * Cloud mode: SaaS based service (free or paid) from ProphetStor
* Optional automatic mitigation
 * Raise health alerts (about specific failing devices, or looming failure storm)
 * Automatically mark soon-to-fail OSDs “out”

Note:
* We are now getting smart data from the devices itself
* Together with the blinky lights functionality we're getting a step closer to enterprise grade

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

Note:
* Previously logs were stored locally and the service just restarted so no one was aware of a crash
* Telemetry module only stores same basic information - not a security issue and no data will be leaked

---

# RADOS

--

#### Messenger V2

* New version of the Ceph on-wire protocol
* Goodness
 * Encryption on the wire
 * Improved feature negotiation
 * Improved support for extensible authentication
  * Kerberos is coming soon… hopefully in Octopus!
 * Infrastructure to support dual stack IPv4 and IPv6 (not quite complete)
* Move to IANA-assigned monitor port 3300
* Dual support for v1 and v2 protocols
 * After upgrade, monitor will start listening on 3300, other daemons will starting binding to new v2 ports
 * Kernel support for v2 will come later

Note:
* The traffic between the daemons in Ceph can now be encrypted
* Currently only IPv4 or IPv6 but not both - WIP

--

#### RADOS - MISC Management

* osd_target_memory
 * Set target memory usage and OSD caches auto-adjust to fit
* NUMA management, pinning
 * ‘ceph osd numa-status’ to see OSD network and storage NUMA node
 * ‘ceph config set osd.<osd-id> osd_numa_node <num> ; ceph osd down <osd-id>’
* Improvements to centralized config mgmt
 * Especially options from mgr modules
 * Type checking, live changes without restarting ceph-mgr
* Progress bars on recovery, etc.
 * ‘ceph progress’
 * Eventually this will get rolled into ‘ceph -s’...
* ‘Misplaced’ is no longer HEALTH_WARN

Note:
* It wasn't easy to say how much memory will be used by an OSD
* NUMA pinning of OSD nodes
* ceph.conf is now stored on the mons and only there
* progress command for long running tasks
* Misplaced is no longer a HEALTH_WARN but can be changed if you want it to be

--

#### Bluestore improvements

* New ‘bitmap’ allocator
 * Faster
 * Predictable and low memory utilization (~10MB RAM per TB SDD, ~3MB  RAM per TB HDD)
 * Less fragmentation
* Intelligent cache management
 * Balance memory allocation between RocksDB cache, BlueStore onodes, data
* Per-pool utilization metrics
 * User data, allocated space, compressed size before/after, omap space consumption
 * These bubble up to ‘ceph df’ to monitor e.g., effectiveness of compression
* Misc performance improvements

Note:

* Again, it's predictable how much memory will be used

--

#### RADOS Miscellany

* CRUSH can convert/reclassify legacy maps
 * Transition from old, hand-crafted maps to new device classes (new in Luminous) no longer shuffles all data
* OSD hard limit on PG log length
 * Avoids corner cases that could cause OSD memory utilization to grow unbounded
* Clay erasure code plugin
 * Better recovery efficiency when less m nodes fail (for a k+m code)

Note:
* New devices classes with SSD/HDD can noe be used without the need to reshuffel the whole data
* Some corner cases this resulted in running out of memory
* Clay gives you a better usage between bandwith and IO during a recovery operation

---

# RGW

--

#### RGW

* pub/sub
 * Subscribe to events like PUT
 * Polling interface
 * Push interface to AMQ, Kafka coming soon
* Archive zone
 * Enable bucket versioning and retain all copies of all objects
* Tiering policy, lifecycle management
 * Implements S3 API for tiering and expiration
* Beast frontend for RGW
 * Based on boost::asio
 * Better performance, efficiency and scaliblity 

Note:
* You can create zones where you could notify or listen on specific events - it creates an evenstream you could subscribe to
* A specific PUT could for example trigger some function as a service
* Archival zone - full copy of your data but versionized
* S3 API which allows to create tiering across different pools
* New frontend - before apache, civetweb and now Beast

---

# RBD

--

#### RBD Live Image Migration

<img src="images/rbd-live-migration.png" style="background:none; border:none; box-shadow:none;">

Note:
* Before you were bound to the pool you created the RBD in - now you can move it between different pools

--

#### RBD Top

* RADOS instracture
 * ceph-mgr instructs OSDs to sample requests
  * Optionally with some filtering by pool, object name, client, etc.
 * Results aggregated by mgr
* rbd CLI presents this for RBD images specifically

--

```
rbd perf image iotop

 >WRITES OPS    READS OPS  WRITE BYTES   READ BYTES    WRITE LAT     READ LAT IMAGE                                                       
        10/s         10/s    160 KiB/s        0 B/s       8.96 s       8.96 s rbd/img1
         7/s          0/s     30 MiB/s    2.4 MiB/s    451.06 ms      7.10 ms rbd/img2
```

--

#### RBD Misc

* rbd-mirror: remote cluster endpoint config stored in cluster
 * Simpler configuration experience!
* Namespace support
 * Lock down tenants to a slice of a pool
 * Private view of images, etc.
* Pool-level config overrides
 * Simpler configuration
* "rbd ls" - Creation, access, modification timestamps

Note:
* rbd-mirror is the Daemon that is used for the asynchronous replication - was added with Luminous
* You can create security domains within a pool to lock specific clients into it
* Before you were able to active rbd caching but not it's also possible to do it on a pool basis
* "rbd ls" now adds timestamps and some more details to it's output

---

# CephFS

--

#### CephFS Volumes and Subvolumes

* Multi-fs (“volume”) support stable
 * Each CephFS volume has independent set of RADOS pools, MDS cluster
* First-class subvolume concept
 * Sub-directory of a volume with quota, unique cephx user, and restricted to a RADOS namespace
 * Based on ceph_volume_client.py, written for OpenStack Manila driver, now part of ceph-mgr
* ‘ceph fs volume …’, ‘ceph fs subvolume …’

Note:
* Multi-fs is now declared stable. You can create multiple CephFS filesystems within your Ceph cluster
* Subvolume concept was copied from the OpenStack Manila driver and added to the ceph-mgr

-- 

#### CephFS NFS Gateways

* Clustered nfs-ganesha
 * active/active
 * Correct failover semantics (i.e., managed NFS grace period)
 * nfs-ganesha daemons use RADOS for configuration, grace period state
 * (See Jeff Layton’s devconf.cz talk recording)
* nfs-ganesha daemons fully managed via new orchestrator interface
 * Fully supported with Rook; others to follow
 * Full support from CLI to Dashboard
* Mapped to new volume/subvolume concept

Note:
* Jeff Layton developed the change that we can now store the config withing a rados object

--

#### CephFS Misc

* Cephfs shell
 * CLI tool with shell-like commands (cd, ls, mkdir, rm)
 * Easily scripted
 * Useful for e.g., setting quota attributes on directories without mounting the fs
* Performance, MDS scale(-up) improvements
 * Many fixes for MDSs with large amounts of RAM
 * MDS balancing improvements for multi-MDS clusters

Note:
* Was developed within an outreachy project - mostly used for scripting and CephFS
* Before you had to mount a CephFS to set quota attributes for example - that is no longer necessary and can be done by the cli

---

# Container

--

#### Kubernetes

* Expose Ceph storage to Kubernetes
 * Any scale-out infrastructure platform needs scale-out storage
* Run Ceph clusters in Kubernetes
 * Simplify/hide OS dependencies
 * Finer control over upgrades
 * Schedule deployment of Ceph daemons across hardware nodes
* Kubernetes as “distributed OS”

Note:
* We have two different views - we provide the storage underneath K8s or we move our services into k8s
* Specially the gateways would benefit of k8s cause those could be spawned and created on demand

--

#### Rook

* All-in on Rook as a robust operator for Ceph in Kubernetes
 * Extremely easy to get Ceph up and running!
* Intelligent management of Ceph daemons
 * add/remove monitors while maintaining quorum
 * Schedule stateless daemons (rgw, nfs, rbd-mirror) across nodes
* Kubernetes-style provisioning of storage
 * Persistent Volumes (RWO and RWX)
 * Coming: dynamic provisioning of RGW users and buckets
* Enthusiastic user community, CNCF incubation project
* Version v1.0 was released three weeks ago

--

#### Barebones Container Orchestration

* We have: rook, deepsea, ansible, and ssh orchestrator (WIP) implementations
* ssh orch gives mgr a root ssh key to Ceph nodes
 * Moral equivalent/successor of ceph-deploy, but built into the mgr
 * Plan is to eventually combine with a ceph-bootstrap.sh that starts mon+mgr on current host
* ceph-ansible can run daemons in containers
 * Creates a systemd unit file for each daemon that does ‘docker run …’
* Plan to teach ssh orchestrator to do the same
 * Easier install
  * s/fiddling with $random_distro repos/choose container registry and image/
 * Daemons can be upgraded individually, in any order, instead of by host

---

# Community


--

#### Ceph Foundation: What is it?

* Organized as a directed fund under the Linux Foundation
 * Members contribute and pool funds
 * Governing board manages expenditures
* Tasked with supporting the Ceph project community
 * Financial support for project infrastructure, events, internships, outreach, marketing, and related efforts
 * Forum for coordinating activities and investments, providing guidance to technical teams for roadmap, and evolving project governance
* 31 founding member organizations
 * 13 Premier Members, 10 General Members, 8 Associate members (academic and government institutions)
* 3 more members have joined since launch

<img src="images/linux-foundation.png" width="25%" style="background:none; border:none; box-shadow:none;">

--

#### Ceph Days

* Ceph Day Netherlands - Amsterdam - Jul 2
 * CFP open until early June
* Ceph Day CERN - Geneva - September 16
 * Will be finalized soon
* Ceph Day London - October 24
* Ceph Day Poland - Wrocław - October 28
* Propose your own! Contact us at events@ceph.io

https://ceph.com/cephdays

---

# Links

* https://capri1989.github.io/oSC19/
* https://github.com/ceph/ceph/
* https://github.com/ceph/ceph/blob/master/doc/releases/nautilus.rst
