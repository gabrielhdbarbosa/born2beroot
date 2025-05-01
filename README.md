
# Born2beroot

![image](https://github.com/user-attachments/assets/f79f0cf8-cddb-42e4-96ff-64d95a6a9578)

![42](https://img.shields.io/badge/-42-black?style=for-the-badge&logo=42&logoColor=white)

![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black) ![Debian](https://img.shields.io/badge/Debian-D70A53?style=for-the-badge&logo=debian&logoColor=white) ![Git](https://img.shields.io/badge/git-%23F05033.svg?style=for-the-badge&logo=git&logoColor=white) ![Markdown](https://img.shields.io/badge/markdown-%23000000.svg?style=for-the-badge&logo=markdown&logoColor=white)

**Born2beroot** is one of the Milestone 1 projects at [42 Porto](https://www.42porto.com). The main goal of this project is to build a Virtual Machine from scratch. We use `VirtualBox` as the virtualization software (more on that later), and `Debian` as the operating system. Throughout the project, we learned key concepts such as `virtualization`, `Linux`, `UFW`, `SSH`, `Shell scripting`, `partitions`, and more. We also dealt with lower-level topics involving system administration (I swear, it was fun!). Additionally, in the `bonus` section, we could build the foundations needed to have a `WordPress` website, defined `LVMGroup` partitions, and install an additional protocol, in our case, `FTP`.

---

*As a student, always be curious. Curiosity is essential in every field, and tech is no exception. Don't just follow guidelines and expect to understand things deeply! Experiment, fail, try again, fail once more, and then, you will learn something new. This guide will be another tool in your studies, but remember, the key is to stay curious!*

---

# Content

- [Summary](#Summary)
- [Theory](#Theory)
  - [What is a Virtual Machine](#What-is-a-Virtual-Machine)
  - [How does a Virtual Machine works](#How-does-a-Virtual-Machine-works)
  - [What are the purposes of Virtual Machines](#What-are-the-purposes-of-Virtual-Machines)
  - [What is Rocky/Debian](#-What-is-Rocky/Debian)
- [Services](#Services)
  - [UFW](#UFW)
  - [SSH](#SSH)
  - [AppArmor](#AppArmor)
  - [apt and aptitude](#apt-and-aptitude)
  - [Sudo](#Sudo)
- [Monitoring.sh](#Monitoring.sh)
  - [Crontab](#Crontab)
- [Bonus](#Bonus)
  - [Wordpress statck](#Wordpress-stack)
  - [Partitions](#Partitions)
  - [FTP](#FTP)
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

I used a few git repos, guidelines, etc. But two of them were very importanto for me: [mcombeau's](https://github.com/mcombeau/Born2beroot/tree/main) and [PedroZappa's](https://github.com/PedroZappa/42_Born2beRoot). Please, check these ones, awesome projects!!

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

**Rocky Linux** and **Debian** are two popular *Linux distributions* (**distros**), each belonging to a different family of the Linux "ecosystem". They serve as foundations for many servers, cloud environments, and development machines.

### Red Hat Distro

![image](https://github.com/user-attachments/assets/a4ecf95a-e100-4ef5-b62b-8fc37f4c257d)

Rocky Linux is part of the **Red Hat family**, and it's designed to be a *100% bug-for-bug* **compatible** replacement for **Red Hat Enterprise Linux (RHEL)**. It was created after **CentOS** shifted focus to CentOS Stream.

Ideal for **enterprise environments** and **production servers**. Focus on **stable** production environments, organizations migrating from **CentOS**.

### Debian Distro

![image](https://github.com/user-attachments/assets/1cce5873-4cd1-4890-949f-1b97e831dd20)

Debian is a **community-driven** distro and the base for many others, including **Ubuntu**. It's known for its **stability**, **security**, and **strict open-source policies**.

A huuuge software repository **maintained by volunteers**. Different from Rocky Linux, it's ideal for **general-purpose servers and desktops**, uses *apt* and *aptitude* package managers, and it's base for customized distros like **Ubuntu** and **Kali Linux**.

---

# Services

In this section we'll discuss the services we'll use in the **Debian** distro in this project, since it's the distro I chose to work with. If you decided to go with Rocky Linux, [check this repository](https://github.com/Edu2metros/Guia-Rocky---Born2BeRoot-42) in Portuguese.

## UFW

The **Uncomplicated Firewall** is a simplified interface used to *manage the Linux firewall*. It's easy to **allow** or **deny** ports, rules and connections.

You can `ufw allow enable`, `ufw allow 4242`, and `ufw status` for basic manipulation.

Check [this](https://wiki.debian.org/Uncomplicated%20Firewall%20%28ufw%29) for more info.

## SSH

This is a **safety protocol**, largely used to remotely access another PC through the command line. It's a **cryptografed connection**, authenticated through **password** or **public key**.

You can `ssh <user>@<localhost/ip address> -p <portal>` to create this connection.

Check [here](https://www.cloudflare.com/learning/access-management/what-is-ssh/) for more info.

## AppArmor

**AppArmor** is a system of *obligatory control access* (**MAC**) for Linux, and it restricts what a program/file/folder can or cannot do. It's like a security guard allowing (or not) a file to work in your system.

You can `aa-status` to check if it's working.

Check [here](https://apparmor.net/) for more info.

## apt and aptitude

They are both **package management tools** for Debian distros.

- `apt` is a modern and simple package instaler, updater and remover.
- `aptitude` is a little bit more robust, has a grafic interface via terminal.

![image](https://github.com/user-attachments/assets/649f8d15-3681-457c-a8b7-cdab0d3fbfe3)

You can ~~and should~~ always `sudo apt update` && `sudo apt upgrade`.

## Sudo

*Superuser Do*, or commonly know as **sudo** allows users to execute commands with **administrador** priviledges. The good part is that it avoids the constant use of commands with `root` users. It also registers the commands done in `/var/log/sudo/sudo.log`.

![image](https://github.com/user-attachments/assets/68afaa8c-2a10-4e22-a6c8-a69234192b9b)

You can visualize the sudo configurations in `/etc/sudoers`, or just `sudo visudo`.

---

# Monitoring.sh

The [monitoring.sh](https://github.com/mcombeau/Born2beroot/blob/main/monitoring.sh) script is a file that will hold a list of commands that will share your **PC status** at the reboot (`@reboot`) and from 10 to 10 minutes after that.

![image](https://github.com/user-attachments/assets/941550e5-275d-4039-b0d1-962f86341166)

It's great to learn a few commands that will show you how a PC works. A few examples are:

- Architecture    : `uname`
- CPUs            : `/proc/cpuinfo`
- Memory Usage    : `free -h`
- Disk Usage      : `df -h`
- CPU Load        : `top -bn1`
- Last Boot       : `who -b`
- LVM use         : `lsblk`
- TCP Connections : `/proc/net/sockstat`
- Users logged    : `who`
- Network         : `hostname -I` && `ip link show`
- Sudo            : `/var/log/sudo/sudo.log`

Check and test these commands in your terminal! You should try and understand what these commands are (be curious).

## Crontab

Cron is a **time-based job scheduler** used to run commands or scripts automatically at specified intervals in your Linux environment. It allows you to schedule tasks to run hourly, daily, weekly, or at custom times. During my peer evaluations and conversations with others, I noticed **at least three different ways** to approach and configure cron jobs.

- You can **create a sleep.sh** file and call it with the `monitoring.sh` in the `/etc/crontab` file;
- You can also edit the `/etc/crontab` and sleep 600 seconds to call `monitoring.sh`;
- Or like me: call `montoring.sh` only `@reboot` and `*/10 * * * *`. It's easier to understand what's going on and easier to modify the `/etc/crontab` infos.

![image](https://github.com/user-attachments/assets/3a7f99a8-cd44-4d66-b25a-87819c015781)

---

# Bonus

Let's get this project a little bit deeper...

## Wordpress stack

The **WordPress stack** is a collection of software needed to run WordPress on a server. Here we use:

- **Lightpd**   = Web server to handle HTTP requests;
- **MariaDB**   = Database to store WordPress content and configuration;
- **PHP**       = Server-side language that powers WordPress logic;
- **Wordpress** = The CMS itself.

After installing and setting these services, through your local web server you will be able to go to `http://localhost:8080` and see a full **Wordpress** configuration website!

## Partitions

**Disk partitions** divide your physical disk into logical segments. This helps **organize** the system, **improve performance**, and **enhance security**.

    /	       = Root filesystem – Contains the entire system. All paths start here.
    /boot    = Boot loader and kernel – Holds GRUB, kernel images, and initrd.
    /home    = User files and settings – Each user gets a subdirectory here.
    swap     = Virtual memory – Used when RAM is full; acts as overflow space.
    /tmp     = Temporary files – Used by apps for short-lived files; often cleared on reboot.
    /srv     = Service data – Holds data for services like web or FTP (/srv/www).
    /var     = Variable data – Logs, databases, mail spool, print queues, etc.
    /var/log = System logs – Stores logs from system and apps (/var/log/syslog).

Not all these paths need to be **separate partitions**, but isolating some (like /home, /var, /tmp, and swap) can improve **Security**, **Performance**, and **Maintenance**.

## FTP

**FTP** is a protocol used to transfer files between computers over a network. By standard we use the **Port 21** for that. We run a `sudo apt install vsftpd` as a server daemon to run the installation. Just FYI the FTP is **not a safe** protocol, since it's **not encrypted**. For better use cases, **SFTP (via SSH)** is preferred over FTP due to its security.

---

# Commands

Below I'll list a few commands that it's going to be useful in this project. Figure out **how** and **where** you should use them:

- `systemctl status <program>`;
- `useradd` && `adduser`;
- `gpasswd -a` && `gpasswd -d`;
- `reboot`;
- `chage`;
- `id` && `id -u`;
- `groups` && `getent group` && `id -g` && `groupadd`.

There are tons of bash commands that you can use. If you see usability for them go for it!

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
