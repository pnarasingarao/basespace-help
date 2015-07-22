---
layout: article
title: Introduction To BaseMount
---

##Overview
The main mechanism to interact with your BaseSpace data is via the website at [www.basespace.com](https://basespace.illumina.com). However, for some use-cases, it can be useful to work with the same data using the Linux command line interface (CLI). This allows direct ad-hoc programmatic access so that users can write ad-hoc scripts and use tools like find, xargs and command line loops to work with their data in bulk.

This is the concept behind our first major BaseSpace command line tool, BaseMount, a [FUSE](https://en.wikipedia.org/wiki/Filesystem_in_Userspace "FUSE driver") that allows command line read access to your [BaseSpace](https://basespace.illumina.com "BaseSpace") data.

##What is BaseMount
[BaseMount](https://basemount.basespace.illumina.com "BaseMount") is a tool to mount your BaseSpace data as a Linux file system. You can navigate through projects, samples, runs and app results and interact directly with the associated files exactly as you would with any other local file system. BaseMount is a [FUSE](https://en.wikipedia.org/wiki/Filesystem_in_Userspace "FUSE driver"), which operates in user-space and uses the [BaseSpace API](https://developer.basespace.illumina.com "BaseSpace API") to populate the contents of each directory.

##Authentication
The first time you run [BaseMount](https://basemount.basespace.illumina.com "BaseMount"), you will be directed to a web URL and asked to enter your BaseSpace user credentials. BaseMount will use these credentials to authenticate your interactions with BaseSpace. By default, the credentials are cached in your home directory and they can be password-encrypted for security, just like an ssh key.

##Directory Structure
BaseMount uses the natural structure of the BaseSpace API to create an appropriate directory hierarchy, which is summarized below:

###Overview
BaseMount mimics the structure of the major resources represented in the API.  The root of this structure starts with the Href links that you would find in the API call: *[https://api.basespace.ilumina.com/v1pre3/users/current](https://api.basespace.illumina.com/v1pre3/users/current)*:

![API Users/Current](/images/articles/api-users-current.png)

###Root
Runs and Projects are the two root directories, inside each is just a list of the runs or projects that you have access too.

![Root](/images/articles/tree-root.png)

###Project Detail
Inside a specific project you would find the following directories:

![Project Detail](/images/articles/tree-project.png)

- **AppSessions**, known as "Analyses" in the BaseSpace website.  This directory will list all AppSessions that you started in this project or have written to this project.  A single directory will contain the AppResults and/or Samples that the app created.  
- **AppResults** directory contains all of the result data available in the project.  These are flattened out list, not broken down by analysis.  
- **Samples** directory contains the flattened list of Samples that are in the project and will have a Files directory containing your fastq.gz files.

###MetaData
In each directory, BaseMount provides a number of hidden files with extra BaseSpace metadata. These hidden files follow the Unix convention of starting with a . and can be seen with the following:
```
ls -a
```

These metadata files are:

-  **.json**: the result of the raw BaseSpace API query. This includes metadata associated with the relevant BaseSpace entity, so for example a sample's metadata includes the number and length of reads, the number of reads passing filter etc.
-  **.type**: the BaseSpace entity type for this directory
-  **.id**: The BaseSpace ID for this entity

###File-level Access
At various appropriate levels, BaseMount provides access to the raw files available in your BaseSpace account. For example, the fastq.gz files associated with a sample. 

This file access is provided by BSFS, the mechanism used by the BaseSpace native app engine to expose BaseSpace files for app runs. BSFS uses HTTPS to expose files at the block level so users can make an access into the middle of a file without needing to download all the proceeding blocks, such as when using a tabix or bam index. BSFS also provides caching features to make repeated access of the same file more efficient.

{% callout note, NOTE %}
The Alpha release of BaseMount creates many BSFS instances, one for each Files directory.  Depending on the type of access patterns and resources available having lots of active mounts can cause stability issues.  This is a known issue and will be resolved in future releases.
{% endcallout%}

##Downloading Data
You can use BaseMount to download your BaseSpace data to a local filesystem. Just use **cp**, **rsync** or any command line tool you prefer to copy the files from your BaseMount space to your chosen destination. 

Although BaseMount does facilitate file download, we would recommend that since BaseMount allows convenient, fast, cached access to your BaseSpace metadata and files, you may find that many operations can be carried out without the need to download locally. During our testing, we have used BaseMount to grep through fastq files, extract blocks of reads from bam files and even use IGV on the bam files directly all without downloading files locally. This can be more convenient than including a download step and saves on the overheads of local storage.

##Limitations of BaseMount *Alpha*
Every new directory access made by BaseMount relies on FUSE, the BaseSpace API and the user's credentials. This mechanism means that, as currently available, BaseMount does not support the following types of access:



- Shared file access, where multiple users can read the same files
- Cluster access, where many compute nodes can access the files. FUSE mounted file systems are per-host and cannot be accessed from many hosts without additional infrastructure.
- BaseMount also doesn't refresh files or directories. In order to reflect changes done via the Web GUI in your command line tree, you currently need to unmount (basemount --unmount <mountpoint>) and restart BaseMount.
- The Runs Files directory is not mounted automatically for you as there can be 100k + files available in that mount which can take a couple minutes to load for really large runs.  You can still mount this directory manually if needed.
- In general, lots of concurrent requests can cause stability issues on a resource constrained system.  Keep in mind, this is an early release and stability will increase.

##Minimum Hardware Requirements and System-Level Settings
Tested on the following operating systems:


- Ubuntu 14, CentOS 6.5
- Minimum 4GB RAM
- Minimum 5GB /tmp

With the following ulimit thresholds:



- The "maximum number of user processes" (set by `ulimit -u`) must be 1024 or above.
- The "maximum number of open file descriptors" (set by `ulimit -n`) must be 1024 or above.
- The "maximum stack size" (set by `ulimit -s`) may be 'unlimited' or less (tested with "unlimited", which is our worst case).


##Tutorial Videos

{% step 1, , Install %}
						<iframe width="560" height="315" src="https://www.youtube.com/embed/jmsdDmHV-aQ" frameborder="0" allowfullscreen></iframe>
{% endstep %}	

{% step 2, , Import Data to Account %}
						<iframe width="560" height="315" src="https://www.youtube.com/embed/R8ou0yLE_Ts" frameborder="0" allowfullscreen></iframe>
{% endstep %}

{% step 3, , First Time Launch %}

<iframe width="560" height="315" src="https://www.youtube.com/embed/VPMFKhNRYjU" frameborder="0" allowfullscreen></iframe>

{% endstep %}

{% step 4, , Projects, Runs, Samples %}

<iframe width="560" height="315" src="https://www.youtube.com/embed/VPMFKhNRYjU" frameborder="0" allowfullscreen></iframe>

{% endstep %}


{% step 5, ,  AppSessions and AppResults %}

<iframe width="560" height="315" src="https://www.youtube.com/embed/VPMFKhNRYjU" frameborder="0" allowfullscreen></iframe>

{% endstep %}

{% step 6, ,  Access Large Files %}

<iframe width="560" height="315" src="https://www.youtube.com/embed/VPMFKhNRYjU" frameborder="0" allowfullscreen></iframe>

{% endstep %}

{% step 7, ,  Metadata %}

<iframe width="560" height="315" src="https://www.youtube.com/embed/xknDqArvRf8" frameborder="0" allowfullscreen></iframe>

{% endstep %}
						
						
			