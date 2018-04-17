sosreport
=========

This role creates sosreport files on remote hosts and collects them into a
directory of choice on the controller. It can apply environment variables to
the `sosreport` command if specified, e.g. for collecting report on OpenStack
nodes.

Requirements
------------

* `sosreport` utility present on the remote host, installed if missing.

Role Variables
--------------

The default variables are:

* A local directory to collect sosreport files to on the controller.

      sosreport_local_dir: ~/sosreport

* A remote directory to output sosreport to on the remote host.
  
      sosreport_remote_dir: /tmp/sosreport

* Arguments to the sosreport command.
  
      sosreport_args:
        - --all-logs
        - --quiet
        - --batch
        - --verify

There are additional variables, which are optional and don't have defaults:
  
* A Red Hat Support case number. The case number will be amended to the
  `sosreport` command and appear in the resulting file name.

      sosreport_case:
  
* The name of a hash containing environment variables. If provided, the hash
  will be passed to the `sosreport` command. This is useful when providing
  sosreport for OpenStack nodes.

      sosreport_env:

Example Playbook
----------------

An example of a playbook for collecting sosreports would be as follows:

```yaml
- hosts: "{{ servers }}"
  become: true
  gather_facts: no

  vars:
    sosreport_env: "{{ my_rc }}"

  tasks:
    - name: Collect sosreport
      import_role:
        name: gadamiak.sosreport
```

The command to run the playbook against 'my_servers' and related to a case
'02090101' would be:

```bash
$ ansible-playbook sosreport -e servers=my_servers -e sosreport_case=02090101
```

License
-------

GPLv3
