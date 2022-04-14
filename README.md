# ansible-role-cloud-compute

Ansible Role to manage Cloud Compute Resources.


Requirements
------------

All Providers:

* ansible >= 2.3

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

- Create the var file `vars/compute/k8s-masters.yaml` to create one EC2 named `k8s-master-01`:
```yaml
# https://docs.ansible.com/ansible/latest/collections/amazon/aws/ec2_instance_module.html
compute_resources:
    # Module 'machine' options:
    # https://docs.ansible.com/ansible/latest/collections/amazon/aws/ec2_instance_module.html
    - provider: aws
      type: machine
      name: master-01
      filters:
        tag:Name: 'k8s-master-01'
      tags: "{% set x=_def.tags.__setitem__('Name','k8s-master-01') %}{{ _def.tags }}"

      detailed_monitoring: "{{ _def.detailed_monitoring }}"
      ebs_optimized: "{{ _def.ebs_optimized }}"
      image_id: "{{ _def.image_id }}"
      instance_role: "{{ _def.instance_role }}"
      instance_type: "{{ _def.instance_type }}"
      security_groups: "{{ _def.security_groups }}"
      state: "{{ _def.state }}"

      termination_protection: "{{ _def.termination_protection }}"
      #user_data: x
      #volumes: []
      vpc_subnet_id: "{{ _def.vpc_subnet_id }}"
      wait: "{{ _def.wait }}"
      wait_timeout: "{{ _def.wait_timeout }}"

```

- Create the Playbook `create-compute.yaml`:
```yaml
---
- hosts: localhost
  vars_prompt:
    - name: provider
      prompt: What is the cloud provider name?
      private: no
    - name: name
      prompt: What is the network name?
      private: no

  pre_tasks:
    - include_vars: "vars/compute/{{ name }}-{{ provider }}.yaml"

  roles:
    - role: mtulio.cloud-compute
```

- Run the Playbook:

```bash
ansible-playbook create-compute-aws.yaml -e cloud=aws -e name=k8s-masters
```

License
-------

Apache v2

Author Information
------------------

[Marco Tulio R Braga](https://github.com/mtulio)
