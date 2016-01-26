---
layout: article
title: Introduction To BaseMount
---

##Overview
The main mechanism to interact with your BaseSpace data is via the website at [basespace.illumina.com](https://basespace.illumina.com). However, for some use-cases, it can be useful to work with the same data using the Linux command line interface (CLI). This allows direct ad-hoc programmatic access so that users can write ad-hoc scripts and use tools like find, xargs and command line loops to work with their data in bulk.

This is the concept behind our first major BaseSpace command line tool, BaseMount, a [FUSE driver](https://en.wikipedia.org/wiki/Filesystem_in_Userspace "FUSE userspace driver") that allows command line read access to your [BaseSpace](https://basespace.illumina.com "BaseSpace") data.

##What is BaseMount?
[BaseMount](https://basemount.basespace.illumina.com "BaseMount") is a tool to mount your BaseSpace data as a Linux file system. You can navigate through projects, samples, runs and app results and interact directly with the associated files exactly as you would with any other local file system. BaseMount is based on [FUSE](https://en.wikipedia.org/wiki/Filesystem_in_Userspace "FUSE userspace driver") and uses the [BaseSpace API](https://developer.basespace.illumina.com "BaseSpace API") to populate the contents of each directory.

##Tutorial Videos

{% step 1, , Install %} <iframe width="560" height="315" src="https://www.youtube.com/embed/jmsdDmHV-aQ" frameborder="0" allowfullscreen></iframe> {% endstep %}

{% step 2, , Import Data to Account %} <iframe width="560" height="315" src="https://www.youtube.com/embed/R8ou0yLE_Ts" frameborder="0" allowfullscreen></iframe> {% endstep %}

{% step 3, , First Time Launch %} <iframe width="560" height="315" src="https://www.youtube.com/embed/VPMFKhNRYjU" frameborder="0" allowfullscreen></iframe> {% endstep %}

{% step 4, , Projects, Runs, Samples %} <iframe width="560" height="315" src="https://www.youtube.com/embed/afP7SogrcQs" frameborder="0" allowfullscreen></iframe> {% endstep %}

{% step 5, ,  AppSessions and AppResults %} <iframe width="560" height="315" src="https://www.youtube.com/embed/FDRQLN5etCM" frameborder="0" allowfullscreen></iframe> {% endstep %}

{% step 6, ,  Access Large Files %} <iframe width="560" height="315" src="https://www.youtube.com/embed/zCkRIKW3rZU" frameborder="0" allowfullscreen></iframe> {% endstep %}

{% step 7, ,  Metadata %} <iframe width="560" height="315" src="https://www.youtube.com/embed/xknDqArvRf8" frameborder="0" allowfullscreen></iframe> {% endstep %}

##BaseMount Installation

Installation requires root access. Non-root CentOS users need to be added to the 'fuse' user group by an administrator before being able to run [BaseMount](https://basemount.basespace.illumina.com "BaseMount").

To run [BaseMount](https://basemount.basespace.illumina.com "BaseMount") inside a docker container, the container must run in privileged mode.

If you are behind a proxy, as BaseMount is sending all HTTPS requests to libcurl, you may need to configure it using the "https_proxy" environment variable (if this doesn't work, please refer to libcurl's manual).


### Quick install:
Run the following command:

    sudo bash -c "$(curl -L https://basemount.basespace.illumina.com/install/)"
This script works on both Ubuntu and CentOS. It adds BaseSpace package repositories, import public keys to your system and installs [BaseMount](https://basemount.basespace.illumina.com "BaseMount") and its prerequisites.

###Manual install

See FAQ
##Minimum Hardware Requirements and System-Level Settings
We have tested BaseMount *Alpha* v0.11.0 on the following systems:

Supported Operating Systems:

- Ubuntu 14 & 15
- CentOS 6.5 & 7


Minimum Hardware requirements:

- RAM: 4GB (check your `ulimit -v and -m`)
- Disk: 5GB /tmp


Recommended ulimit thresholds:

- Open files: tested with `ulimit -n 1024`, should be greater or equal to this value
- Stack size: tested with `ulimit -s 8192`, should be lower or equal to this value
- Max user processes: tested with `ulimit -u 16384`, should be greater or equal to this value
- Virtual memory size: tested with `ulimit -v unlimited`, should be greater or equal to 4GB
- Max memory size: tested with `ulimit -m unlimited`, should be greater or equal to 4GB
- Max locked memory: tested with `ulimit -l 64`, should be greater or equal to this value


##Mounting Your BaseSpace Account
After a successful installation, you can mount your [BaseSpace](https://basespace.illumina.com "BaseSpace") account with [BaseMount](https://basemount.basespace.illumina.com "BaseMount"). Basic usage is as follows:

    basemount [--config <config name>] <mount-point folder>

where the --config parameter is optional, and useful when a user has multiple basespace accounts to be mounted.
The config name is used to create a <config name>.cfg file in the ~/.basespace/ directory.
The mount point directory becomes the top level folder in your mounted file tree.

Example:

    mkdir ~/BaseSpace_Mount
    basemount --config user1 ~/BaseSpace_Mount

##Mounting Your BaseSpace Onsite Account
BaseMount can also be used to mount user account data for BaseSpace Onsite. This is supported starting with BaseSpace Onsite version 2.1.
In order to mount your account data, you must use an additional command-line parameter: --api-server. This will direct BaseMount to connect via the API URL of the BaseSpace Onsite system.
Basic usage for BaseSpace Onsite is as follows:

    basemount [--config <config name>] --api-server <API URL> <mount-point folder>

where the API URL follows the format `http://{BaseSpace-IP-Address}:8080` for BaseSpace Onsite.

##Authentication
The first time you run [BaseMount](https://basemount.basespace.illumina.com "BaseMount"), you will be directed to a web URL and asked to enter your BaseSpace user credentials. BaseMount will use these credentials to authenticate your interactions with BaseSpace. By default, the credentials are cached in your home directory and they can be password-encrypted for security, just like an ssh key.

##Directory Structure
BaseMount uses the natural structure of the BaseSpace API to create an appropriate directory hierarchy, summarized below.

###Overview
BaseMount mimics the structure of the major resources represented in the API.  The root of this structure starts with the Href links that you would find in the API call: *[https://api.basespace.ilumina.com/v1pre3/users/current](https://api.basespace.illumina.com/v1pre3/users/current)*:

![API Users/Current](/images/articles/api-users-current.png)

###Root
Runs and Projects are two root directories, each containing sub-directories with the names of the Runs/Projects to which the user has access.

![Root](/images/articles/tree-root.png)

###Project Details
Inside each project directory you would find the following sub-directories:

![Project Detail](/images/articles/tree-project.png)

- **AppSessions**, known as "Analyses" on the BaseSpace website.  This directory will list all AppSessions that you started in this project or have written to this project. Sub-directories contain the AppResults and/or Samples that the app created. Multi-node appsessions will also contain child appsession sub-directories.
- **AppResults** directory contains all of the result data available in the project.  These are flattened out list, not broken down by analysis.  
- **Samples** directory contains the flattened list of Samples that are in the project and will have a Files directory containing your fastq.gz files.

###MetaData
In each directory, BaseMount provides a number of hidden files with extra BaseSpace metadata. These hidden files follow the Unix convention of starting with a "." and can be seen using the command:
```
ls -a
```

These metadata files are:

-  **.json**: the result of the raw BaseSpace API query. This includes metadata associated with the relevant BaseSpace entity. For example a sample's metadata includes the number and length of reads, the number of reads passing filter, etc.
-  **.curl**: the raw API request for this entity

###Access By Entity Id
In each directory, BaseMount provides hidden symlinks to the human-readable named entity folders, with the **.id** prefix. These allow navigation through a users directories by entity ID's rather than names. 

###File-level Access
At various appropriate levels, BaseMount provides access to the raw files available in your BaseSpace account. For example, the fastq.gz files associated with a sample. 

File accesses use direct HTTPS connections to Amazon storage (i.e. they don't go through the BaseSpace API) to expose files at the block level so users can access any part of a file without needing to download all the preceding blocks, such as when using a tabix or bam index. Caching makes repeated accesses to the same file more efficient.


##Downloading Data
You can use BaseMount to download your BaseSpace data to a local filesystem. Just use **cp**, **rsync** or any command line tool to copy the files from your BaseMount space to your chosen destination.

BaseMount automatically uses multiple threads even for single file accesses. Copying multiple files in parallel may be slower or faster than one-at-a-time file copy: it varies based on your system and on the way files were stored on Amazon servers.

Although BaseMount does facilitate file download, and since BaseMount allows for convenient, fast, cached access to your BaseSpace metadata and files, you may find that many operations can be carried out without the need to download entire files locally. During our testing, we have used BaseMount to grep through FASTQ files, extracted blocks of reads from BAM files and even used IGV on the bam files directly all without downloading files locally. This can be more convenient than including a download step and saves on the overheads of local storage.

## BaseSpaceFS (BSFS) plugin
The BSFS plugin is a high performance FUSE-based plugin for BaseMount.

This plugin can be activated as follows:

	basemount --plugin=bsfs <mountpoint>
The BSFS plugin is targeted at EC2 instances and cases when you want to run apps on the mounted BaseSpace data. 
The main advantage of BSFS plugin is that all the downloaded data is kept in a disk-based LRU (Least Recently Used) cache. 
Native apps currently running on BaseSpace uses BSFS with a disk-buffer size of 1TB.
Without the BSFS plugin, caching is handled by the Linux disk cache, using (and restricted to) any available RAM.

### Differences in behavior when using the BSFS plugin
* A new mount point /tmp/BsfsMount/<mount point> will be created (the /tmp location can be configured).
* The "Files" directories containing your actual files are symlinks to these folders in /tmp. If, for example, you wish to tar the files, you will have to use the "h" option to follow the symlinks.
* BSFS plugin does not support proxies

### BSFS options
To properly use the BSFS plugin, the following values have to be set in the configuration file under [BSFS] tag. All the configuration options are case-sensitive.

### Cache Mode
There are currently three caching methods implemented in BSFS as described below:

**SparseFile** (default): This mode is used in EC2 instances, where the cache path is on an XFS file system. If this mode is specified and the cache path is not on XFS, then BlockFile mode will be used automatically. Ejection of blocks from cache will use the hole-punching feature of XFS.

	cacheMode = SparseFile

**BlockFile**: This mode can be used with any file system. In this mode, each file-block is represented as a separate file in the cache folder. If lot of blocks are being accessed, then the number of open file limit (ulimit -n) of the OS will be reached. Therefore it is recommended to increase this value to more than 20,000.

	cacheMode = BlockFile

**RamBuffer**: In this mode, the cached data is kept in RAM, and not on disk. You should ensure the cache size limit fits in your available RAM, and adjust your block size to have enough blocks if you plan to access multiple files in parallel.

	cacheMode = RamBuffer

### Prefetch Strategy

**NextBlock** (default): In addition to the block(s) containing the current request the "next block" in sequence is also fetched. In our production EC2 instances, this mode is used.

	prefetchStrategy = NextBlock

**OnDemand**: Only the block(s) containing the current read request is fetched, i.e. pre-fetching of blocks are not done.

	prefetchStrategy = OnDemand

### BlockSize
This represents the maximum size (in bytes) of a file block in BSFS's cache. The value you set gets rounded up to a multiple of the filesystem's block size. In our production EC2, block size of 5242880 (5MB) is used. 

### CacheFolder
This is the location on disk (not needed for RamBuffer mode) where the parts/blocks of the accessed files are cached. In our production EC2, this is on a 1TB XFS file system.

	cacheFolder = /tmp/cache

### CacheSizeLimit
This represents the maximum size (in bytes) of the disk (or RAM in case of RamBuffer mode) which can be used for buffering the data. At runtime, if the cache size limit is reached, then some (least recently used) file blocks are purged to make space for the new incoming blocks. Ensure that the disk where you specify the cache folder location has enough free space. In our production EC2, CacheSizeLimit of 1TB is used.

	cacheSizeLimit = 10737418240

### NumberOfBlocksToPurgeOnCacheFull 
This represents the number of file-blocks to purge when the LRU cache becomes full. In our production EC2, this value is set to 20.

	numberOfBlocksToPurgeOnCacheFull = 20

### Example configuration of BSFS plugin
To properly use the BSFS plugin, add the following lines to the end of the default config file in /home/user/.basemount/default.cfg
	
    [BSFS]
	cacheMode = BlockFile
	prefetchStrategy = NextBlock
	cacheFolder = /tmp/bsfs
	cacheSizeLimit = 10737418240
	blockSize = 4194304
	numberOfBlocksToPurgeOnCacheFull = 20
##Limitations of BaseMount *Alpha* v0.11.0
Each new directory access made by BaseMount relies on the local FUSE device (/dev/fuse), the BaseSpace API and the user's credentials. This mechanism means that, as currently available, BaseMount does not support the following types of accesses:

- Cluster access, where many compute nodes can access the files. FUSE mounted file systems are per-host and cannot be accessed from many hosts without additional infrastructure.
- BaseMount also doesn't refresh files or directories. In order to reflect changes done via the Web GUI in your command line tree, you currently need to unmount (basemount --unmount <mountpoint>) and restart BaseMount.
- In general, lots of concurrent requests can cause stability issues on a resource-constrained system.  Keep in mind, this is an early release and stability will increase.
- If you have terabytes of data in BaseSpace, doing a "find" command or recursive "ls" or recursive "grep" on the mount is not recommended: it is likely to start consuming many GB of RAM and crash when the RAM runs out. Yes, we'll handle this better soon!

##ChangeLog

### Mon Jan 26 2016 - v0.11.1 Alpha
- Proxy support
- "Files" directories are not symlinks anymore
- Run Files are automatically mounted, now that they load more interactively
- .basemount directories for BaseSpaceCLI support
- BSFS available as a plugin

### Fri Jul 24 2015 - v0.1.0 Alpha
- Initial release


## Feedback

Please visit our [BaseSpace google group](https://groups.google.com/forum/#!forum/basespace-developers) for inquiries and to submit feedback. 

## Questions and Answers

### How can I refresh the contents of BaseMount's directories?

Feature coming soon.
For the moment, the only way is to unmount and remount basemount.


### How can I install a previous version of Basemount?
The install script always installs the latest version of BaseMount. If you want to lock in a specific version as part of your system setup scripts, please use the following steps in your install script.

Please uninstall all existing versions of BaseMount and BSFS first, and then install using the following commands:

####v0.1 Alpha:

#####Ubuntu:

	wget https://bintray.com/artifact/download/basespace/BaseMount-DEB/basemount_0.1.2.463-20150714_amd64.deb
	wget https://bintray.com/artifact/download/basespace/BaseSpaceFS-DEB/bsfs_1.1.631-1_amd64.deb
	sudo dpkg -i basemount_0.1.2.463-20150714_amd64.deb
	sudo dpkg -i --force-confmiss bsfs_1.1.631-1_amd64.deb
	sudo apt-get -f install # Install any potentially missing dependency

#####CentOS

	wget https://bintray.com/artifact/download/basespace/BaseMount-RPM/basemount-0.1.2.464-20150714.x86_64.rpm
	wget https://bintray.com/artifact/download/basespace/BaseSpaceFS-RPM/bsfs-1.1.632-1.x86_64.rpm
	sudo yum install ./basemount-0.1.2.464-20150714.x86_64.rpm
	sudo yum install ./bsfs-1.1.632-1.x86_64.rpm

####v0.11 Alpha:

#####Ubuntu:

	wget https://bintray.com/artifact/download/basespace/BaseMount-DEB/basemount_0.11.1.1342-20160126_amd64.debs
	sudo dpkg -i basemount_0.11.1.1342-20160126_amd64.deb
	sudo apt-get -f install # Install any potentially missing dependency

	# Optional BSFS plugin
	wget https://bintray.com/artifact/download/basespace/BaseSpaceFS-DEB/bsfs_1.3.771-1_amd64.deb
	sudo dpkg -i --force-confmiss bsfs_1.3.771-1_amd64.deb
	sudo apt-get -f install # Install any potentially missing dependency

#####CentOS

	wget https://bintray.com/artifact/download/basespace/BaseMount-RPM/basemount-0.11.1.1343-20160126.x86_64.rpm
	sudo yum install ./basemount-0.11.1.1343-20160126.x86_64.rpm

	# Optional BSFS plugin
	wget https://bintray.com/artifact/download/basespace/BaseSpaceFS-RPM/bsfs-1.3.772-1.x86_64.rpm
	sudo yum install ./bsfs-1.3.772-1.x86_64.rpm
