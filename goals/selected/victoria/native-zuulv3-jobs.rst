=================================
Switch legacy Zuul jobs to native
=================================

As part of the switch to Zuul v3, all the existing jobs have been
automatically converted from their Jenkins counterparts.
Unfortunately not all of them have been migrated to fully native
Zuul v3 jobs, making their maintainance more complicated.
Legacy jobs, created using automated scripts, brings with them
a lot of duplicated code. They also rely on obsoleted (zuul-cloner)
or difficult to maintain components (devstack-gate) and in general
an older job creation logic. All these factors make their
maintainence extremely difficult.

Given the benefits of the usage of native Zuul v3 jobs, it is
worth asking why not all the jobs have been converted.
There are several reasons. For example, the required functionality
for native Tempest tests were not available at the time of the switch.
Native Grenade jobs are not yet available,
even though some work is in progress.

Nevertheless, the general status improved significantly in the last year
and we can finally move the remaining legacy jobs to the native Zuul v3
jobs for all official OpenStack projects.

Champion
========

Luigi Toscano (tosky)

Gerrit Topic
============

To facilitate tracking, commits related to this goal should use the
gerrit topic::

  native-zuulv3-migration

Completion Criteria
===================

#. All legacy jobs for official OpenStack projects, if still relevant,
   should be replaced by native Zuul v3 jobs in the master branch.
#. The replacement jobs should be designed in a way which does not
   prevent their backport to the stable branches.

Stretch goals
=============

#. If a replaced job is also used in some stable branches, backport it
   to all supported branches.

References
==========

The main reference for converting legacy Zuul jobs is the `Zuul v3
Migration Guide <https://docs.openstack.org/infra/manual/zuulv3.html>`_,
part of the `Infrastructure User Manual
<https://docs.openstack.org/infra/manual/>`_.

A complete reference to the Zuul jobs is available as part of the
`Zuul User's Guide <https://zuul-ci.org/docs/zuul/user/config.html#job>`_.

During the Dublin PTG 2018 the QA team provided some updates and
examples on the Zuul v3 migrations. The slides are not merged yet
in the publications repository but the `work-in-progress patch
<https://review.opendev.org/548178>`_ can still be useful.

Examples of migration:

- sahara-tests: https://review.opendev.org/512058/
  and https://review.opendev.org/547669/

  This latter was an early migration and the jobs have evolved
  a bit since then. Specifically, the second change fixes the
  multinode support. Together, and taking into account
  the current version of the affected files, they are a
  good starting point.

  This porting includes both "straightforward" conversions
  (inheriting from devstack-tempest)
  and examples of custom ansible code.

- python-cinderclient: https://review.opendev.org/672784/
  A more recent example of a devstack (non-tempest) job.

- heat-functional-tests: https://review.opendev.org/660877

  This example collect tests from two repositories (heat and
  heat-tempest-plugin). Also example for gabbit tests.

Current State / Anticipated Impact
==================================

Jobs which executes devstack alone or combined with common
operations like running tempest tests have been ported already or
can be ported without many problems.

Jobs which requires less standard operations (for example, a cycle of
reconfiguration/test executions, or custom tests) require additional
work.

Grenade-based jobs are not yet available, but there are `patches in advanced
state of development <https://review.opendev.org/#/q/topic:grenade_zuulv3>`_
and they should be ready when the Victoria cycle starts.
If not, finalizing the effort is considered part of the goal,
which can still move forward with all the other non-Grenade conversions.
Once they are finalized, porting most Grenade jobs should be trivial.

While cleaning the legacy jobs from `openstack-zuul-jobs
<https://opendev.org/openstack/openstack-zuul-jobs.git>`_ and
`project-config <https://opendev.org/openstack/project-config.git>`_
is not part of the completion criteria, achieving the stretch goal
would reduce the amount of items to track in those repositories.
