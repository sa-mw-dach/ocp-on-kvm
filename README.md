# OCP on KVM

## Table of Contents

* [OCP on KVM](#ocp-on-kvm)  
  * [Purpose](#purpose)  
  * [Design Principles](#design-principles)  
  * [General Setup](#general-setup)  
    * [Prerequisites](#prerequisites)  
    * [Basic Steps I: What is done?](#basic-steps-i-what-is-done)  
    * [Basic Steps II: What do you need to do?](#basic-steps-ii-what-do-you-need-to-do)  
    * [Target Architecture](#target-architecture)  
    * [Tasks](#tasks)  (just non-option/unconditional in this TOC)
        * [Setup KVM on RHEL 8.1](#setup-kvm-on-rhel-81)  
        * [Setup PXE-Boot for KVM](#setup-pxe-boot-for-kvm)  
        * [(Optional) Add Bridging](#optional-add-bridging)  
        * [(Optional) Setup Outbound Proxy](#optional-setup-outbound-proxy)  
        * [(Optional) Setup Docker Repository (f.e. Nexus) for Detached Install](#optional-setup-docker-repository-fe-nexus-for-detached-install)  
        * [Stage Secret](#stage-secret)  
  * [Reference, Inspiration, Links](#reference-inspiration-links)  
  * [TO DO/WIP](#to-dowip)  

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
  * Root access to your machine.
  * You need to have access to the Red Hat Enterprise Linux (RHEL) binaries. E.g. head over to [Red Hat Developers](https://developers.redhat.com/) and create an account.
  * Ansible 2.8++.
  * Terminal access.

### Basic Steps I: What is done?

  1. KVM gets installed and set up.
  1. Based on an _inventory_ file, a single KVM node ("Bootstrap" node) is created for further deployment of the cluster).
  1. When the bootstrap node is up, it is used to create the cluster nodes.

### Basic Steps II: What do you need to do?

1. Clone this repository (or make a fork: contributions welcome!):  
````
git clone https://github.com/sa-mw-dach/ocp-on-kvm.git
cd ocp-on-kvm
````  

2. Login to RHN and download the RHEL 8.1 DVD image from https://access.redhat.com/downloads/content/479/ver=/rhel---8/8.1/x86_64/product-software and place it under ``/root``, e.g. ``/root/rhel-8.1-x86_64-dvd.iso``

3. Create an individual SSH-Key  
````
ssh-keygen -q -t rsa -f /root/.ssh/id_ocplabs -C "" -N ""
````

4. Create an [Ansible vault](https://docs.ansible.com/ansible/latest/user_guide/vault.html) file.  
````
touch group_vars/all/vault.yml
````
Put in this information and replace the placeholders ``<rhn-user>`` and ``<rhn-password>``:  
````
vault_rhn_user: <rhn-user>
vault_rhn_password: <rhn-password>
````
and encrypt it:  
````
ansible-vault encrypt group_vars/all/vault.yml
````
Make sure you can remember the password given as you'll need it later.  

5. Create an inventory file.  
````
touch inventory
````
Add content to the inventory file according to your infrastructure setup. You can find an [example inventory file here](https://raw.githubusercontent.com/sa-mw-dach/ocp-on-kvm/documentation-kg/documentation/example-files/inventory-example).

6. Run the playbook  
````
ansible-playbook -vvv --ask-vault-pass -i inventory playbook.yml > ansible.log 2>&1
````

### Target Architecture
The target architecture consists of the Bootstrap 1..3..5..n Master and 1..n Worker (aka Compute) nodes. Supportive functionality (DNS, DHCP) is provisioned on the Bootstrap node.

See this basic archictural diagram: ![High Level Architecture](/documentation/images/general_architecture.png).

### Tasks
The following paragraphs give an overview of the individual steps needed to setup the OpenShift cluster. These tasks are fully automated.

#### Setup KVM on RHEL 8.1

KVM/libvirt installation needed for running the OpenShift cluster. Deep explanation of many steps needed to be done can be found in the [official OpenShift installer documentation](https://github.com/openshift/installer/tree/master/docs/dev/libvirt).

#### Setup DNS  
Installed unconditionally.

#### Setup NTP  
Installed unconditionally.

#### Setup DHCP  
Installed unconditionally.

#### Setup TFTP  
Installed unconditionally.

#### Setup PXE-Boot for KVM  
Check the `ìnventory``file if your setup needs the various ``kvm_`` parameters.

#### (Optional) Add Bridging  
If you don't think you need bridging, remove  
``
kvm_network_mode=bridge
``  
in your ``ìnventory`` file.

#### Update Firewall  
Done unconditionally where needed.

#### Setup HTTP Server  
Installed unconditionally.

#### Setup HA Proxy  
Installed unconditionally.

#### (Optional) Setup Outbound Proxy  
Needed if you want to simulate real life settings (like in TelCo) where all traffic is proxied. Done via

``
squid_install=True
``

in the ``inventory`` file.

#### (Optional) Setup Docker Repository (f.e. Nexus) for Detached Install  
Nexus will be installed if you set

``
ocp_install_type=disconnected
``

*and*

``
nexus_version=<your-nexus-version>
``

e.g.

``
nexus_version=3.20.1-01
``

in the ``inventory`` file.

#### Stage and Extract Client and Installer  
Done automatically.

#### Stage Secret  
Still on the to-do list, see below. Contributions welcome!

#### Setup Ignition Config  
Done automatically.

#### Create Ignition Config with Installer  
Done automatically.

#### Copy Ignition Configs to HTTP Folder  
Done automatically.

#### Start Bootstrap  
Done automatically.

#### Install and Customize all VMs  
Done automatically.

#### virsh start VMs  
Done automatically.

#### Add Worker Nodes  
Done automatically.

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
Contributions welcome!

  * Stage secret automatically.
  * Make RHEL DVD iso file placement configurable. (Unfortunately it doesn't seem possible automating the download).
  * Make a pre-check of your configuration.
