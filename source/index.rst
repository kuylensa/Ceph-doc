.. Ceph-doc documentation master file, created by
   sphinx-quickstart on Mon Feb  3 11:34:34 2014.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to Ceph-doc's documentation!
====================================

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
De installatie van Ceph terug overlopen naar aanleiding van Ceph-opleiding van 42on.com (gegeven door Wido den Hollander) op 28 en 29 janurari 2014 te Breda.

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

.. code-block:: guess
	print("Hello World")

 sdfsdfrr