napalm-validate
---------------

* It's part of the `napalm <https://github.com/napalm-automation/napalm>`_ library
* It allows you build rules to retrieve data from the devices and ensure correctness
* It leverages on ``getters`` so if napalm can get the information it can be validated

|

* `Official documentation <http://napalm.readthedocs.io/en/latest/validate/index.html>`_

napalm-validate (rules)
-----------------------

.. rst-class:: float-50

.. literalinclude:: /2_napalm_validate/facts_rule.yaml
   :emphasize-lines: 1

.. rst-class:: float-50

Getter to use. Rules will apply to the data returned by it

napalm-validate (rules)
-----------------------

.. rst-class:: float-50

.. literalinclude:: /2_napalm_validate/facts_rule.yaml
   :emphasize-lines: 2

.. rst-class:: float-50

Exact match on the hosntame

napalm-validate (rules)
-----------------------

.. rst-class:: float-50

.. literalinclude:: /2_napalm_validate/facts_rule.yaml
   :emphasize-lines: 3

.. rst-class:: float-50

Regular expression against the OS version

napalm-validate (rules)
-----------------------

.. rst-class:: float-50

.. literalinclude:: /2_napalm_validate/facts_rule.yaml
   :emphasize-lines: 4-

.. rst-class:: float-50

Strict check of a list, ``_mode: strict`` will report if there is an exact match on elements

napalm-validate (rules)
-----------------------

.. rst-class:: float-50

.. literalinclude:: /2_napalm_validate/facts_rule_relaxed.yaml
   :emphasize-lines: 4-

.. rst-class:: float-50

Relaxed check of a list, will report if specified elements are missing but will not report if there are others as well

napalm-validate (BGP rule)
--------------------------

.. rst-class:: float-50

.. literalinclude:: /2_napalm_validate/bgp_rule.yaml
   :language: yaml
   :emphasize-lines: 1

.. rst-class:: float-50

Getter to use

napalm-validate (BGP rule)
--------------------------

.. rst-class:: float-50

.. literalinclude:: /2_napalm_validate/bgp_rule.yaml
   :language: yaml
   :emphasize-lines: 4,5,11

.. rst-class:: float-50

We want only these peers present

napalm-validate (BGP rule)
--------------------------

.. rst-class:: float-50

.. literalinclude:: /2_napalm_validate/bgp_rule.yaml
   :language: yaml
   :emphasize-lines: 8-10,14-16

.. rst-class:: float-50

Check we are getting prefixes (``num_prefixes > 0``)

napalm-validate (Routing rule)
------------------------------

.. rst-class:: float-50

.. literalinclude:: /2_napalm_validate/routing_rule.yaml
   :language: yaml
   :emphasize-lines: 1,2,10,11

.. rst-class:: float-50

You can add as many rules as you want, you can even repeat the same getter by naming them.

napalm-validate (Routing rule)
------------------------------

.. rst-class:: float-50

.. literalinclude:: /2_napalm_validate/routing_rule.yaml
   :emphasize-lines: 3-5,12-14

.. rst-class:: float-50

You can also pass arguments to the getters if needed

napalm-validate (Routing rule)
------------------------------

.. rst-class:: float-50

.. literalinclude:: /2_napalm_validate/routing_rule.yaml
   :emphasize-lines: 8,9,17-18

.. rst-class:: float-50

Check we have at least those paths with those exact ``next_hop``. Other attributes or other potential paths don't matter


How to use it
-------------

.. rst-class:: float-50



.. code-block:: yaml
    :caption: ansible

    - name: "validate deployment"
      napalm_validate:
        hostname: "{{ host }}"
        username: "{{ username }}"
        dev_os: "{{ os }}"
        password: "{{ password }}"
        optional_args:
          port: "{{ port }}"
        validation_file: "validate.yaml"
      register: report

.. rst-class:: float-50

.. code-block:: python
    :caption: python

    from napalm import get_network_driver

    eos_driver = get_network_driver("eos")
    eos_config = {
        "hostname": "localhost",
        "username": "vagrant",
        "password": "vagrant",
        "optional_args": {"port": 12443},
    }

    with eos_driver(**eos_config) as eos:
        eos.compliance_report("validate.yml")


Result
------

Not very human-readable but easy to parse programmatically

.. rst-class:: smallest-text float-50

.. literalinclude:: /3_ansible/static_error_2
   :language: bash
   :lines: 23-29,35-
   :emphasize-lines: 1,17-

.. rst-class:: smallest-text float-50

.. literalinclude:: /3_ansible/static_error_2
   :language: bash
   :lines: 60-
   :emphasize-lines: 13-15
