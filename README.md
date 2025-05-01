
# Born2beroot

![42](https://img.shields.io/badge/-42-black?style=for-the-badge&logo=42&logoColor=white)

<imagem_aqui>

![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black) ![Debian](https://img.shields.io/badge/Debian-D70A53?style=for-the-badge&logo=debian&logoColor=white) ![Git](https://img.shields.io/badge/git-%23F05033.svg?style=for-the-badge&logo=git&logoColor=white) ![Markdown](https://img.shields.io/badge/markdown-%23000000.svg?style=for-the-badge&logo=markdown&logoColor=white)

**Born2beroot** is one of the Milestone 1 projects at [42 Porto](https://www.42porto.com). The main goal of this project is to build a Virtual Machine from scratch. We use `VirtualBox` as the virtualization software (more on that later), and `Debian` as the operating system. Throughout the project, we learned key concepts such as `virtualization`, `Linux`, `UFW`, `SSH`, `Shell scripting`, `partitions`, and more. We also dealt with lower-level topics involving system administration (I swear, it was fun!). Additionally, in the `bonus` section, we could build the foundations needed to have a `WordPress` website, defined `LVMGroup` partitions, and install an additional protocol, in our case, `FTP`.

---

*As a student, always be curious. Curiosity is essential in every field, and tech is no exception. Don't just follow guidelines and expect to understand things deeply! Experiment, fail, try again, fail once more, and then, you will learn something new. This guide will be another tool in your studies, but remember, the key is to stay curious!*

---

# Content

- [Summary](#Summary)
- [Theory](#Theory)
- [What is a Virtual Machine](##What-is-a-Virtual-Machine)
- [How does a Virtual Machine works](##How-does-a-Virtual-Machine-works)
- [What are the purposes of Virtual Machines](##What-are-the-purposes-of-Virtual-Machines)
- [What is Rocky/Debian](##-What-is-Rocky/Debian)
- [Services](#Services)
- [UFW](##UFW)
- [SSH](##SSH)
- [AppArmor](##AppArmor)
- [apt and aptitude](##apt-and-aptitude)
- [Sudo](##Sudo)
- [Monitoring.sh](#Monitoring.sh)
- [Bonus](#Bonus)
- [Wordpress statck](##Wordpress-stack)
- [Partitions](##Partitions)
- [FTP](##FTP)
- [Commands](#Commands)
- [Feedback](#Feedback)
- [Contact](#Contact)

---

# Summary

**Born2beroot** is a foundational system administration project from the 42 Porto curriculum, focused on Linux system hardening and automation. The objective is to provision and configure a Debian-based Virtual Machine using `VirtualBox`, applying best practices in system security and user privilege management.

Core implementations include:

- Creation of a non-root user with sudo privileges and strong password policies.
- Configuration of `UFW` (Uncomplicated Firewall) for basic firewall rules.
- Enabling and securing remote access via `SSH`.
- Automated system setup using `bash` scripts.
- Disk partitioning using `LVM` (Logical Volume Manager), with structured logical volumes for `/` (root), `/boot`, `/home`, `/tmp`, `/usr`, `/var`, `/srv`, `/usr/local`.

Bonus tasks involved:

- Deploying a `WordPress` stack (`Lighttpd` + `PHP` + `MariaDB`) on the VM.
- Understanding better the Virtual Machine partitioning.
- Installing and configuring an additional protocol (`vsftpd` for `FTP` in our case).

This project emphasizes **reproducibility**, **security**, and foundational **DevOps** skills using bare system tools.

---

# Theory

## What is a Virtual Machine

A **Virtual Machine (VM)** is essentially an **Operating System (OS)** running inside your **Personal Computer (PC)**. It uses your computer’s hardware resources to run the OS of the virtual system you created.
To achieve this, we use a layer called a **Hypervisor** (such as **VirtualBox**, in our case).

A *hypervisor* is a software that enables us to run a virtual system *independently* of the host's *physical hardware*.
In other words, a **VM** is an **OS** running inside your computer — it can be the **same OS** as your host machine, or a **different** one entirely! Isn't it interesting?!

![Wikipedia scheme](https://github.com/user-attachments/assets/c4ce62a8-7a02-4e8b-abce-502fdcbf7075)

![Watch this video of IBM explaining what a VM is!](https://www.ibm.com/think/topics/virtual-machines#)

## How does a Virtual Machine works

### Host Machine

It's our physical computer, our **hardware**. Here we provide the hardware resources, such as **CPU**, **RAM**, **disk**, etc. On top of this structure we have our main **Operating System**, or the **host OS** (it could be Windows, Mac, Linux).

### Hypervisor

It's a software layer that allows us to virtualize a machine, **managing and allocating the hardware resources** we destinated to our VM. We have basically two types of it:
- **Type 1**: the **bare-metal** one, running directly on our hardware (VMware ESXi);
- **Type 2**: the **hosted**, running on top of a host OS (**VirtualBox**, VMware Workstation).

### Guest OS

It's the **OS** running **inside** the Virtual Machine! As we stated before, it could be the same **OS** as your **host OS** (you have a *Windows* PC and run a *Windows* VM) or a different one (you have a *Linux Ubuntu* PC and run a *MacOS* VM). This is good to understand one of the main *advantages* of a VM: it's **fully isolated** from the **host OS**, running as if it's on a *different* PC.

### Virtual Hardware

For doing what we said above, the **hypervisor** emulates components like:
- Virtual **CPU** (vCPU);
- Virtual **RAM**;
- Virtual **disk**;
- Network interface card (**NIC**).

These apper to the **host OS** as a running *software*, and for the **guest OS** as a real *hardware* (that's magic, buddy).

### Storage and Snapshots

Virtual Machines use *disk image files* (.vdi, .vmdk) as virtual drives, the same ones you run `sha1sum <yourvm.vdi>` and get the `signature.txt` content. You can take **snapshots** and save the current state of a VM, and roll back if needed too.

### Resource Sharing

The VM shares the physical hardware of the host, I think that's quite clear, right? But the *resources are limited* by what the *hypervisor allocates*, even though you can modify it later. Plus, *multiple VMs* can run **at the same time** on the same **host PC**. 

![Microsoft scheme](https://github.com/user-attachments/assets/8ef529ba-8c0b-4f99-8a93-403efab54caa)


## What are the purposes of Virtual Machines

I'll list below a few usabilities of VMs:
- **Testing and Development**: you can test different OS for a software you're developing, in a safe envorionment, on the same PC. Ideal for **cross-platform compatibility tests**;
- **Sandboxing and Isolation**: you can run potentially unsafe applications in a *secure environment* (one example is *Kali Linux* and other cybersecurity softwares), preventing **malwares** and **misconfigurations** from affecting the host OS;
- **Server Virtualization**: same thing with **servers**, we can run different servers on a single physical server, reducing *hardware costs* and increasing *efficiency* in data centers;
- **Learning**: what we are doing here! We can have access to different systems and do ~~with a very high probability~~ things that could crash something, lol.
- **Recovery**: very easy to recover something in case of a *system failure*, easy to migrate to other machines, and creating *backups*.
- **Cloud Computing and Scalability**: VMs are the backbone of cloud services (*AWS*, *Azure*, *GCP*), being able to create, destroy, and scale dynamically based on demand.

## What is Rocky/Debian
### Red Hat Distro
### Debian Distro

---

# Services

## UFW

## SSH

## AppArmor

## apt and aptitude

## Sudo

---

# Monitoring.sh

---

# Bonus

## Wordpress stack

## Partitions

## FTP

---

# Commands

---

# Feedback

![image](https://github.com/user-attachments/assets/954c0dc7-cd7f-4c47-bb24-33e78d47249b)

---

# Contact

If you have any question or suggestion feel free to contact me!

- E-mail: ghrb2811@gmail.com

- LinkedIn: [gabrielhdb](https://www.linkedin.com/in/gabrielhdb/)

---

Made with 🫀 by me!

---
