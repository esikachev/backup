
..
 This work is licensed under a Creative Commons Attribution 3.0 Unported
 License.

 http://creativecommons.org/licenses/by/3.0/legalcode

============================================
Implement Sahara cluster verification checks
============================================

https://blueprints.launchpad.net/sahara/+spec/cluster-verification

Now we don't have any check to take a look at cluster processes status through
Sahara interface. Our plans is to implement cluster verification and add
button for recheck processes in Horizon.

Problem description
===================

Sahara doesn't have any check for cluster processes  health monitoring.
One cluster processes can be broken or unavailable but Sahara will still
present it ACTIVE status.

Proposed change
===============

The good way to implement cluster verification checks is to add functional
tests for every processes in every plugin. Cluster automatically run this
checks after creating and also this check can be called from api for scenario
tests.

The status of cluster processes health should be indicated:

SUCCESS: True/False, DESCRIPTION(optional): description of problem

Cluster can be checked after "Active" state.

Alternatives
------------

As an alternative we can use this checks for testing cluster of scenario tests

Data model impact
-----------------

Need to add a table to DB with result of verification with column: id,
cluster_id, created_at, updated_at, checks, status.

.. sourcecode:: console

   {
      "cluster_verification": [
         {
           "id": "1",
           "cluster_id": "1111",
           "created_at": "2013-10-09 12:37:19.295701",
           "updated_at": "2013-10-09 12:37:19.295701",
           "status": "done",
           "checks":
             {
               "check_name":
                 {
                      "successful": "True"
                 },
             },
         },
      ]
   }

..

REST API impact
---------------

Get verification

.. sourcecode:: console

        GET /clusters/<cluster_id>/verification/<verification_id>

..

Get all verifications for cluster

.. sourcecode:: console

        GET /clusters/<cluster_id>/verification

..

Run verification

.. sourcecode:: console

        POST /clusters/<cluster_id>/verification/<verification_id>

..

Run all verifications for cluster

.. sourcecode:: console

        POST /clusters/<cluster_id>/verification

..

Delete verification

.. sourcecode:: console

        DELETE /clusters/<cluster_id>/verification/<verification_id>

..

Delete all verifications for cluster

.. sourcecode:: console

        DELETE /clusters/<cluster_id>/verification

..

Other end user impact
---------------------

Need to implement requests for run checks via python-saharaclient.

Deployer impact
---------------

None.

Developer impact
----------------

None.

Sahara-image-elements impact
----------------------------

None.

Sahara-dashboard / Horizon impact
---------------------------------

Dashboard impact is need to add new tab in cluster details with results of
verifications.

Implementation
==============

Assignee(s)
-----------

Primary assignee:
  esikachev

Other contributors:
  None

Work Items
----------

1. Implement verification check for each plugin
2. Implement run verification checks via python-saharaclient
3. Implement tab with verification results to Horizon
4. Need to add new WADL docs with new api-method

Dependencies
============

None.

Testing
=======

New unit tests and integration tests should be written for the feature.

Documentation Impact
====================

None.

References
==========

None.
