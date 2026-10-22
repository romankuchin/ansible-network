# Lab 4

## Notes

    - Use "PATCH" for RESTCONF. "PUT" replaces everything at the target path
    - "PUT" is only acceptable when the path ends at an object you own,
      for example .../interface/Loopback=1010
    - "PUT" on .../native/interface would wipe every interface on the router
    - NETCONF: use default_operation "merge", never "replace"

## Task 1. Requirements and connections

NETCONF needs the `ncclient` Python library. Install it into the same virtual env
you made in lab 1.

NETCONF and RESTCONF need different
connection plugins, so add two more groups to your inventory.

For NETCONF:

    ansible_port: 830
    ansible_connection: ansible.netcommon.netconf
    ansible_network_os: default

For RESTCONF:

    ansible_connection: ansible.netcommon.httpapi
    ansible_network_os: ansible.netcommon.restconf
    ansible_httpapi_port: 443
    ansible_httpapi_use_ssl: true
    ansible_httpapi_validate_certs: false

Use `validate_certs: false` in real scenarios with caution.

## Task 2. Find out what the device supports

Get the list of NETCONF capabilities and the list of YANG models the router
supports.

Save the list of models to "reports/yang_models.txt".

## Task 3. Configure loopback interfaces over NETCONF

Create a Jinja2 template that renders the XML payload for loopback
interface IPs and descriptions from the same variables you already
have in lab 3.

Apply it with `netconf_config`.

Hint: without `get_filter` ansible will try to compare desired configuration against the whole config,
which might not fit into single response. Better to compare only with subset of config with:

    <native xmlns="http://cisco.com/ns/yang/Cisco-IOS-XE-native">
        <interface>
            <Loopback/>
        </interface>
    </native>

## Task 4. Configure OSPF over RESTCONF

Create a Jinja2 template that renders the JSON payload for loopback
interfaces OSPF configurations from the same variables you already
have in lab 3.

Apply it with `restconf_config`.

## Task 5. Print some OSPF info

Get OSPF operational data and print the
number of interfaces that are enabled for your OSPF process.

Find the right models in your list from task 2. They are not the same models that were used for configuration.

## Task 6. Cleanup playbook

Extend your cleanup playbook so it removes the loopback interfaces this lab
created. Remove them with NETCONF or RESTCONF.

## Expected result

All configuration should be done with a single command without any parameters:

    ansible-playbook lab04.yaml

If you didn't change any variables or templates, a second run shouldn't report
any changes.

## Hints

Playbook will have at least 2 plays inside, one for netconf group, another for restconf group. Group members are the same, but connection parameters are different.

Render payloads the same way as in lab 3, with a template lookup:

    ansible.netcommon.netconf_config:
      content: "{{ lookup('ansible.builtin.template', 'loopback.xml.j2') }}"

`netconf_get` can return the data as json instead of xml.

`restconf_config` uses method `post` by default. Read "notes" from above.
