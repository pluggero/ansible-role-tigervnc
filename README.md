# Ansible Role: TigerVNC

[![CI](https://github.com/pluggero/ansible-role-tigervnc/actions/workflows/ci.yml/badge.svg)](https://github.com/pluggero/ansible-role-tigervnc/actions/workflows/ci.yml) [![Ansible Galaxy downloads](https://img.shields.io/ansible/role/d/pluggero/tigervnc?label=Galaxy%20downloads&logo=ansible&color=%23096598)](https://galaxy.ansible.com/ui/standalone/roles/pluggero/tigervnc)

An Ansible Role that installs TigerVNC on various Linux distributions.

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
tigervnc_install_method: "package"
```

The method used to install tigervnc can be defined in the variable `tigervnc_install_method`.
The following methods are available:

- `package`: Installs tigervnc from the package manager of the distribution
  - **NOTE**: This method installs the latest version available in the package manager and not the version defined in `tigervnc_version`.

## Dependencies

None.

## Example Playbook

```yaml
- hosts: all
  roles:
    - pluggero.tigervnc
```

## License

MIT / BSD

## Author Information

This role was created in 2025 by Robin Plugge.
