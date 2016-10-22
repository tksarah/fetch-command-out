fetch-files
=========

Ansible Role: fetch-file
------------

As executing the playbook, the role is copied from the target host in background and save it as a text file.
The role allows to get the list of specific files simply.

Install
------------

```
# ansible-galaxy install --roles-path ./roles tksarah.fetch-command-out
```


Requirements
------------

* Ansible version 2.0 <
* Perl

Role Variables
--------------

Available variables are listed below,

**defaults/main.yml:**
```
savedir: "fetched"
remtemp: "/tmp/remtemp"
```

**vars/com_vars.yml:**

```
lists:
  - { com: 'ls -l /etc' , ofile: 'ls_l' }
  - { com: 'hostid' , ofile: 'hostid_out' }
  - { com: 'cat /etc/hosts' , ofile: 'cat_hosts' }
```

This roles was created with automatic when you run a playbook.
You only create a text file listed to get files below,


```
<command>,<output file>
<command>,<output file>
<command>,<output file>
```

**Ex) sample_comlist.txt:**

```
'ls -l /etc','ls_l'
'hostid','hostid_out'
'cat /etc/hosts','cat_hosts'
```

Dependencies
------------

None

Example Playbook
----------------

```
- name: Playbook for fetching files
  hosts: "{{ target }}"
  gather_facts: no

  become: true
  become_user: root

  vars_prompt:
    - { name: "target" , prompt: "Input target host" ,  default: all , private: no }
    - { name: "inputfile" , prompt: "Input your file list" , default: sample_filelist.txt , private: no }
    - { name: "savedir" , prompt: "Input a save directory" , default: fetched , private: no }

  pre_tasks:
    - block:
        - name: Pre setup (create lists for fetch)
          local_action: raw ./roles/tksarah.fetch-files/tools/create_vars.pl "{{ inputfile }}"
      become: false

  roles:
    - tksarah.fetch-files

```

License
-------

BSD

Author Information
------------------

fetch-file role was written by: T.Kuramochi
