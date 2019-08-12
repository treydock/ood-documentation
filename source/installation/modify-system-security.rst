.. _modify-system-security:

Modify System Security
======================

.. _ood_selinux:

SELinux
--------

.. DANGER::
   Support for SELinux on the Open OnDemand host is currently considered an alpha feature.

#. If you plan to use `SELinux`_ on the Open OnDemand host you must install the `ondemand-selinux` package.

   .. code-block:: sh

      sudo yum install ondemand-selinux

   .. note::

      OnDemand runs under the Apache `httpd_t` context.

The OnDemand SELinux package makes several changes to allow OnDemand to run with SELinux enabled.

* Set context of several ondemand-nginx directories and files. `See ondemand-selinux.fc <https://github.com/OSC/ondemand/blob/master/packaging/ondemand-selinux.fc>`_
* Enable several booleans. `See "%post selinux" in ondemand.spec <https://github.com/OSC/ondemand/blob/master/packaging/ondemand.spec#L245>`_
* Apply a custom policy to allow some additional actions by processes in the `httpd_t` context. See `ondemand-selinux.te <https://github.com/OSC/ondemand/blob/master/packaging/ondemand-selinux.te>`_ and `ondemand-selinux-systemd.te <https://github.com/OSC/ondemand/blob/master/packaging/ondemand-selinux-systemd.te>`_

If you experience denials when running SELinux with Open OnDemand please provide denial details by generating a `ood.te` file and posting that to `Discourse <https://discourse.osc.edu/c/open-ondemand>`_. It would also help to post the `audit.log` lines that correspond to the OnDemand specific denials.

   .. code-block:: sh

      cat /var/log/audit/audit.log | audit2allow -M ood

The OnDemand SELinux package does not package policy changes for batch systems as those will be site specific.
If you experience denials and wish to write a custom policy these are the basic steps:

   .. code-block:: sh

      cat /var/log/audit/audit.log | audit2allow -M mypolicy
      semodule -i mypolicy.pp

.. _firewall:

Firewall
---------
#. Open ports 80 (http) and 443 (https) in the firewall, typically done with
   `iptables`_.

   .. warning::

      If using **RHEL 7** you will need to either use the newer `firewalld`_
      daemon and modify the firewall settings or disable it and install
      `iptables`_.

.. _selinux: https://wiki.centos.org/HowTos/SELinux
.. _iptables: https://wiki.centos.org/HowTos/Network/IPTables
.. _firewalld: https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/security_guide/sec-using_firewalls
