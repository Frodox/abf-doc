---
title: ABF Package build environment
---

# ABF Package build environment

This describes the resources that make up the official Rosa ABF Package build environment. If you have any problems or requests please contact support.

**Note: This Documentation is in a beta state. Breaking changes may occur.**

* [Assembly of packages](/abf/scripts/assembly_of_packages/)
* [Publication of packages](/abf/scripts/publication_of_packages/)

Different scripts uses for each type of platform. If You would like to create new script, we recommend to review next projects:

* [RHEL scripts](https://abf.rosalinux.ru/abf/rhel-scripts)
* [MDV scripts](https://abf.rosalinux.ru/abf/mdv-scripts)

It contains scripts for `MDV` and `RHEL` platforms.

We use [Vagrant](http://docs-v1.vagrantup.com/v1/docs/boxes.html) for manage Virtual Machines on [VirtualBox](https://www.virtualbox.org/). This VMs use for building/publishing packages, creating ISO. You can find boxes which uses on [ABF](https://abf.rosalinux.ru/abf/abf-worker/blob/master/config/vm.yml.sample)