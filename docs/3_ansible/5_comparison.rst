Comparison (assert)
-------------------

.. rst-class:: smallest-text float-60

.. code-block:: yaml
    :caption: assert

    - name: Check BGP sessions are healthy
      assert:
        that:
          - item.value.is_up
        msg: "{{ item.key }} is down"
      with_dict: "{{ napalm_bgp_neighbors.global.peers }}"
      tags: [print_action]
    - name: Check BGP sessions are receiving prefixes
      assert:
        that:
          - item.value.address_family.ipv6.received_prefixes > 0
        msg: "{{ item.key }} is not receiving any prefixes"
      with_dict: "{{ napalm_bgp_neighbors.global.peers }}"
      tags: [print_action]

.. rst-class:: float-30

    * cumbersome to write
    * the more complex the data the more cumbersome it will become
    * if a test fails, rest won't run

Comparison (static rules)
-------------------------

.. rst-class:: smallest-text float-60

.. code-block:: jinja
    :caption: static rules

    ---
    - get_interfaces_ip:
        Ethernet1:
          ipv6:
            2001:db8:caf3:12:::
              prefix_length: 127
        Ethernet2:
          ipv6:
            2001:db8:caf3:13:::
              prefix_length: 127
        Loopback0:
          ipv6:
            2001:db8:b33f::1:
              prefix_length: 128

.. rst-class:: float-30

    * easy to write
    * easy to read
    * a failed test won't stop the rest
    * scales fine with complexity
    * static rules are good for low changing environments

Comparison (dynamic rules)
--------------------------

.. rst-class:: smallest-text float-60

.. code-block:: jinja
    :caption: dynamic rules

    ---
    - get_interfaces_ip:
    {% for interface in interfaces %}
    {{ interface.name }}:
          ipv6:
            {{ interface.ip_address.split("/")[0] }}:
              prefix_length: {{ interface.ip_address.split("/")[1] }}
    {% endfor %}

.. rst-class:: float-30

    * easy to write
    * easy-ish to read
    * a failed test won't stop the rest
    * scales fine-ish with complexity (scales with configuration template)
    * dynamic rules are good for high changing environments
