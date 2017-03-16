---
layout: page
title: "The Shell"
comments: true
date: 2017-03-15
---

# Introduction to cloud computing

## Learning Objectives
* Understand benefits of working on a remote computer system
* Be able to connect to a cloud instance vis `SSH`
* Check the available resources and file system on your remote machine
* Keep background processes working in the cloud with `tmux`

There are a number of reasons why accessing a remote machine is invaluable to any scientists working with large datasets. In the early history of computing, working on a remote machine was standard practice - computers were bulky and expensive. Today we work on laptops that are more powerful than the sum of the world's computing capacity 20 years ago, but many analyses (especially in genomics) won't work on these laptops and must be run on remote machines.

You'll know you need to start working on the cloud when...

* Your computer does not have enough resources to run the desired analysis (memory, processors, disk space, network bandwidth).
* Your computer is taking hours or days to get through an analysis.
* You cannot install software on your computer (application does not have support for your operating system, conflicts with other existing applications, etc.)

The cloud is a part of our everyday life (e.g. using Amazon, Google, Netflix, or an ATM involves remote computing). The topic is fascinating but **this lesson says '5 minutes or less'** so let's get connected.


### The Shell
---


The `shell` is a program that presents a command line interface which allows you to control your computer using commands entered with a keyboard instead of controlling graphical user interfaces (GUIs) with a mouse/keyboard combination. **The `shell` is an interpreter that helps translate our input into computer language.**

There are many reasons to learn about the `shell`.

* For most bioinformatics tools, you have to use the `shell`. There is no graphical interface. If you want to work in metagenomics or genomics you're going to need to use the `shell`.
* The `shell` gives you **power**. The command line gives you the power to do your work more efficiently and more quickly.  When you need to do things tens to hundreds of times, knowing how to use the `shell` is transformative.
* Computational resources that can handle large datasets, such as remote computers or cloud computing, require a working knowledge of Unix.

We will be using the shell on our laptops to connect to the cloud. **How do we access the `shell`?**

## Exercises

---

## Setting up

For the duration of this workshop, we will be accessing data and tools required to analyze our data on a remote computer using the **Amazon's Elastic Compute Cloud (EC2)**.

### Connecting to a remote computer using Amazon EC2

This is the first and last place in these lessons where it will matter if you are using PC, Mac, or Linux. After we connect, we will all be on the same operating system/computing environment.

> To save time, your instructor will have launched an remote computer (instance) for you prior to the workshop. **Each instance will have an associated IP adresss listed in the etherpad which you will need to connect to the instance**. If you are following these lessons on your own, or after the workshop see the lesson on [cloud computing](https://github.com/datacarpentry/cloud-genomics/tree/gh-pages/lessons) for instructions on how to do this yourself.

**User Credentials** are case sensitive:

- Username: dcuser
- Password: data4Carp


#### **Windows users**

*Prerequisites*: You must have an SSH client. There are several free options and we will use PuTTY [[Download Putty.exe](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)]

For Windows, you will have downloaded a separate program called PuTTy to allow you to use the shell.

1. Open `PuTTY`; In the 'Host Name (or IP address)' section paste in the IP address provided by your instructor (or the ip address of an instance you have provisioned yourself). *Keep the default selection 'SSH' and Port (22)*. <br>
![Putty Image](../img/putty_screenshot_1.png)
1. Click 'Open' and you will be presented with a security warning. Select 'Yes' to continue to connect. <br>
![Putty security screen](../img/putty_screenshot_2.png)
1. In the final step, you will be asked to provide a login and password. **Note:** When typing your password, it is common in Unix/Linux not see see any asterisks (e.g. ****) or moving cursors. Just continue typing.<br>
![Putty login](../img/putty_screenshot_3.png)
1. You should now be connected!


#### **Mac/Linux users**

*Prerequisites*: The shell is already available on Mac and Linux computers.

On Linux search for `Terminal`

On Mac the shell is available through Terminal:
	`Applications -> Utilities -> Terminal`

Or click on the magnifying glass in the upper right of the Desktop and search for 'terminal' and/or look for the `terminal` icon.<br>
![terminal icon](../img/terminal.png)

  1. Open the terminal and type the following command substituting 'ip_address' for the IP address your instructor will provide. *Be sure to pay attention to capitalization and spaces*

  ```
  $ ssh dcuser@ip_address
  ```

  1. You will receive a security message that looks something like the message below. Type 'yes' to proceed.

  ```
  The authenticity of host 'ec2-52-91-14-206.compute-1.amazonaws.com (52.91.14.206)' can't be established. ECDSA key fingerprint is SHA256:S2mMV8mCThjJHm0sUmK2iOE5DBqs8HiJr6pL3x/XxkI. Are you sure you want to continue connecting (yes/no)?
  ```

  1. In the final step, you will be asked to provide a login and password. **Note:** When typing your password, it is common in Unix/Linux not see see any asterisks (e.g. ****) or moving cursors. Just continue typing.
  1. You should now be connected!

### **Verifying your connection and environment**

When you connect, it is typical to receive a welcome screen. The Data Carpentry Amazon instances display this message upon connecting:


```
Welcome to Ubuntu 14.04.3 LTS (GNU/Linux 3.13.0-48-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Sun Jan 24 21:38:35 UTC 2016

  System load:  0.0                Processes:           151
  Usage of /:   48.4% of 98.30GB   Users logged in:     0
  Memory usage: 6%                 IP address for eth0: 172.31.62.209
  Swap usage:   0%

  Graph this data and manage this system at:
    https://landscape.canonical.com/

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

12 packages can be updated.
10 updates are security updates.


Last login: Sun Jan 24 21:38:36 2016 from
```

You should also have a blinking cursor awaiting your command:

```
dcuser@ip-172-31-62-209 ~ $
```

## Bonus material
---

Now that we have connected we can move on to the Unix shell lesson. There are however a few commands that tell you a little about the machine you have connected to:

  * `whoami` - shows your username on computer you have connected to

```
dcuser@ip-172-31-62-209 ~ $ whoami
dcuser
```

  * `df -f` - shows space on hard drive (Under the column 'Mounted on' row that has `/` as the value shows the value for the main disk)

```
dcuser@ip-172-31-62-209 ~ $ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            2.0G   12K  2.0G   1% /dev
tmpfs           396M  792K  395M   1% /run
/dev/xvda1       99G   48G   47G  51% /
none            4.0K     0  4.0K   0% /sys/fs/cgroup
none            5.0M     0  5.0M   0% /run/lock
none            2.0G  144K  2.0G   1% /run/shm
none            100M   36K  100M   1% /run/user

-h option = 'human readable output'
```

  * `cat /proc/cpuinfo` - shows detail information on how many processors (CPUs) the machine has

```
dcuser@ip-172-31-62-209 ~ $ cat /proc/cpuinfo
processor  : 0
vendor_id	: GenuineIntel
cpu family	: 6
model		: 62
model name	: Intel(R) Xeon(R) CPU E5-2670 v2 @ 2.50GHz
stepping	: 4
microcode	: 0x415
cpu MHz		: 2494.060
cache size	: 25600 KB
physical id	: 0
siblings	: 2
core id		: 0
cpu cores	: 2
apicid		: 0
initial apicid	: 0
fpu		: yes
fpu_exception	: yes
cpuid level	: 13
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology eagerfpu pni pclmulqdq ssse3 cx16 pcid sse4_1 sse4_2 x2apic popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm xsaveopt fsgsbase smep erms
bogomips	: 4988.12
clflush size	: 64
cache_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:

processor	: 1
vendor_id	: GenuineIntel
cpu family	: 6
model		: 62
model name	: Intel(R) Xeon(R) CPU E5-2670 v2 @ 2.50GHz
stepping	: 4
microcode	: 0x415
cpu MHz		: 2494.060
cache size	: 25600 KB
physical id	: 0
siblings	: 2
core id		: 1
cpu cores	: 2
apicid		: 2
initial apicid	: 2
fpu		: yes
fpu_exception	: yes
cpuid level	: 13
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology eagerfpu pni pclmulqdq ssse3 cx16 pcid sse4_1 sse4_2 x2apic popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm xsaveopt fsgsbase smep erms
bogomips	: 4988.12
clflush size	: 64
cache_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
```

  * `tree -L 1` - shows a tree view of the file system 1 level below your current location.


```
dcuser@ip-172-31-62-209 ~ $ tree -L 1

├── dc_sample_data
├── Desktop
├── Downloads
├── FastQC
├── openrefine-2.6-beta.1
├── R
└── Trimmomatic-0.32

7 directories, 0 files

# -L option = 'Level'
# -L 1 option = One level deep
```

## Staying connected to the cloud

---

Depending on how you connect to the cloud, you may have processes and jobs that are running, and will need to continue running for sometime. If you are connecting via `SSH` and you end/lose the `SSH` connection your jobs will terminate.

There are a few ways to keep cloud processes running in the background. Many times when we refer to a background process we are talking about what is described at this tutorial - running a command and returning to shell prompt. Here we describe a program that will allow us to run our entire shell and keep that process running even if we disconnect: `tmux`.

`tmux` stands for terminal multiplexer

### Starting and attaching to `tmux` sessions

#### Starting a new session

A 'session' can be thought of as a window for `tmux`, you might open an terminal to do one thing on the a computer and then open a new terminal to work on another task at the command line. You can start a session and give it a descriptive name:

```
$ tmux new -s session_name

# option -s = 'session name'
```

This creates a session with the name 'session_name'

As you work, this session will stay active until you close this session. Even if you disconnect from your machine, the jobs you start in this session will run till completion.

#### Seeing active sessions

If you disconnect from your session, or from your ssh into a machine, you will need to reconnect to an existing tmux session. You can see a list of existing sessions:

```
$ tmux list-sessions
```

#### Connecting to a session

To connect to an existing session:

```
$ tmux attach -t session_name
# -t option = 'target'
```

#### Switch sessions

You can switch between sessions:

```
tmux switch -t session_name
```

#### Kill a session

You can end sessions:

```
tmux kill-session -t session_name
```

### Resources on cloud computing
* [Amazon EC2](http://aws.amazon.com/ec2/)
* [Microsoft Azure](https://azure.microsoft.com/en-us/)
* [Google Cloud Platform](https://cloud.google.com/)
* [CyVerse Atmosphere](http://www.cyverse.org/atmosphere)

Learn more about cloud computing in bioinformatics
Fusaro VA, Patil P, Gafni E, Wall DP, Tonellato PJ (2011) Biomedical Cloud Computing With Amazon Web Services. [PLoS Comput Biol 7(8): e1002147](http://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1002147)


### Resources on the shell
* [FOSSwire cheat sheet](http://fosswire.com/post/2007/08/unixlinux-command-cheat-sheet/)
* [Software Carpentry cheat sheet](https://github.com/swcarpentry/boot-camps/blob/master/shell/shell_cheatsheet.md)
* [Explain shell](http://explainshell.com )
* [Command line fu](http://www.commandlinefu.com)
