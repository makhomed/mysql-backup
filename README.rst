========
mysql-backup
========

MySQL backup tool

Warning! This tool can be used to backup only transactional databases, like InnoDB.

Installation
------------

- ``cd /opt``
- ``git clone https://github.com/makhomed/mysql-backup.git``


Upgrade
-------

- ``cd /opt/mysql-backup``
- ``git pull``


Usage
-----

.. code-block:: none

    # mkdir /srv/mysql-backup
    # /opt/mysql-backup/mysql-backup


Automation via cron
-------------------

.. code-block:: none

    # cat /etc/cron.d/mysql-backup
    0 0 * * * root /opt/mysql-backup/mysql-backup

