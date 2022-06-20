# ansible-role-cloud-compute

![](https://github.com/mtulio/ansible-role-cloud-compute/actions/workflows/release.yml/badge.svg)
![](https://github.com/mtulio/ansible-role-cloud-compute/actions/workflows/ci.yml/badge.svg?branch=main)
![](https://img.shields.io/ansible/role/59505)


Ansible Role to manage cloud compute resources.

Requirements
------------

* ansible >= 2.10

Provider=AWS:

* boto3


Role Variables
--------------

`compute_resources`: List of compute resources to be created. Only `machine` is supported.

<!--

Dependencies
------------

> TBD

-->


Usage
-----

- Create the Playbook `create-compute.yaml`:
```yaml
---
- hosts: localhost
  vars:
    compute_resources:
      - provider: "{{ provider }}"
        type: machine
        user_data: ""
        vpc_subnet_id: "subnet-1234567"
        instance_type: m5.xlarge
        image_id: ami-123456789
        filters:
          tag:Name: "{{ name }}"
          instance-state-name: running
        tags:
          Name: "{{ name }}"

  roles:
    - role: mtulio.cloud-compute
```

- Run the Playbook:

```bash
ansible-playbook create.yaml -e provider=aws -e name=my-machine
```

License
-------

Apache v2

Author Information
------------------

[Marco Tulio R Braga](https://github.com/mtulio)
