# OCP on KVM

## Purpose
In the wild, libvirt/KVM based OpenShift installation are used for development, demo, PoC and other, normally non-productive, purposes (see [the official installer documentation](https://github.com/openshift/installer/tree/master/docs/dev/libvirt])). 


Though different approaches exist addressing an automated OpenShift installation (see references below), they deploy to specific target environments. This project aims to be agnostic when it comes to the installation platform.


## Design Principles

  1. Solution should work on different infrastructure setups.
  1. The number of nodes should easily be adjustable (i.e. 1/3/n master nodes etc).
  1. Specific setups for individual target platforms shall be made configurable, not hard coded.
  1. Ansible is used for automation (vs. Go/other lang individual programs).


## General Setup



### Target Architecture

### Tasks 

#### Setup KVM on RHEL 8.1

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


