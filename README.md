omdistro and check_mk
=====================

Ansible role to manage [omdistro](http://omdistro.org) and
[check_mk](https://mathias-kettner.de/check_mk.html).

For more information on **OMD**, **Check_MK**, and **Nagios**, please refer to
the following:

- [OMD](http://omdistro.org/doc)
- [Check_MK](https://mathias-kettner.de/checkmk.html)
- [Nagios Object Definitions](http://nagios.sourceforge.net/docs/3_0/objectdefinitions.html)

Requirements
------------

NA

Role Variables
--------------

TODO

For more information regarding *netmon*, please see:
[netmon.org](netmon.org).

Dependencies
------------

NA

Example Playbook
----------------

A brief example:

    - hosts: servers
      roles:
         - {
              role: sfromm.omdistro,
              omdistro_version: 1.20
              omdistro_site: local
           }

License
-------

GPLv2

Author Information
------------------

See https://github.com/sfromm
