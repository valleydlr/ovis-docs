.. Copyright 2022 Sandia National Laboratories, LLC
   (c.f. AUTHORS, NOTICE.LLNS, COPYING)

   SPDX-License-Identifier: (LGPL-3.0)

.. Flux documentation master file, created by
   sphinx-quickstart on Fri Jan 10 15:11:07 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome To OVIS-HPC Documentation!
================================

OVIS is a modular system for HPC data collection, transport, storage, -log message exploration, and visualization as well as analysis.

LDMS is a low-overhead, low-latency framework for collecting, transfering, and storing metric data on a large distributed computer system.

A github repository for the OVIS-LDMS source code can be found here: https://github.com/ovis-hpc/ovis 

.. toctree::
   :maxdepth: 3
   :caption: Contents

   quickstart
   adminguide
   contributing
   debugging
   batch
   hierarchies
   coral
   coral2
   faqs
   stats

.. toctree::
   :maxdepth: 1
   :caption: Sub-Projects
   :hidden:

   flux-core <https://flux-framework.readthedocs.io/projects/flux-core/en/latest/index.html>
   flux-sched <https://flux-framework.readthedocs.io/projects/flux-sched/en/latest/index.html>
   flux-security <https://flux-framework.readthedocs.io/projects/flux-security/en/latest/index.html>
   RFCs <https://flux-framework.readthedocs.io/projects/flux-rfc/en/latest/index.html>
   Workflow Examples <https://flux-framework.readthedocs.io/projects/flux-workflow-examples/en/latest/index.html>


Contributor Relevant RFCs
-------------------------

- :doc:`rfc:spec_2`
- :doc:`rfc:spec_7`
- :doc:`rfc:spec_9`


All RFCs
--------

- :doc:`rfc:index`


Manual Pages
------------

- :ref:`core:man-pages`
- :ref:`sched:man-pages`
- :ref:`security:man-pages`


Workflow Examples
-----------------

- :doc:`workflow-examples:index`

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
