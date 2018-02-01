.. rst-class:: agenda

Agenda
------

1. Introducation
2. napalm (validate)
3. **ansible + napalm (validate)**
    1. Objective
    2. assertions
    3. **static rules**
    4. dynamic rules

Static (rules)
--------------

.. rst-class:: float-50

.. literalinclude:: /3_ansible/static_rules.tree
   :emphasize-lines: 3,7,11

.. rst-class:: float-50

Each hosts gets its own set of rules

Static (rules)
--------------

.. rst-class:: float-50

.. literalinclude:: /3_ansible/static_rules.tree
   :emphasize-lines: 4-6,8-10,12-14

.. rst-class:: float-50

We divide the rules per purpose (optional)

Static (Interfaces rule)
------------------------

.. rst-class:: float-50

.. literalinclude:: /../2_intent/roles/validate_static/files/r1/interfaces.yaml

.. rst-class:: float-50

    Simple check to guarantee interfaces have the correct IPs

    Note, this is tailored for one of the routers, the other routers will have this data tailored for them as well

Static (BGP rule)
-----------------

.. rst-class:: float-50

.. literalinclude:: /../2_intent/roles/validate_static/files/r1/bgp.yaml

.. rst-class:: float-50

    We check we have the right peers, that they are up and sending and receiving prefixes

    Note, this is tailored for one of the routers, the other routers will have this data tailored for them as well

Static (Routing rule)
---------------------

.. rst-class:: smaller-text float-50

.. literalinclude:: /../2_intent/roles/validate_static/files/r1/routes.yaml

.. rst-class:: float-50

    We check we have all paths to the rest of the loopbacks via the other two routers

    Note, this is tailored for one of the routers, the other routers will have this data tailored for them as well


Static (role)
-------------

.. rst-class:: smallest-text float-70

.. literalinclude:: /../2_intent/roles/validate_static/tasks/main.yaml
   :lines: 2-
   :emphasize-lines: 1-2

.. rst-class:: float-30

    Wait 10 seconds so BGP converges

Static (role)
-------------

.. rst-class:: smallest-text float-70

.. literalinclude:: /../2_intent/roles/validate_static/tasks/main.yaml
   :lines: 2-
   :emphasize-lines: 3-15

.. rst-class:: float-30

   Load validation rules for each case

Static (role)
-------------

.. rst-class:: smallest-text float-70

.. literalinclude:: /../2_intent/roles/validate_static/tasks/main.yaml
   :lines: 2-
   :emphasize-lines: 16-

.. rst-class:: float-30

   If ``napalm_validate`` fails print it as ``nice_json``

Static (example 1)
------------------

.. rst-class:: smallest-text float-70

.. code-block:: bash

    Pausing for 10 seconds
    (ctrl+C then 'C' = continue early, ctrl+C then 'A' = abort)
    .
    # validate_static : validate deployment ***********************************************************************************
      * r2                         - changed=False -- All items completed -----------------------------
          * interfaces                  - changed=False --  ------------------------------------------------
          * bgp                         - changed=False --  ------------------------------------------------
          * routes                      - changed=False --  ------------------------------------------------
      * r1                         - changed=False -- All items completed -----------------------------
          * interfaces                  - changed=False --  ------------------------------------------------
          * bgp                         - changed=False --  ------------------------------------------------
          * routes                      - changed=False --  ------------------------------------------------
      * r3                         - changed=False -- All items completed -----------------------------
          * interfaces                  - changed=False --  ------------------------------------------------
          * bgp                         - changed=False --  ------------------------------------------------
          * routes                      - changed=False --  ------------------------------------------------

    # STATS *******************************************************************************************************************
    r1    : ok=9	changed=0	failed=0	unreachable=0
    r2    : ok=8	changed=0	failed=0	unreachable=0
    r3    : ok=8	changed=0	failed=0	unreachable=0

.. rst-class:: float-30


All green!

Static (example 2)
------------------

.. rst-class:: smallest-text float-70

.. literalinclude:: /3_ansible/static_error
   :language: bash
   :emphasize-lines: 7,8,11,12,14,16

.. rst-class:: float-30

We can see which reports actually failed, not all of them did

Static (example 2)
------------------

.. rst-class:: smallest-text float-70

.. literalinclude:: /3_ansible/static_error_2
   :lines: 23-
   :language: bash
   :emphasize-lines: 1,22-24,26-29

.. rst-class:: float-30

EXAMPLE: If we inspect the report we can see that peer ``2001:db8:caf3:12::1`` is receiving prefixes but not sending any
