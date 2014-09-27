onmetal-fast-docker
===================

This is an effort to create cloud-config.yml files that utilize some of the
more performant parts of OnMetal flavors for CoreOS.

Usage
-----
To use these, simply append `--user-data /path/to/cloud-config.yml` to your 
nova boot command in order to pass in your cloud-config, via configdrive, to
CoreOS.

onmetal-io1
-----------

`onmetal-io1/cloud-config.yml` configures the two LSI Warpdrive Nytro PCIe
flash cards in a Linux MD RAID 0, with BTRFS filesystem, and mounts them at
/var/lib/docker before the docker daemon is allowed to start. This configures
your server so any images downloaded and containers run go onto the fast PCIe
storage.

Example:

This example assumes you already have python-novaclient configured and pointed
at a Rackspace Cloud datacenter that provides OnMetal (as of this writing;
IAD-only)

Note: Please ensure you use the most recent CoreOS image ID as shown by
`nova image-list` on your account. Example is only the most recent image as of
this writing.

`nova boot --flavor onmetal-io1 --image 85e3d8d2-6d0f-4ded-bd55-0f13d3e57e69 
--key-name my-key-name --user-data ./onmetal-io1/cloud-config.yml mytestio1`

onmetal-memory1, onmetal-compute1
---------------------------------

For onmetal-memory1 and onmetal-compute1 nodes I would like to implement a
similar backing change for docker, but instead have it run inside a ramdisk
of some kind (tmpfs? ramfs? other?) but have not implemented it yet.
