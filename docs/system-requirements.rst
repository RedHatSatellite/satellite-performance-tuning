===================
System Requirements
===================

For details of Red Hat Satellite 6 hardware and software requirements, please take a look at `Preparing your environment for installation <https://access.redhat.com/documentation/en-us/red_hat_satellite/6.9/html/installing_satellite_server_from_a_connected_network/preparing-environment-for-satellite-installation#system-requirements_satellite>`_, inside the installation guide.

Quick Tuning Guide
==================

Users who wish to tune their Satellite based on expected managed host counts and hardware allocation can utilize the built in tuning profiles included in Satellite 6.9 and later that are available via the installation routine's new tuning flag (see `information in installation guide <https://github.com/RedHatSatellite/satellite-support/tree/master/tuning-profiles>`_)::

  # satellite-installer --help
  Usage:
      satellite-installer [OPTIONS]

  Options:
  [....]
        --tuning INSTALLATION_SIZE  Tune for an installation size. Choices: default, medium, large, extra-large, extra-extra-large (default: "default")


There are 4 sizes provided based on estimates of the number of managed hosts your Satellite will be hosting.

+-------------------+-------------------------+---------------+-----------------+
| Name              | Number of managed hosts | Recommend RAM | Recommend Cores |
+===================+=========================+===============+=================+
| default           | 0-5000                  | 20G           | 4               |
+-------------------+-------------------------+---------------+-----------------+
| medium            | 5000-10000              | 32G           | 8               |
+-------------------+-------------------------+---------------+-----------------+
| large             | 10000-20000             | 64G           | 16              |
+-------------------+-------------------------+---------------+-----------------+
| extra-large       | 20000-60000             | 128G          | 32              |
+-------------------+-------------------------+---------------+-----------------+
| extra-extra-large | 60000+                  | 256G+         | 48+             |
+-------------------+-------------------------+---------------+-----------------+

Instructions for use:

1. Determine the profile you wish to use
2. Run `satellite-installer --tuning large`. This will apply to the chosen tuning profile.
3. The Ruby app server will need to be tuned directly via the Puma Tuning section: :ref:`puma_tuning`.
4. Resume operations

NOTE: The specific tuning settings for each profile can be viewed in the configuration files contained in `/usr/share/foreman-installer/config/foreman.hiera/tuning/sizes`
