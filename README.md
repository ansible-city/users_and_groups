# Users and Roles

Master: [![Build Status](https://travis-ci.org/ansible-city/users_and_groups.svg?branch=master)](https://travis-ci.org/ansible-city/users_and_groups)  
Develop: [![Build Status](https://travis-ci.org/ansible-city/users_and_groups.svg?branch=develop)](https://travis-ci.org/ansible-city/users_and_groups)

* [ansible.cfg](#ansible-cfg)
* [Installation and Dependencies](#installation-and-dependencies)
* [Tags](#tags)
* [Examples](#examples)

This roles manages OS users and groups.




## ansible.cfg

This role is designed to work with merge "hash_behaviour". Make sure your
ansible.cfg contains these settings

```INI
[defaults]
hash_behaviour = merge
```




## Installation and Dependencies

This role has no dependencies.

To install run `ansible-galaxy install ansible-city.users_and_groups` or add
this to your `roles.yml`

```YAML
- name: ansible-city.users_and_groups
  version: v1.0
```

and run `ansible-galaxy install -p ./roles -r roles.yml`




## Tags

This role uses two tags: **build** and **configure**

* `build` & `configure` - Ensures that specified groups and users are
  present.




## Examples

Simple example for creating two users and two groups.

```YAML
- name: Install GO CD Server
  hosts: sandbox

  roles:
    - name: ansible-city.users_and_groups
      users_and_groups:
        groups:
          - name: lorem
            system: yes
          - name: ipsum
        users:
          - name: lorem.ipsum
            groups:
              - ipsum
              - lorem
            ssh_key: ./lorem.ipsum.pub
          - name: dolor.ament
            groups:
              - ipsum
```

In most cases you would keep the list of users in external vars file or
group|host vars file.

```YAML
- name: Install GO CD Server
  hosts: sandbox

  vars_files:
    - "vars/sandbox/users.yml"

  roles:
    - name: ansible-city.users_and_groups
      users_and_groups:
        groups: "{{ base_image.os_groups }}"
        users: "{{ base_image.admins }}"

    - name: ansible-city.users_and_groups
      users_and_groups:
        users: "{{ developers }}"
```

Add selected group to sudoers

```YAML
- name: Configure User Access
  hosts: sandbox

  vars_files:
    - "vars/sandbox/users.yml"

  roles:
    - role: sansible.users_and_groups
      users_and_groups:
        groups: "{{ base_image.os_groups }}"
        users: "{{ base_image.admins + developers }}"

    - role: sansible.users_and_groups
      users_and_groups:
        sudoers:
          - name: wheel
            user: "%wheel"
            runas: "ALL=(ALL)"
            commands: "NOPASSWD: ALL"
```
