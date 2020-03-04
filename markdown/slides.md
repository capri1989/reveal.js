<div><img alt="ceph logo" width="35%" src="images/ceph-logo.png" style="background:none; border:none; box-shadow:none;"></div>

<hr>
Ceph meets Container! 
</hr>

<hr>
<p>Kai Wagner | <a href="mailto:kwagner@suse.com">kwagner@suse.com</a></p>
<p>openSUSE Conference 2020

</p>

---

### Content for today

* Where are we?
* Container on Ceph? Ceph in Container?
 * Rook
 * Podman
* cephadm
 * ceph-salt
* Live Demo

---

# Where are we?

--

#### Release Schedule

<img src="images/release-schedule.png" style="background:none; border:none; box-shadow:none;">

Note:
* Current release cycle is every 12 Month
* Upgrade support n+2
* Max support between releases limited to 24 month

--

#### What's new in Octopus?

* Output of **ceph status** or **ceph -s** is now more concise
* The telemetry module now reports more information - **ceph telemetry on**
* Health warning if **pg_num** is not a power of two
* **pg_autoscale_mode** is now set to `on` by default for newly created pools
* RBD snapshot based mirroring
* Ceph Dashboard can deploy/remove OSDs

[https://docs.ceph.com/docs/master/releases/octopus/](https://docs.ceph.com/docs/master/releases/octopus/)

Note:

* Telemetry - health statistics of ssd/hdd
 * CephFS - how many MDS, which features are on, how many data pools...

---

#### Container on Ceph? Ceph in Container?

--

#### Container on Ceph

* Much investment in ceph-csi
 * RWO and RWX support via RBD and/or CephFS
 * Snapshots, clones and so on

--

#### Container on Ceph

* Well known and functional use-case
* Can be used with all various types of mgmt plattforms
* Feature set of ceph-csi becomes more and more feature rich

--

#### Ceph in Container?

* We have two different types at the moment
* Rook+Kubernetes or Podman

--

<img src="images/rook-logo.png" style="background:none; border:5px; box-shadow:none;">

--

#### Operator for running Storage in Kubernetes

* Framework for many storage providers / solutions
* Open Source (Apache 2.0)
* Hosted by CNCF

Note:

* Rook is a framework to run storage backends such like Ceph/Cassandra
 * Cassandra is a distributed database management system 
 * Generally Rook provides persistant storage
* Running the whole components in containers we can simply go ahead and update/change or spawn new ones if we want
* We utulize what K8s gives us - so we're talking to the K8s API
* Hosted by the Cloud Native Computing Foundation

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

Note:

* kubectl - Simply allows you to interact with the K8s API
* Nodes running kublet which is our node component
* Long term goal is to only spawn mons/mgr and spawning the rest will/can be done within the Ceph Dashboard
* Rook discover is a pod that discovers the disk in the machines
* Operator can based on the information from the discover pod create the OSDs
* Rook Agent is the FlexVolume driver atm and will be the CSI component

-- 

<img src="images/rook-architecture02.png" style="background:none; border:none; box-shadow:none;">

--

# Podman

--

#### Podman

* Uses systemd
* Supports multiple images including OCI and Docker
* Container image management
* Full container lifecycle management
* Support for Docker compatible CLI through Podman

---

# cephadm

--

#### cephadm

* Container
* systemd (podman)
* Replacement for ceph-deploy, ceph-ansible, DeepSea

--

#### Benefits

* Uses same container as Rook
* HA for Ceph orchestration
* Single deployment tool across all vendors

--

#### Upgrades

* Automate updates of point releases
* A proper upgrade story for co-located daemons
* Make Ceph upgrades independent from OS upgrades

--

#### cephadm Workflow

* Start with minimal bootstrap process: One monitor + one manager
* Bring up Ceph Dashboard
* Rest is day two operation

--

#### "Rest is day two operation"

* Orchestrator CLI or Dashboard
* Adding OSDs, RGWs, Monitors, MGR etc.
* Each daemon is declaritive
* You have to execute **one CLI command** per type
 * MONs, MGRs, OSDs, RGWs, etc.

--

#### Scope of cephadm

* Doesn't install any RPM packages
* Doesn't setup NTP
* Doesn't care about OS upgrades
* Needs to be bootstraped

--

#### ceph-salt

* Deploy Ceph clusters using cephadm

```
Components:
 1. ceph-salt-formula is a Salt Formula using Salt Highstates 
    to manage Ceph minions.
 2. ceph-salt is a CLI tool to manage the Salt Formula.
```

--

* OS management (performs OS updates, ntp, tuning)
* Installs required RPM dependencies
* Bootstrap cephadm
* Enhanced bootstrapping by defining roles for Salt minions
* Migration from DeepSea to cephadm

[https://github.com/ceph/ceph-salt](https://github.com/ceph/ceph-salt)

--

Architecture

<img src="images/architecture_ceph_salt.png" style="background:none; border:none; box-shadow:none;">

--

#### Orchestrator Integration

* Dashboard integration
* Orchestrator CLI:

```
$ ceph orchestrator {host,device,service} ls
$ ceph orchestrator {mon,mgr,osd,rgw,...} {add,rm,update} ...
```

---

# Live Demo

--

#### sesdev 

CLI tool to deploy and manage Ceph cluster

[https://github.com/SUSE/sesdev](https://github.com/SUSE/sesdev)

--

#### Install sesdev from package

```
$ sudo zypper ar https://download.opensuse.org/repositories/filesystems:/ceph/<repo> filesystems_ceph
$ sudo zypper ref
$ sudo zypper install sesdev
```

#### Install sesdev from source

```
$ git clone https://github.com/SUSE/sesdev.git
$ cd sesdev
$ virtualenv venv
$ source venv/bin/activate
$ pip install --editable .
```
