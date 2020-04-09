===================
System Requirements
===================

For details of Red Hat Satellite 6 hardware and software requirements, please take a look at Preparing your environment for installation, inside the installation guide.

Using custom-hiera.yaml
=======================

Whenever you need to permanently change the default configuration of some of Satellite’s services, prefer changing satellite-installer configuration above changing services configuration files, because these changes would be overwritten on next satellite-installer run (e.g. because of upgrade). More details on how to do that properly can be found in Applying Custom Configuration to Red Hat Satellite 6 appendix of Installer  `Guide <https://access.redhat.com/documentation/en-us/red_hat_satellite/6.7-beta/html/installing_satellite_server_from_a_connected_network/applying_custom_configuration_to_red_hat_satellite>`_.

In short, you need to edit `/etc/foreman-installer/custom-hiera.yaml` file and then run `satellite-installer` command. Note that you have to first test changes in non-production environment.

Quick Tuning Guide
==================

Users who wish to tune their Satellite based on expected managed host counts and hardware allocation can utilize the `custom-heira.yaml` tuning files we provide publicly on the Red Hat Satellite GitHub repository here:
https://github.com/RedHatSatellite/satellite-support/tree/master/tuning-profiles

There are 4 custom sizes provided based on estimates of the number of managed hosts your Satellite will be hosting.

+----------+-------------------------+---------------+-----------------+
| Name     | Number of managed hosts | Recommend RAM | Recommend Cores |
+==========+=========================+===============+=================+
| DEFAULT  | 0-5000                  | 20G           | 4               |
+----------+-------------------------+---------------+-----------------+
| MEDIUM   | 5000-10000              | 32G           | 8               |
+----------+-------------------------+---------------+-----------------+
| LARGE    | 10000-20000             | 64G           | 16              |
+----------+-------------------------+---------------+-----------------+
| X-LARGE  | 20000-60000             | 128G          | 32              |
+----------+-------------------------+---------------+-----------------+
| 2X-LARGE | 60000+                  | 256G+         | 48+             |
+----------+-------------------------+---------------+-----------------+

Instructions for use:

1. Backup your previous `/etc/foreman-installer/custom-hiera.yaml` to `custom-hiera.original`.
2. Copy file that matches RAM on your Satellite to `/etc/foreman-installer/custom-hiera.yaml`.
3. If you had settings previously added in `custom-hiera.original` that do not exist in the copied `custom-hiera.yaml`, you may need to re-add them to the copied file.
4. Take care to preserve values added by the upgrade process such as::

    # Added by foreman-installer during upgrade, run the installer with --upgrade-mongo-storage to upgrade to WiredTiger.
    mongodb::server::storage_engine: 'mmapv1'

5. Run `satellite-installer` with no arguments to apply these settings.
