# OCP on KVM

## Purpose
In the wild, libvirt/KVM based OpenShift installations are used for development, demo, PoC, and other, normally non-productive, purposes (see [the official installer documentation](https://github.com/openshift/installer/tree/master/docs/dev/libvirt])). 


Though different approaches exist addressing an automated OpenShift installation (see references below), they deploy to specific target environments. This project aims to be agnostic when it comes to installation platforms: Wherever RHEL 8 runs, this setup should work.


## Design Principles

  1. Solution should work on different infrastructure setups.
  1. The number of nodes should easily be adjustable (i.e. 1/3/n master nodes etc).
  1. Specific setups for individual target platforms shall be made configurable, not hard coded.
  1. Ansible is used for automation (vs. Go/other lang individual programs).


## General Setup

### Prerequisites
  * You need to have access to the Red Hat Enterprise Linux (RHEL) binaries.
  * Ansible 2.8++
  * Terminal access.

### Basic Steps I: What is done?

  1. KVM gets installed and set up.
  1. Based on an _inventory_ file, a single KVM node ("Bootstrap" node) is created for further deployment of the cluster).
  1. When the bootstrap node is up, it is used to create the cluster nodes.

### Basic Steps II: What do you need to do?

  1. Clone this repository (or make a fork: contributions welcome!):  
``
git clone https://github.com/sa-mw-dach/ocp-on-kvm.git
``
  1. Create your inventory file.  
``
touch inventory
``
Add content to the inventory file according to your infrastructure setup. You can find an [example inventory file here](documentation/example-files/inventory-example).
  1. Run the playbook  
``
ansible-playbook -vvv --ask-vault-pass -i inventory playbook.yml > ansible.log 2>&1
``

### Target Architecture
The target architecture consists of the Bootstrap 1..3..5..n Master and 1..n Worker (aka Compute) nodes. Supportive functionality (DNS, DHCP) is provisioned on the Bootstrap node.

See this basic archictural diagram: ![High Level Architecture](/documentation/images/general_architecture.png).

### Tasks
The following paragraphs give an overview of the individual steps needed to setup the OpenShift cluster.

#### Setup KVM on RHEL 8.1

KVM/libvirt installation needed for running the OpenShift cluster. Deep explanation of many steps needed to be done can be found in the [official OpenShift installer documentation](https://github.com/openshift/installer/tree/master/docs/dev/libvirt).

#### Setup DNS  

#### Setup NTP  

#### Setup DHCP  

#### Setup TFTP  

#### Setup PXE-Boot  

#### (Optional) Add Bridging  

#### Uodate Firewall  

#### Setup HTTP Server  

#### Setup HA Proxy  

#### Setup Outbound Proxy  

#### Setup Docker Repository (f.e. Nexus) for Detached Install  

#### Stage and Extract Client and Installer  

#### Stage Secret  

#### Setup Ignition Config  

#### Create Ignition Config with Installer  

#### Copy Ignition Configs to HTTP Folder  

#### Start Bootstrap  

#### Install and Customize all VMs  

#### virsh start VMs  

#### Add Worker Nodes  

### Reference, Inspiration, Links  
In open source, you always stand on the shoulders of giants, so do we.


[OCP4 UPI Helpernode](https://github.com/christianh814/ocp4-upi-helpernode)  
Comprehensive single node setup. The helper node acts as bastion and bootstrap node.


[Hetzner OCP4](https://github.com/RedHat-EMEA-SSA-Team/hetzner-ocp4)  
An automated installation From the Red Hat EMEA "Tigers", specifically targeting the Hetzner dedicated hosting environment (https://www.hetzner.com/sb).


[OpenShift 4 IPI for Hetzner](https://jicken.gitbook.io/blog/guides/openshift-4-ipi-kvm-install-on-a-hetzner-root-server)  
Documentation of a KVM based IPI install on the Hetzner environment.




Some of us are actually giants in their own right ;) ...:


[Ansible lab](https://github.com/elajoie/lab.local)  
From [@jcordes73](https://github.com/jcordes73) - target platform is his famous "OpenShift-in-a-box" environment.


[OCP4 Labs-Ansible](https://github.com/jcordes73/ocp4labs-ansible)  
From [@elajoie](https://github.com/elajoie) - derived from practical field work.

### TO DO/WIP

  * NN
  * NN
