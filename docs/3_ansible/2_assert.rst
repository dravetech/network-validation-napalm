.. rst-class:: agenda

Agenda
------

1. Introducation
2. napalm (validate)
3. **ansible + napalm (validate)**
    1. Objective
    2. **assertions**
    3. static rules
    4. dynamic rules

Assert (role)
-------------

.. rst-class:: float-50

.. literalinclude:: /../2_intent/roles/validate_assert/tasks/main.yaml
   :lines: 2-13
   :emphasize-lines: 1-2

.. rst-class:: float-50

    (role continues next slide)

    Wait 10 seconds so BGP converges

Assert (role)
-------------

.. rst-class:: float-50

.. literalinclude:: /../2_intent/roles/validate_assert/tasks/main.yaml
   :lines: 2-13
   :emphasize-lines: 3-11

.. rst-class:: float-50

    (role continues next slide)

    Get BGP information from the device with napalm

Assert (role)
-------------

.. rst-class:: float-70

.. literalinclude:: /../2_intent/roles/validate_assert/tasks/main.yaml
   :lines: 13-
   :emphasize-lines: 1-7

.. rst-class:: float-30

    (continues)

    Check that all BGP sessions are up

Assert (role)
-------------

.. rst-class:: float-70

.. literalinclude:: /../2_intent/roles/validate_assert/tasks/main.yaml
   :lines: 13-
   :emphasize-lines: 8-

.. rst-class:: float-30

    (continues)

    Check we are receiving prefixes

Assert (example 1)
------------------

.. rst-class:: smallest-text float-70

.. code-block:: bash

    Pausing for 10 seconds
    (ctrl+C then 'C' = continue early, ctrl+C then 'A' = abort)
    ....
    # validate_assert : Check BGP sessions are healthy ************************************************************************
      * r1                         - changed=False -- All items completed -----------------------------
          * 2001:db8:caf3:12::1         - changed=False -- All assertions passed ---------------------------
          * 2001:db8:caf3:13::1         - changed=False -- All assertions passed ---------------------------
      * r2                         - changed=False -- All items completed -----------------------------
          * 2001:db8:caf3:23::1         - changed=False -- All assertions passed ---------------------------
          * 2001:db8:caf3:12::          - changed=False -- All assertions passed ---------------------------
      * r3                         - changed=False -- All items completed -----------------------------
          * 2001:db8:caf3:13::          - changed=False -- All assertions passed ---------------------------
          * 2001:db8:caf3:23::          - changed=False -- All assertions passed ---------------------------
    # validate_assert : Check BGP sessions are receiving prefixes *************************************************************
      * r1                         - changed=False -- All items completed -----------------------------
          * 2001:db8:caf3:12::1         - changed=False -- All assertions passed ---------------------------
          * 2001:db8:caf3:13::1         - changed=False -- All assertions passed ---------------------------
      * r2                         - changed=False -- All items completed -----------------------------
          * 2001:db8:caf3:23::1         - changed=False -- All assertions passed ---------------------------
          * 2001:db8:caf3:12::          - changed=False -- All assertions passed ---------------------------
      * r3                         - changed=False -- All items completed -----------------------------
          * 2001:db8:caf3:13::          - changed=False -- All assertions passed ---------------------------
          * 2001:db8:caf3:23::          - changed=False -- All assertions passed ---------------------------

    # STATS *******************************************************************************************************************
    r1    : ok=11	changed=0	failed=0	unreachable=0
    r2    : ok=10	changed=0	failed=0	unreachable=0
    r3    : ok=10	changed=0	failed=0	unreachable=0

.. rst-class:: float-30

All green

Assert (example 2)
------------------

.. rst-class:: smallest-text float-70

.. code-block:: bash

    Pausing for 10 seconds
    (ctrl+C then 'C' = continue early, ctrl+C then 'A' = abort)
    ....
    # validate_assert : Check BGP sessions are healthy ************************************************************************
      * r1                         - FAILED!!! -- One or more items failed ----------------------------
          * 2001:db8:caf3:12::1         - FAILED!!! -- 2001:db8:caf3:12::1 is down -------------------------
          * 2001:db8:caf3:13::1         - changed=False -- All assertions passed ---------------------------
      * r2                         - FAILED!!! -- One or more items failed ----------------------------
          * 2001:db8:caf3:23::1         - changed=False -- All assertions passed ---------------------------
          * 2001:db8:caf3:12::          - FAILED!!! -- 2001:db8:caf3:12:: is down --------------------------
      * r3                         - changed=False -- All items completed -----------------------------
          * 2001:db8:caf3:13::          - changed=False -- All assertions passed ---------------------------
          * 2001:db8:caf3:23::          - changed=False -- All assertions passed ---------------------------
    # validate_assert : Check BGP sessions are receiving prefixes *************************************************************
      * r3                         - changed=False -- All items completed -----------------------------
          * 2001:db8:caf3:13::          - changed=False -- All assertions passed ---------------------------
          * 2001:db8:caf3:23::          - changed=False -- All assertions passed ---------------------------

    # STATS *******************************************************************************************************************
    r1    : ok=9	changed=0	failed=1	unreachable=0
    r2    : ok=8	changed=0	failed=1	unreachable=0
    r3    : ok=10	changed=0	failed=0	unreachable=0

.. rst-class:: float-30

Something failed, note tests got interrupted
