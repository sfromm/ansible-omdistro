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
[netmon.org](netmon.org).  Extensions to *netmon* are the following:

* **parent**:  The name of the parent device.  The parent *MUST* also be
  listed in the monitored devices.  The parent can be defined as a
  string or as a list.  Here are two examples:

```yaml
- name: dc1-gw.example.net
- name: dc2-gw.example.net
- name: node-gw.example.net
- name: node1.example.net
  ext:
    omdistro:
      parent:  node-gw.example.net
- name: node2.example.net
  ext:
    omdistro:
      parent:
        - dc1-gw.example.net
        - dc2-gw.example.net
```


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
