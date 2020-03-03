# OCP on KVM

## Purpose
In the wild, libvirt/KVM based OpenShift installation are used for development, demo, PoC and other, normally non-productive, purposes (see [the official installer documentation](https://github.com/openshift/installer/tree/master/docs/dev/libvirt])). 


Though different approaches exist addressing an automated OpenShift installation 


## Design Principles


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


