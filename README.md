# ansible-role-cloud-compute

Ansible Role to manage Cloud Compute Resources.

Requirements
------------

All Providers:

* ansible >= 2.10

Provider=AWS:

* boto3


Role Variables
--------------

`cloud_networks`: List of networks to be created on the Cloud Provider.

Dependencies
------------

> TBD

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
