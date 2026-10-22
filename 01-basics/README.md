# Lab 1

## Ansible Intro

We will use Ansible in this course as a configuration management tool. There are
dozens of configuration management tools out there but we've chosen Ansible as
one of the simplest one to get started with.

You can run Ansible from your laptop directly in case you have Linux or MacOS.
In case you have Windows, use WSL.

## Task 1. Install Ansible

It is recommended to use Python virtual env. Recommended Ansible version needs Python
3.12 or newer:

    python3 -m venv ~/ansible-venv
    ~/ansible-venv/bin/pip install ansible==14.3.1
    ~/ansible-venv/bin/ansible --version

Last command should print something like

    ansible [core 2.21.3]

Then, add this line to your `~/.profile` file (if the file is missing, create it)
so you can use 'shorter' commands:

    PATH="$HOME/ansible-venv/bin:$PATH"

Logout and log in again. Now this command should also work:

    ansible --version


## Task 2. Get access to Cisco DevNet lab

Login to https://devnetsandbox.cisco.com/DevNet using your netacad.com credentials.

Request "Catalyst 8000 Always-On Sandbox" or "Catalyst 9000 Always-On Sandbox". Read instructions carefully, especially part what is ok to do what is not in there.

Ansible uses ssh for device management. If you can't ssh to device, Ansible can't either.

Try:

    ssh <username>@<host>

You should be able to login.

## Task 3. Create Ansible inventory

Create inventory file in project folder.

The only content in inventory you need right now (list contains only 1 element):

    <fqdn_of_your_device>

## Task 4. Create Ansible configuration file

Create Ansible configuration file in project folder.

Docs: https://docs.ansible.com/ansible/latest/reference_appendices/config.html

Parameters that should be specified:

    - inventory
    - ask_pass
    - gathering

## Task 5. Ansible Ad-Hoc commands

Using Ad-Hoc commands is not the best practice usually, but sometimes they can be useful for gathering info from devices.

Write commands that will collect the following from group "all":

    - list of IP addresses
    - arp table
    - routing table

## Hints

Specify ansible_user for every device you connecting. You won't need to specify username every time after that.
