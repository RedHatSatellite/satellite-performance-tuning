============================================
Configuring your environment for Performance
============================================

CPU
===

The more physical cores that are available to Satellite 6.5, the higher throughput can be achieved for the tasks. Some of the Satellite components such as Puppet, MongoDB, PostgreSQL are CPU intensive applications and can really benefit from the higher number of available CPU cores.

Memory
======

The higher amount of memory available in the system running Satellite, the better will be the response times for the Satellite operations. Since Satellite uses PostgreSQL and MongoDB as the database solutions, any additional memory coupled with the tunings will provide a boost to the response times of the applications due to increased data retention in the memory.

Disk
====

With Satellite doing heavy IOPS due to repository synchronizations, package data retrieval, high frequency database updates for the subscription records of the content hosts, it is advised that Satellite be installed on a high speed SSD drive so as to avoid performance bottlenecks which may happen due to increased Disk reads or writes. Satellite 6 requires disk IO to be at or above 60-80 megabytes per second of average throughput for read operations. Anything below this value can have severe implications for the operation of the Satellite.

Benchmark disk performance
--------------------------
We are working to update foreman-maintain to only warn the users when its internal quick 'fio' benchmark results in numbers below our recommended throughput but will not require a whitelist parameter to continue.

Also working on an updated benchmark script you can run (which will likely be integrated into foreman-maintain in the future) to get a more accurate real-world storage information. 

Note:

- One may have to temporarily reduce the RAM in order to run the io benchmark, aka if the box has 256GB that is a lot of pulp space, so add mem=20G kernel option in grub. This is needed because script will execute a series of fio based IO tests against a targeted directory specified in its execution. This test will create a very large file that is double (2x) the size of the physical RAM on this system to ensure that we are not just testing the caching at the OS level of the storage.
- Please bear above in mind when performing benchmark of other filesystems if you have them (like PostgreSQL or MongoDB storage) which might have significantly smaller capacity than Pulp storage and perhaps on different set of storage (SAN, iSCSI, etc).

This test does not use directio and will utilize the OS + caching as normal operations would.

You can find our first version of the script `storage-benchmark <https://github.com/RedHatSatellite/satellite-support/blob/master/storage-benchmark>`_. To execute just download to your Satellite, `chmod +x` the script and run::

    # ./storage-benchmark /var/lib/pulp
    This test creates a test file that is double (2X) the size of this system's
    RAM in GB. This benchmark will create a test file of size: 

    64 Gigabytes

    in the: [/var/lib/pulp/storage-benchmark] location. This is to ensure that the test utilizes
    a combination of cached and non-cached data during the test.

    **** WARNING! Please verify you have enough free space to create a 64 GB
    file in this location before proceeding. 

    Do you wish to proceed? (Y/N) Y


    Starting IO tests via the 'fio' command. This may take up to an hour or more
    depending on the size of the files being tested. Be patient!

    ************* Running READ test via fio:

    read-test: (g=0): rw=read, bs=(R) 4096B-4096B, (W) 4096B-4096B, (T) 4096B-4096B, ioengine=psync, iodepth=1
    fio-3.1
    Starting 1 process
    …

As noted in the README block in the script: generally you wish to see on average 100MB/sec or higher in the tests below:

- Local SSD based storage should values of 600MB/sec or higher.
- Spinning disks should see values in the range of 100-200MB/sec or higher.

If you see values below this, please open a support ticket for assistance.

Refer this `blog <https://access.redhat.com/solutions/3397771>`_ for more detailed info. 

Network
=======

The communication between the Satellite and Capsules is impacted by the network performance. A decent network with a minimum jitter and low latency is required to enable hassle free operations such as Satellite and Capsule synchronization (at least make sure it is not causing connection resets, etc).

Server Power Management
=======================
Your server by default is likely to be configured to conserve power. While this is a good approach to keep the max power consumption in check, it also has a side effect of lowering the performance that Satellite may be able to achieve. For a server running Satellite, it is recommended to set the BIOS to enable the system to be run in performance mode to boost the maximum performance levels that Satellite can achieve.
