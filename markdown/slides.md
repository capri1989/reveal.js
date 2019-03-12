<div><img alt="ceph logo" width="35%" src="images/ceph-logo.png" style="background:none; border:none; box-shadow:none;"></div>

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

Note:
Ceph ist eine Unified Storage Plattform welche Objekt, Block und Filestorage zur Verfügung stellen kann

--

#### Release Schedule

<img src="images/release-schedule.png" style="background:none; border:none; box-shadow:none;">

* Stable, named release every 9 months
* Backports for 2 releases
* Upgrade up to 2 releases at a time
 * e.g Luminous -> Nautilus, Mimic -> Octopus

Note:
Aktuelle Release Zyklus ist 9 Monate.
Es wird dabei ein n-2 n+2 Upgrade unterstützt
Maximum von 18 Monaten aktuell möglich bevor ein Upgrade ansteht

--

#### Ceph Priorities

* Usability and management

* Container ecosystem

* Performance

* Multi- and hyprid cloud

Note:
Upstream hat 4 Leitelemente aktuell:
Usability and management - Ceph hat immer den Beigeschmack sehr schwierig zu benutzen zu sein, von der Installation über das Mangement
Performance - Jeder will All-flash und NVMe und somit muss auch Ceph nachziehen. Aktuell gibt ein ein Projekt Names "crimson" was den OSD stack neu definiert für diesen Fall
Container Ecosystem - kubernetes, csi


---

# Management

--

#### Ceph Dashboard

<img src="images/ceph-dashboard.png" style="background:none; border:none; box-shadow:none;">

Note:
Größte Neuigkeit in diesem Bereich ist das Ceph Dashboard
Erste read-only Implementierung mit Luminous
Jetzt dank dem openATTIC team und Projekt haben wir endlich eine nutzbare und hilfreiche UI

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

Note:
Orchestrator Abstraktion
Die Idee dahinter ist einen Orchestrator Layer zu haben der egal welches Tool Ceph nutzen will dies vereinheitlich und vereinfacht
Aktuell wird an rook, ansible, DeepSea und ssh gearbeitet.
Ceph spricht mit dem Orchestrator egal wer diese Anfrage einkippt und kümmert sich um die Ausführung
Das ist die Grundlage für eine einheitliche ceph-cli

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
Wir sind sehr froh das wir endlich soweit sind und somit auch nur noch wenige Schritte davon entfernt endlich einen kompletten Cluster im Dashboard managen zu können.

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

Note:
Eines der großen RADOS features in Nautilus
Leute waren immer verwirrt und keiner wusste genau wie er die PGs definieren sollte
Es war immer möglich die pg_num zu erhöhen, aber nicht zu verringern - fixed in Nautilus
PGs können automatisch im Hintergrund anpassen - Es schaut sich den Cluster und die Daten an und passt das die pg_num an
Es kann informieren oder es selbst machen, kann gewählt werden - wenn unterschied von soll zu sein >3 ist

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

--

#### RADOS Miscellany

* CRUSH can convert/reclassify legacy maps
 * Transition from old, hand-crafted maps to new device classes (new in Luminous) no longer shuffles all data
* OSD hard limit on PG log length
 * Avoids corner cases that could cause OSD memory utilization to grow unbounded
* Clay erasure code plugin
 * Better recovery efficiency when less m nodes fail (for a k+m code)

---

# RGW

--

#### RGW

* pub/sub
 * Subscribe to events like PUT
 * Polling interface, recently demoed with knative at KubeCon Seattle
 * Push interface to AMQ, Kafka coming soon
* Archive zone
 * Enable bucket versioning and retain all copies of all objects
* Tiering policy, lifecycle management
 * Implements S3 API for tiering and expiration
* Beast frontend for RGW
 * Based on boost::asio
 * Better performance and efficiency
* STS

---

# RBD

--

#### RBD Live Image Migration

<img src="images/rbd-live-migration.png" style="background:none; border:none; box-shadow:none;">

--

#### RBD Top

* RADOS instracture
 * ceph-mgr instructs OSDs to sample requests
  * Optionally with some filtering by pool, object name, client, etc.
 * Results aggregated by mgr
* rbd CLI presents this for RBD images specifically

--

#### RBD Misc

* rbd-mirror: remote cluster endpoint config stored in cluster
 * Simpler configuration experience!
* Namespace support
 * Lock down tenants to a slice of a pool
 * Private view of images, etc.
* Pool-level config overrides
 * Simpler configuration
* Creation, access, modification timestamps

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

--

#### CephFS Misc

* Cephfs shell
 * CLI tool with shell-like commands (cd, ls, mkdir, rm)
 * Easily scripted
 * Useful for e.g., setting quota attributes on directories without mounting the fs
* Performance, MDS scale(-up) improvements
 * Many fixes for MDSs with large amounts of RAM
 * MDS balancing improvements for multi-MDS clusters

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
* Working hard toward v1.0 release
 * Focus on ability to support in production environments

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

<img src="images/linux-foundation.png" style="background:none; border:none; box-shadow:none;">

--

#### Cephalocon Beijing

* Inaugural Cephalocon APAC took place in March 2018
 * Beijing, China
 * 2 days, 4 tracks, 1000 attendees
 * Users, vendors, partners, developers
* 14 industry sponsors

<img src="images/cephalocon-beijing.png" style="background:none; border:none; box-shadow:none;">

--

#### Cephalocon Barcelona

* Cephalocon Barcelona 2019
 * May 19-20, 2019
 * Barcelona, Spain
 * Similar format: 2 days, 4 tracks
* Co-located with KubeCon + CloudNativeCon
 * May 20-23, 2018
* https://ceph.com/cephalocon/

