resolv.conf
=========

This basic role ensures `/etc/resolv.conf` is set with the values needed for DNS resolution.

Requirements
------------

Please fill out the variables located under the `templates` directory. A template is saved as a `*.j2` Jinja template to provide options in the future.

Role Variables
--------------

No variables are defined under this role's `variables` directory and a template named `resolv.conf.j2` under the `template` directory is used instead.

Dependencies
------------

A RedHat operating system with a version supporting NewtorkManager is required.

Example Playbook
----------------

Below is an example of calling the role:

  tasks:
    - name: Configure /etc/resolv.conf
      include_role:
        name: resolv.conf

License
-------

BSD 3-Clause License

Author Information
------------------

DevNet
