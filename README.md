# Ansible Role: Grafana [![Build Status](https://travis-ci.org/manala/ansible-role-grafana.svg?branch=master)](https://travis-ci.org/manala/ansible-role-grafana)

This role will deal with the configuration of __grafana__.

It's part of the Manala <a href="http://www.manala.io" target="_blank">Ansible stack</a> but can be used as a stand alone component.


## Requirements

None.

## Dependencies

None.

## Installation

Using ansible galaxy:

```bash
ansible-galaxy install manala.grafana
```

Using ansible galaxy requirements file:

```yaml
- src: manala.grafana
```

## Role Handlers

| Name              | Type    | Description            |
| ----------------- | ------- | ---------------------- |
| `grafana restart` | Service | Restart grafana server |

## Role Variables

| Name                                 | Default                  | Type   | Description |
| ------------------------------------ | ------------------------ | ------ | ----------- |
| manala_grafana_config_file           | /etc/grafana/grafana.ini | string |             |
| manala_grafana_config_template       | config/base.ini.j2       | string |             |
| manala_grafana_config                | []                       | Array  |             |
| manala_grafana_api_url               | http://127.0.0.1:3000    | string |             |
| manala_grafana_api_user              | admin                    | string |             |
| manala_grafana_api_password          | admin                    | string |             |
| manala_grafana_datasources_exclusive | false                    | bool   | Remove old datasources |
| manala_grafana_datasources           | []                       | Array  |             |
| manala_grafana_dashboards_exclusive  | false                    | bool   | Remove old dashboards |
| manala_grafana_dashboards            | []                       | Array  |             |

### Configuration example

See : http://docs.grafana.org/installation/configuration/

```yaml
manala_grafana_config:
  - app_mode: production
  - server:
    - http_port: 3001
  - security:
    - admin_user: admin
    - admin_password: admin

manala_grafana_api_url: http://127.0.0.1:3000
manala_grafana_api_user: admin
manala_grafana_api_password: admin

manala_grafana_datasources_exclusive: true
manala_grafana_datasources:
  - name:      telegraf
    type:      influxdb
    isDefault: true
    access:    proxy
    basicAuth: false
    url:       http://localhost:8086
    database:  telegraf
    username:  ''
    password:  ''

manala_grafana_dashboards_exclusive: true
manala_grafana_dashboards:
    - template: "{{ playbook_dir }}/templates/grafana/dashboards/system.json"
      inputs:
        - name:     "DS_TELEGRAF"
          pluginId: "influxdb"
          type:     "datasource"
          value:    "telegraf"
      overwrite: true
```

## Example playbook

```yaml
- hosts: servers
  roles:
    - { role: manala.grafana }
```

# Licence

MIT

# Author information

Manala [**(http://www.manala.io/)**](http://www.manala.io)
