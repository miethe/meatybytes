---
title: "Mastering NFS Server Deployment & Configuration"
date: 2023-06-24
description: "A comprehensive guide on deploying and configuring an NFS server on a DAS attached to a Dell R730, featuring example configurations, best practices, and a detailed setup guide for an NFS client."
tags: ["NFS", "Homelab", "Storage", "Dell R730", "RHEL", "DAS"]
topics: ["Homelab", "Storage"]
categories: ["Technical", "Introduction", "Guide"]
---

## Introduction

Welcome back, my tech-savvy readers! This is your go-to guy for all things OpenShift, Platform Engineering, and DevSecOps, [Nick Miethe](/about), the meat behind [MeatyBytes.io](/). As always, I'm here to guide you through the entire lifecycle of all things tech, and today we're getting back to basics on a fairly foundational topics - mastering the deployment and configuration of an NFS server. In this case, we're deploying on a DAS attached to a Dell R730 running RHEL 9.0 (don't chew me out!), but that is largely inconsequential.

### Overview

Why does this matter, you may ask? Well, **NFS** or Network File System is a widely-used distributed file system protocol that allows sharing of files and directories over a network. Understanding how to effectively deploy and configure an NFS server is key to efficiently managing file sharing and storage, especially in a homelab setup. And when you're working with a Direct Attached Storage (**DAS**) unit like the [Supermicro CSE-847](https://www.supermicro.com/en/products/chassis/4u/847/sc847be1c12-r1k68lpb4) attached to my Dell R730, it's important to get the configuration just right to maximize storage utility and network performance.

### Synopsis

In this post, we're going to deep dive into how to deploy and configure an **NFS server** on a DAS, or anywhere for that matter! We'll cover everything from the basics of NFS, example configurations, best practices, and a detailed setup guide for an NFS server and client. Along the way, I'll share some personal tips and tricks from my own homelab experiences.

So, if you're ready to take your NFS game to the next level, read on. Because this guide is about to beef up your server deployment and configuration skills like never before!

## Understanding NFS

Before diving into the technicalities of the deployment, it's essential to understand what NFS is and how it works. Network File System, or **NFS**, is a popular distributed file system protocol that operates based on the client-server model, with an NFS server hosting the files and directories to share, and the NFS client accessing the shared content over a network.

By leveraging NFS, users and programs can access files on remote systems as if they were local files.

### NFS Advantages

**Network File System** (NFS) provides several benefits, such as:

1. **Transparent Access**: It provides the same access rights to the client as if the resources were local to the client's system.
2. **Network Independence**: NFS protocols are designed to be independent of the physical network.
3. **Machine Independence**: NFS is implemented on various machines and operating systems.
4. **File System Semantics**: NFS offers a set of standard file operations, like open, close, read, write, etc.

## Mount Filesystem

Before we can setup NFS, we must mount a filesystem to our drives. If you've already done this, consider moving on to [NFS Server Configuration](#nfs-server-configuration).

### DAS Prerequisites

Ensure your Dell R730 is equipped with 12 10TB HDDs, and you have RHEL installed on your server.

### Step 1: Installing Necessary Packages

First, we need to install the required packages for NFS and XFS file system. You can do this by running the following command:

```bash
sudo yum install -y nfs-utils xfsprogs
```

### Step 2: Preparing the DAS

To use the DAS for NFS, we first need to format it with the XFS file system. Before doing so, confirm the name of the disk (replace `/dev/sdb` with your actual disk name):

```bash
sudo fdisk -l
```

Next, format the disk to XFS:

```bash
sudo mkfs.xfs /dev/sdb
```

### Step 3: Mounting the DAS

We'll now mount the DAS as the `/nfs` directory:

```bash
sudo mkdir /nfs
sudo mount /dev/sdb /nfs
```

To ensure the DAS is automatically mounted on boot, we need to add it to the `/etc/fstab` file. Add the following line to the end of the file (replace `/dev/sdb` with your actual disk name):

```bash
sudo echo "/dev/sdb /nfs xfs defaults 0 0" >> /etc/fstab
```

Next, we'll proceed to the share configuration.

## NFS Server Configuration

Now, let's dive into the core part of this guide: deploying and configuring an NFS server on a DAS attached to a Dell R730 running RHEL.

### NFS Prerequisites

A mounted filesystem, preferably XFS, such as explained above. Be sure the filesystem is blank, or has been backed up before proceeding!

### Step 1 (optional): Ensure Necessary Packages are Installed

If you skipped the instructions on mounting a filesystem for whatever reason, you will need to install the `nfs-utils`:

```bash
sudo yum install -y nfs-utils
```

### Step 2: Configuring the Shares

After installing the NFS packages, we will configure the shares. **Shares** represent the directories that you want to share over the network.

```bash
sudo mkdir /nfs
sudo chown nfsnobody:nfsnobody /nfs
sudo chmod 755 /nfs
```

Next, we will edit the `/etc/exports` file to add our **share** as an export point/directory. This should be structured as follows:

```bash
/SHARE HOST(rw,sync,no_root_squash)
```

Each directory line should contain:

* `/SHARE` - Your above configured **share**
* `HOST` - List of clients allowed to access **share** - hostnames, IP address, networks, wildcard “*”, etc

Options per client:

* `rw` - filesystem access permissions
* `sync` - reply to client that data was stored
* `no_root_squash` - do not map `root` or other UID/GID to anonymous

Multiple clients per share might look like this:

`/SHARE HOST0(option0,option1,...) HOST1(...)`

Our command will look like this:

```bash
sudo echo "/nfs *(rw,sync,no_root_squash)" >> /etc/exports
```

**Note:** If you were to change the configuration after starting `nfs-server`, you can reload with the below command:

```bash
sudo exportfs -r
```

### Step 3: Configuring the Firewall

Ensure the firewall allows NFS services by running these commands:

```bash
sudo firewall-cmd --permanent --add-service=nfs
sudo firewall-cmd --permanent --add-service=mountd
sudo firewall-cmd --permanent --add-service=rpc-bind
sudo firewall-cmd --reload
```

### Step 4: Starting the NFS Services

Finally, we can start the NFS services:

```bash
sudo systemctl enable nfs-server
sudo systemctl start nfs-server
```

### Finalize

Your NFS server is now up and running, ready to serve files to any client within the network! It's worth noting that it's always a good idea to ensure that all the machines within the network have their clocks synchronized. You can use NTP for this.

## NFS Client Configuration

After configuring the NFS server, we need to set up the NFS client. The following steps explain how to mount the NFS share on a RHEL client system.

### Step 1: Installing NFS Packages

Install the required NFS packages on the client machine:

```bash
sudo yum install -y nfs-utils
```

### Step 2: Mounting the NFS Share

Create a directory to mount the NFS share:

```bash
sudo mkdir /mnt/nfs
```

Mount the NFS share on the client:

```bash
sudo mount -t nfs <server-ip>:/nfs /mnt/nfs
```

Check if the NFS share is mounted successfully:

```bash
df -h
```

## Conclusion

This tutorial guided you through the process of deploying and configuring an NFS server on an XFS filesystem, in this case hosted on a DAS attached to a **Dell R730** running RHEL. It also walked you through setting up an NFS client. With this knowledge, you should be well-equipped to set up your own NFS environment and manage your shared directories and files over a network with ease. Remember, the keys to an effective NFS server setup lie in a well-prepared DAS, correctly configured shares, a correctly configured firewall, and services started in the right order.

## References

1. [mkfs.xfs(8): construct XFS filesystem - Linux man page](https://linux.die.net/man/8/mkfs.xfs)
2. [NFS Server (Network File System)](http://chschneider.eu/linux/server/nfs.shtml)
3. [Chapter 2. Exporting NFS shares RHEL 9](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/configuring_and_using_network_file_services/exporting-nfs-shares_configuring-and-using-network-file-services)
4. [Supermicro - CSE847](https://www.supermicro.com/en/products/chassis/4u/847/sc847be1c12-r1k68lpb4)
