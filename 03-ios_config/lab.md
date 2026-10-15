# Lab 3

## Task 1. Loopback interfaces from a template

Create `loopbacks` variable that describes the interfaces you own:

    loopbacks:
      0:
        ip: 10.{{ student_id }}.0.1/26
        description: Some text  # Optional
        ospf_area: 2  # Optional. Don't enable OSPF if missing
      1:
        ip: 10.{{ student_id }}.1.1/27
      2:
        ip: 10.{{ student_id }}.2.1/28
        ospf_area: 32
      3:
        ...
      4:
        ...

The interface name must be built from `student_id` and the key, so student_id 101
leads to Loopback1010, Loopback1011 and so on. Addresses must stay inside your own
range.

The OSPF process id is your `student_id`, so you
don't share a process with anybody else.

Do not use OSPF network statements inside `router ospf X` configuration. Use

      (config-if)# ip ospf <student_id> area X

Create a Jinja2 template that renders the interface configuration from this
variable, and a task in the playbook that applies it.

Note: `description` and `ospf_area` are optional. Your template has to handle
such cases.

## Task 2. Add OSPF authentication

Configure md5 OSPF authentication on your own loopback interfaces.

Use Ansible Vault to store the password in your repo. Update your `ansible.cfg`
with the vault password file location. The vault password file must be outside
of your repo.

## Task 3. Print some OSPF info

Parse the output of `show ip ospf <student_id> interface brief` and print the
number of interfaces that are enabled for your OSPF process.

## Task 4. Access list

Create extended access list acl_\<student_id\>.

The acl must permit OSPF in the first line:

      permit ospf any any

Don't allow any changes in the access list if the first line doesn't permit
OSPF. In this case, just print a message with an explanation. Example ACL you try to apply:

      - permit tcp host 192.168.10.10 host {{ ansible_host }} eq 22
      - deny ip any any

Example of message you receive:

      Skipping ACL change because OSPF is not permitted in the first line. Possible outage.

Any changes to access-list should be applied if 1st line matches with condition and no changes to access-list should be done if condition is not satisfied.

Use `ios_config` module.

## Task 5. Apply the access list

Apply the access list inbound to one of your loopback interfaces from previous tasks. For lab purpose it will be enough.

Do not apply it to vty lines or to the management interface - that might break the environment.

## Task 6. Cleanup playbook

Extend your `cleanup.yaml` playbook so it also removes everything this lab created:
  - access-list
  - OSPF process
  - interfaces

## Expected result

All configuration should be done with a single command without any parameters:

    ansible-playbook lab03.yaml

If you didn't change any variables or templates, a second run shouldn't report
any changes.

## Hints

Your `loopbacks` variable uses prefix length, IOS expects a dotted netmask. The
`ansible.utils` collection has filters for that conversion, you don't need to
write a lookup table.
