.. rst-class:: agenda

Agenda
------

1. Introducation
2. napalm (validate)
3. **ansible + napalm (validate)**
    1. **Objective**
    2. assertions
    3. static rules
    4. dynamic rules

Objective
---------

To interconnect three devices forming a triangle and ensure BGP sessions come up and loopbacks are redistributed correctly and reachable.

.. image:: /_static/net.png
   :scale: 50 %

Objective (ansible preparation)
-------------------------------

.. rst-class:: float-50

.. literalinclude:: /3_ansible/roles.tree
   :emphasize-lines: 2,10,17

.. rst-class:: float-50

Three roles

Objective (ansible preparation)
-------------------------------

.. rst-class:: float-50

.. literalinclude:: /3_ansible/roles.tree
   :emphasize-lines: 2-9

.. rst-class:: float-50

Base configuration; users, snmp, hostname, etc...

Objective (ansible preparation)
-------------------------------

.. rst-class:: float-50

.. literalinclude:: /3_ansible/roles.tree
   :emphasize-lines: 10-16

.. rst-class:: float-50

Interfaces, IP addresses, BGP sessions and policies...

Objective (ansible preparation)
-------------------------------

.. rst-class:: float-50

.. literalinclude:: /3_ansible/roles.tree
   :emphasize-lines: 17-

.. rst-class:: float-50

Role using napalm to deploy the configuration

Objective (ansible data)
-------------------------------

.. rst-class:: float-50

.. literalinclude:: /../2_intent/host_vars/r1.yaml

.. rst-class:: float-50

Data to build a fabric for demonstration purposes


Objective (validations)
-----------------------

Three different roles with three different approaches for validation:


* **validate_assert** - using ansible's ``assert``
* **validate_static** - using static rules for ``napalm_validate``
* **validate_dynamic** - using dynamic rules for ``napalm_validate``

