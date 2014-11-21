omdistro and check_mk
=====================

Ansible role to manage [omdistro](http://omdistro.org) and
[check_mk](https://mathias-kettner.de/check_mk.html).

Requirements
------------

NA

Role Variables
--------------

TODO

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
