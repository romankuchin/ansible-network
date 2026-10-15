# Lab 2

## Task 1. Pick up student ID

With the help of your imagination or 

    echo $(( RANDOM % 255 ))

choose random number as ID for the next tasks.

## Task 2. Collect info about the device

With the help of `ios_facts` module create a report about the sandbox router.
Report should be in "reports/\<device_name\>.txt".

Content of report:

    Hostname: XXX
    Model: XXX
    IOS type: XXX
    IOS version: XXX
    Serial number: XXX

## Task 3. Backup your config

Create a config backup before applying any changes to the device.

Backup files should be in folder "backups" with names
"\<device_name\>.txt"

## Task 4. Configuration

Create set of tasks that configure:

    - your loopback interface (description and ip address)
    - syslog server
    - ntp server
    - service timestamps for log and debug messages

Pick syslog, ntp and loopback addresses from `10.{{ student_id }}.0.0/24`
range.

Define NTP server and syslog server IPs in group_vars/all.yaml.

Use `ios_config` module.

## Task 5. Make your playbook idempotent

If you didn't change any variables or the playbook itself, this command

    ansible-playbook lab02.yaml

shouldn't report any changes.

Most probably you will have to adjust the tasks in the playbook to make it
idempotent.

## Task 6. Save the configuration

Save the running configuration only if something actually changed. Do nothing
when there were no changes.

## Task 7. Cleanup playbook

Create `cleanup.yaml` that removes every object your playbook created and
leaves the rest of the device untouched.

Please run it at the end of every lab session.

## Hints

`ios_config` can manage a section of the configuration with `parents` and
`lines`. Indentation of `lines` should match how the config looks on the device.
