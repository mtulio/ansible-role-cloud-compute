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


Usage Examples
--------------

### Provider: AWS

- Install Dependencies

```bash
pip3 install boto3
ansible-galaxy install collection community.aws
ansible-galaxy install collection amazon.aws
```

- Create the playbook file `create.yaml` to create EC2 Instance on AWS:

```yaml
- hosts: localhost
  vars:
    compute_resources:
      - provider: "aws"
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

### Provider: Digital Ocean

- Install Dependencies

```bash
ansible-galaxy collection install community.digitalocean
```

- Export variables

```bash
export SSH_KEY=$(cat ~/.ssh/id_rsa.pub)
```

- Create the Playbook
```yaml
- hosts: localhost
  vars:
    compute_resources:
      # Module 'machine' options:
      - provider: do
        type: machine
        name: "{{ name }}"
        state: present
        user_data: "<CHANGE_ME_USERDATA>"
        vpc_name: "my-vpc-name"
        wait: "yes"
        wait_timeout: 500
        image_name: "fedora-coreos-34.20210626.3.1-digitalocean.x86_64.qcow2.gz"
        private_networking: yes
        project_name: "my-project"
        region: "NYC3"
        size: "s-4vcpu-8gb"
        ssh_key:
          name: "my-key"
          pub_key: "{{ lookup('ansible.builtin.env', 'SSH_KEY') }}"

  roles:
    - role: mtulio.cloud-compute
```

- Run the Playbook:

```bash
ansible-playbook create.yaml -e provider=do -e name=my-machine
```

License
-------

Apache v2

Author Information
------------------

[Marco Tulio R Braga](https://github.com/mtulio)
