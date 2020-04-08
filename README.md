[![Build Status](https://travis-ci.org/open-io/ansible-role-openio-openio_service.svg?branch=master)](https://travis-ci.org/open-io/ansible-role-openio-openio_service)
# Ansible role `openio_service`

An Ansible role for install openio_service. Specifically, the responsibilities of this role are to:

- install and configure openio_service

## Requirements

- Ansible 2.9+

## Role Variables

| Variable   | Default | Comments (type)  |
| :---       | :---    | :---             |
| `openio_service_namespace` | `"{{ namespace \| default('OPENIO') }}"` | OpenIO namespace |
| `openio_service_maintenance_mode` | `"{{ openio_maintenance_mode \| d(false) }}"` | Maintenance mode|
| `openio_service_id` | `0` | Service ID |
| `openio_service_name` | `"{{ openio_service_type }}-{{ openio_service_id }}"` | Service name |
| `openio_service_gridinit_service_name` | `"{{ openio_service_namespace }}-{{ openio_service_name }}"` | Gridinit service name|
| `openio_service_gridinit_dir` | `"/etc/gridinit.d/{{ openio_service_namespace }}"` | Gridinit directory |
| `openio_service_gridinit_file_prefix` | `""` | Gridinit prefix |
| `openio_service_inventory_dir` | `"/etc/oio/sds/{{ openio_service_namespace }}/inventory"` | Inventory directory |
| `openio_service_inventory_file` | `"{{ openio_service_inventory_dir }}/{{ openio_service_name }}.yml"` | Service inventory_file|
| `openio_service_package_upgrade` | `"{{ openio_package_upgrade \| d(false) }}"` | Do we upgrade package during deployment ? |
| `openio_service_conf_dir` | `"/etc/oio/sds/{{ openio_service_namespace }}/{{ openio_service_name }}"` | Service configuration directory |
| `openio_service_log_dir` | `"/var/log/oio/sds/{{ openio_service_namespace }}/{{ openio_service_name }}"` | Service log directory |
| `openio_service_volume` | `"/var/lib/oio/sds/{{ openio_service_namespace }}/{{ openio_service_name }}"` | Service volume directory (data) |
| `openio_service_syslog_tag` | `"OIO,{{ openio_service_namespace }},{{ openio_service_type }},{{ openio_service_id }}"` | Service syslog tag|
| `openio_service_gridinit_default_group` | `"{{ [openio_service_namespace,openio_service_type,openio_service_id] \| join(',') }}"` | Service gridinit default groups |
| `openio_service_sysctl_priority` | `98` | sysctl file priority |
| `openio_service_sysctl_file` | `"/etc/sysctl.d/{{ openio_service_sysctl_priority }}-{{ openio_service_type }}.conf"` | sysctl configuration file |
| `openio_service_state` | `present` | Service state |
| `openio_service_packages` | `[]` | Service packages |
| `openio_service_directories` | `[]` | Service directories |
| `openio_service_configuration_files` | `[]` | Service configuration files |
| `openio_service_services` | `[]` | Gridinit services to launch |
| `openio_service_checks` | `[]` | Service Checks to run at the end |

## Dependencies
- https://github.com/open-io/ansible-role-gridinit

## Example Playbook

```yaml
- hosts: all
  gather_facts: true
  become: true

  tasks:
    - include_role:
        name: openio_service
      vars:
        openio_service_type: "myservice"
        openio_service_packages:
          - myservice
        openio_service_directories:
          - path: "{{ openio_service_conf_dir }}/foo"
        openio_service_configuration_files:
          - name: myservice.yml
          - name: foo/bar..yml
        openio_service_services:
          - command: "/urs/bin/myservice -c {{ openio_service_conf_dir }}/myservice.yml"
        openio_service_checks:
          - uri:
              url: "http://{{ ansible_default_ipv4.address }}:4242/ping"
```

## Contributing

Issues, feature requests, ideas are appreciated and can be posted in the Issues section.

Pull requests are also very welcome.
The best way to submit a PR is by first creating a fork of this Github project, then creating a topic branch for the suggested change and pushing that branch to your own fork.
Github can then easily create a PR based on that branch.

## License
AGPLv3 - Copyright (C) 2015-2020 OpenIO SAS
