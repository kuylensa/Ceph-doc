.. Ceph-doc documentation master file, created by
   sphinx-quickstart on Mon Feb  3 11:34:34 2014.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Installatie van Ceph
====================

Contents:

.. toctree::
   :maxdepth: 2


Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

Inleiding
=============
De installatie van Ceph terug overlopen naar aanleiding van Ceph-opleiding van `42on.com`_ (gegeven door Wido den Hollander) op 28 en 29 janurari 2014 te Breda.

.. _42on.com: http://www.42on.com/training/

Wat is Ceph?
------------
Ceph is an open-source, massively scalable, software-defined storage system which provides object, block and file system storage in a single platform. It runs on commodity hardware—saving you costs, giving you flexibility—and because it’s in the Linux kernel, it’s easy to consume.

Enkele begrippen
----------------
OSDs
^^^^
A Ceph OSD Daemon (OSD) stores data, handles data replication, recovery, backfilling, rebalancing, and provides some monitoring information to Ceph Monitors by checking other Ceph OSD Daemons for a heartbeat. A Ceph Storage Cluster requires at least two Ceph OSD Daemons to achieve an active + clean state when the cluster makes two copies of your data (Ceph makes 2 copies by default, but you can adjust it).

Monitors
^^^^^^^^
A Ceph Monitor maintains maps of the cluster state, including the monitor map, the OSD map, the Placement Group (PG) map, and the CRUSH map. Ceph maintains a history (called an “epoch”) of each state change in the Ceph Monitors, Ceph OSD Daemons, and PGs.

MDSs
^^^^
A Ceph Metadata Server (MDS) stores metadata on behalf of the Ceph Filesystem (i.e., Ceph Block Devices and Ceph Object Storage do not use MDS). Ceph Metadata Servers make it feasible for POSIX file system users to execute basic commands like ls, find, etc. without placing an enormous burden on the Ceph Storage Cluster.


Ceph stores a client’s data as objects within storage pools. Using the CRUSH algorithm, Ceph calculates which placement group should contain the object, and further calculates which Ceph OSD Daemon should store the placement group. The CRUSH algorithm enables the Ceph Storage Cluster to scale, rebalance, and recover dynamically.

Opstelling VM's
---------------
* 42on1: client?
* 42on2: client?
* 42on3: Monitor + OSD's
* 42on4: Monitor + OSD's
* 42on5: Monitor + OSD's
* 42on6: OSD's (wordt later toegevoegd aan cluster om deze uit te breiden)
* 42on7: OSD's (wordt later toegevoegd aan cluster om deze uit te breiden)
* 42on8: OSD's (wordt later toegevoegd aan cluster om deze uit te breiden)


Configuratie VM's
-----------------
42on1 en 42on2:
 * 25GB disk met Basic Ubuntu 12.04.03 LTS geïnstalleerd

42on3 - 42on8:
 * 25GB disk met Basic Ubuntu 12.04.03 LTS geïnstalleerd
 * 5x 5GB disken die niet geformateerd zijn (worden gebruikt voor OSD's)

Installatie
===========

Voorbereiding
-------------

Volgende stappen dienen op alle nodes te gebeuren **42on3** t/m **42on5** (en later ook **42on6** t/m **42on8**)

Netwerkverbindingen
^^^^^^^^^^^^^^^^^^^
De nodes (ook **42on1** en **42on2**) zijn aangemaakt met 3 netwerkverbindingen:
  * eth0: Adapter 1 - NAT
  * eth1: Adapter 2 - host-only: vboxnet2
  * eth2: Adapter 3 - host-only: vboxnet3

.. code-block:: shell-session

  ceph@42on1:~$ sudo vim /etc/network/interfaces

.. code-block:: shell-session

  # The primary network interface
  auto eth0
  iface eth0 inet dhcp

  auto eth1
  iface eth1 inet static
    address 192.168.71.101
    netmask 255.255.255.0

  auto eth2
  iface eth2 inet static
    address 192.168.72.101 
    netmask 255.255.255.0

Sudo ceph-gebruiker
^^^^^^^^^^^^^^^^^^^
Om de installatie van ceph door ``ceph-deploy`` goed te laten verlopen, moet de ceph-gebruiker op node **42on3** toegevoegd worden aan ``sudoers``

.. code-block:: shell-session

  ceph@42on3:~$ echo "ceph ALL = (root) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/ceph
  ceph@42on3:~$ sudo chmod 0440 /etc/sudoers.d/ceph

/etc/hosts
^^^^^^^^^^
``/etc/hosts`` van elke node aanvullen met ip-adressen van ander nodes, dit om gemakkelijker verbinding te kunnen maken. Normaal hebben enkel de *Monitors* een vast ip.

.. code-block:: shell-session

  ceph@42on1:~$ sudo vim /etc/hosts

.. code-block:: shell-session

  127.0.0.1       localhost
  192.168.71.101  42on1
  192.168.71.102  42on2
  192.168.71.103  42on3
  192.168.71.104  42on4
  192.168.71.105  42on5
  192.168.71.106  42on6
  192.168.71.107  42on7
  192.168.71.108  42on8



Ssh-key
^^^^^^^
De ssh-key van **42on3** pushen naar andere nodes (**42on4** & **42on5**) zodat **42on3** de commando's via deze node naar de andere nodes doorsturen.

.. code-block:: shell-session
  
  ceph@42on3:~$ ssh-key
  ...
  [kuylensa@dfsdfsd]$ pip search pygments
  kuylensa@sdfsdfsd: $
  kuylensa:$ dfdf
  kuylensa:# sdsd 
