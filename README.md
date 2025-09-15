# Ansible Tmux

This role installs and copies over user specific configuration files for the terminal multiplexer Tmux.

## Requirements

This role requires three separate tools be installed.

First it requires `git` to be installed and on the path.

Second it requires the `ansible.utils` collection be installed from Ansible-Galaxy via:

```bash
ansible-galaxy collection install ansible.utils
```

Third it requires the `jsonschema` Python package be installed.

This can be done via `pip`:

```bash
pip install jsonschema
```

## Role Variables

All parameters are optional unless otherwise stated.

```yaml
tmux:
  remove:       [bool]    Indicates if tmux should be removed.
  config:
    - user:     [string]  [required] The name of the user that the config should be copied.
      file:     [string]  [required] The role path to the config file to copy.
      remove:   [bool]    Indicates if the config for the [user] should be removed.
      plugins:
        remove: [bool]    Indicates if the tmux package manager should be removed. If not set or set to false then TPM will be installed.
```

### Defaults

The config and plugins will be placed under the path defined by variable `tmux_default_user_config_path`. This path will always be in relation to the user's home directory. By default the path is `.config/tmux`.


## Dependencies

None.

## Setup

Before the role can be used it needs to be added to the machine running the playbook, and as of writing this, this role is not hosted on Ansible-Galaxy only on Github.

1. Create a `requirements.yml` file in the root directory of the playbook being worked on.

2. Add the following definition inside the `requirements.yml` file:

```yml
- name: hth-tmux
  src: https://github.com/hrafnthor/ansible-tmux.git
  scm: git
```

3. Install the requirements by executing

```shell
ansible-galaxy install -r .requirements.yml
```

This will allow any playbook run from this machine to use the role `hth-tmux`


## Example Playbook


```yaml
- hosts: all
    vars:
      tmux:
        remove: false
        config:
          - user: odinn
            file: /software/config/tmux/odinn.tmux.conf
            plugins:
              remove: false
          - user: baldur
            file: /software/config/tmux/baldur.tmux.conf
            plugins:
              remove: false
  roles:
     - hth-tmux
```

## License

```
Copyright 2025 Hrafn Thorvaldsson

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
