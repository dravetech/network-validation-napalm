.. rst-class:: agenda

Agenda
------

1. Introducation
2. napalm (validate)
3. **ansible + napalm (validate)**
    1. Objective
    2. assertions
    3. static rules
    4. **dynamic rules**

Dynamic rules
-------------

* Pretty much like static rules
* However, instead of having static files per device you have a generic template that resolves the data
* Those templates will be similar to the equivalent to configure the service

Dynamic rules (example)
-----------------------

.. rst-class:: smallest-text float-40

.. code-block:: yaml
    :caption: data

    interfaces:
      - name: "Loopback0"
        ip_address: "2001:db8:b33f::1/128"
      - name: "Ethernet1"
        ip_address: "2001:db8:caf3:12::/127"
      - name: "Ethernet2"
        ip_address: "2001:db8:caf3:13::/127"

.. rst-class:: smallest-text float-60

.. code-block:: jinja
    :caption: template for interfaces

    {% for interface in interfaces %}

    default interface {{ interface.name }}
    interface {{ interface.name }}
      {{ 'no switchport' if not interface.name.startswith("lo") else "" }}
      ipv6 address {{ interface.ip_address }}
      ipv6 address fe80::{{ inventory_hostname[-1] }}/64 link-local

    {% endfor %}

.. rst-class:: smallest-text float-40

.. code-block:: jinja
    :caption: rules for interfaces (static)

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

.. rst-class:: smallest-text float-60

.. code-block:: jinja
    :caption: rules for interfaces (dynamic)

    ---
    - get_interfaces_ip:
    {% for interface in interfaces %}
    {{ interface.name }}:
          ipv6:
            {{ interface.ip_address.split("/")[0] }}:
              prefix_length: {{ interface.ip_address.split("/")[1] }}
    {% endfor %}

Dynamic rules (exercise for the reader)
---------------------------------------

Create a role similar to ``/2_intent/roles/validate_static`` but using dynamic rules
