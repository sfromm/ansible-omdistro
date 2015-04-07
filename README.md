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

There are no external dependencies.

Role Variables
--------------

Because there are several moving parts to configuring a NMS solution,
there are a number of variables to control the behavior of **OMD** and
**Check_MK**.

- **omdistro_use_inventory**: Whether to use Ansible inventory when
  populating list of devices to monitor.
- **omdistro_use_ssl**: Whether to force SSL when connecting to the
  frontend via Apache.  Will copy a simple Apache configuration to
  redirect non-SSL connections to SSL.  Defaults to `no`, but you are
  encouraged to set to `yes`.
- **omdistro_notification_logging**: Instruct **Check_MK** to log
  notifications.  See **Check_MK**
  [documentation](https://mathias-kettner.de/checkmk_flexible_notifications.html).
  Defaults to `1` (ie. yes).
- **omdistro_snmp_default_community**: Default SNMP community string to use.
- **omdistro_no_snmpbulk_tag**:  Default is `nobulk`.  When this tag is
  associated with a host, **Check_MK** will not use *snmpbulk* queries.
- **omdistro_scan_parents**: Whether to scan devices to determine
  parent/child relationships.  Default is `no`.
- **omdistro_site**:  The *omdistro* site name.  Defaults to `local`.
- **omdistro_site_config**:  A list of dictionaries that configures the
  **OMD** site.  The best way to look up configuration options is to run
  `omd config <sitename> show`.  The default is:
```
    - { name: MULTISITE_COOKIE_AUTH, value: "on" }
    - { name: PNP4NAGIOS,            value: "on" }
    - { name: MKEVENTD,              value: "on" }
```
- **omdistro_mkeventd**: Whether to enable **Check_MK** *mkeventd*.
  Default is `yes`.  For further information, see
  [Check_MK Event Console](https://mathias-kettner.de/checkmk_mkeventd.html).
- **omdistro_mkeventd_syslog**:  Whether to use *mkeventd*'s builtin
  syslog daemon.  Default is `yes`.  If this is set to no *and*
  **omdistro_mkeventd** is `yes`, this will enable forwarding of
  messages from *rsyslog* to *mkeventd*.  See also:
  [Setting up the Event Console](https://mathias-kettner.de/checkmk_mkeventd_setup.html).
- **omdistro_version**: Version of **OMD** to install.  Defaults to
  `1.30`.
- **omdistro_contacts**: A list of dictionaries that describe *contact*
  and *user* entries for **Check_MK**.  Each entry should at minimum
  define:
  - `alias`: The contact's full name.
  - `contactgroups`: A list of /contactgroups/ the user is associated with.
  - `email`: The email address of the contact.
  - `name`:  The username of the contact.
- **omdistro_contactgroups**: A list of *contactgroups*.
- **omdistro_hostgroups**: A list of *hostgroups* a host can be a member
  of.  These are a subset of the tags you would apply to a host.
- **omdistro_servicegroups**: A list of *servicegroups* a service can be
  a member of.
- **omdistro_iftypes**: A list of /ifTypes/ that **Check_MK** will use
  for discovery of inventory.  You must use the /ifType/ integer.
- **omdistro_ignored_checktypes**: A list of checks to ignore during the
  inventory process.
- **omdistro_ignored_services**: A dictionary of services to ignore.
  The key is a tag associated with a host.  The value is a service-name
  description.
- **omdistro_inventory_check**: A dictionary with two valid keys:
  - `interval`: How often to perform an inventory check.  The value is
    in minutes.  The default is `1440` (one day).
  - `severity`: The severity level when new services are discovered.
    The default is `1`, a warning.

For more information regarding *netmon*, please see:
[netmon.org](netmon.org).  Extensions to *netmon* are the following:

* **parent**:  The name of the parent device.  The parent *MUST* also be
  listed in the monitored devices.  The parent can be defined as a
  string or as a list.  Here are two examples:

```yaml
- name: dc1-gw.example.net
  groups: [ core, dc ]
- name: dc2-gw.example.net
  groups: [ core, dc ]
- name: node-gw.example.net
  groups: [ core, dc ]
- name: node1.example.net
  groups: [ noc ]
  ext:
    omdistro:
      parent:  node-gw.example.net
- name: node2.example.net
  groups: [ noc ]
  ext:
    omdistro:
      parent:
        - dc1-gw.example.net
        - dc2-gw.example.net
```

Dependencies
------------

There are no dependencies on other roles.

Templates
---------

Instead of creating a single /main.mk/ file to configure and manage
**Check_MK**, this roles creates several files:

- *etc/check_mk/main.mk*:  Defines 'general' configuration pieces, such
as `custom_checks`, `checkgroup_parameters`, `ignored_services`, and so
on.
- *etc/check_mk/conf.d/hosts.mk*: Defines hosts to discover, the tags
associated with each host, and parent/child relationships if
**omdistro_scan_parents** is `no`.
- *etc/check_mk/conf.d/groups.mk*: Defines *hostgroups*,
*servicegroups*, and *contactgroups*.
- *etc/check_mk/conf.d/wato/contacts.mk*: Defines contacts to associate
with hosts and services.  This is only created if the file does not
exist.  This allows further adjustment via the **WATO** interface.
- *etc/check_mk/multisite.d/wato/users.mk*: Defines users allowed to log
in to **Multisite**, the web interface, and the level of access to
grant.  Similar to *contacts.mk*, this only creates the file if it does
not already exist.

Example Playbook
----------------

A brief example:

```yaml
    - hosts: servers
      roles:
         - {
              role: sfromm.omdistro,
              omdistro_version: 1.20
              omdistro_site: local
           }
```
           
License
-------

GPLv2

Author Information
------------------

See https://github.com/sfromm
