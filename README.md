# Ansible Role: TigerVNC

[![CI](https://github.com/pluggero/ansible-role-tigervnc/actions/workflows/ci.yml/badge.svg)](https://github.com/pluggero/ansible-role-tigervnc/actions/workflows/ci.yml) [![Ansible Galaxy downloads](https://img.shields.io/ansible/role/d/pluggero/tigervnc?label=Galaxy%20downloads&logo=ansible&color=%23096598)](https://galaxy.ansible.com/ui/standalone/roles/pluggero/tigervnc)

An Ansible Role that installs and configures TigerVNC on various Linux distributions with support for multiple concurrent user sessions.

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

### Installation Settings

```yaml
tigervnc_version: "x.x.x"
```

The version of TigerVNC to install.

```yaml
tigervnc_install_method: "dynamic"
```

The method used to install TigerVNC. The following methods are available:

- `source`: Installs TigerVNC from source
- `package`: Installs TigerVNC from the package manager of the distribution
  - **NOTE**: This method installs the latest version available in the package manager and not the version defined in `tigervnc_version`.
  - In distributions where TigerVNC is not available in the package manager, it will be installed from source.
- `dynamic`: Installs TigerVNC from package manager if available in the correct version, otherwise installs from source

### Multi-User Configuration

```yaml
tigervnc_users: []
```

A list of users for which VNC sessions will be configured. Each user can have the following properties:

- `username` (required): The system username
- `display` (required): The VNC display number (e.g., `1` for `:1`, `2` for `:2`)
- `password` (optional): VNC password for the session. **Must be 6-8 characters** (VNC protocol limitation - only the first 8 characters are used due to DES encryption)
- `session_command` (optional): The desktop session or application to start (default: `xterm`)
- `localhost` (optional): Listen only on localhost (default: `true`)
- `securitytypes` (optional): Security types to use (default: `none`)
- `alwaysshared` (optional): Allow shared connections (default: `true`)
- `config_additional` (optional): Additional configuration lines (default: `[]`)
- `service_enabled` (optional): Whether to enable the systemd service (default: `false`)

### Default Values

```yaml
tigervnc_default_session_command: "xterm"
tigervnc_default_localhost: true
tigervnc_default_securitytypes: "none"
tigervnc_default_alwaysshared: true
tigervnc_default_service_enabled: false
```

Default values applied to user configurations when not explicitly specified.

## Dependencies

None.

## Example Playbook

### Basic Multi-User Setup

```yaml
- hosts: all
  become: true
  vars:
    tigervnc_users:
      - username: alice
        display: 1
        session_command: "xfce4-session"
        service_enabled: true # Service will be enabled and started
      - username: bob
        display: 2
        session_command: "gnome-session"
        service_enabled: false # Service will be disabled and stopped
      - username: charlie
        display: 3
        session_command: "xterm"
        # service_enabled not specified - uses default (false)
  roles:
    - pluggero.tigervnc
```

## How It Works

This role:

1. Installs TigerVNC binaries and required dependencies
2. Creates VNC configuration for each user in their home directory (`~/.config/tigervnc/`)
3. Maps users to display numbers in `/etc/tigervnc/vncserver.users`
4. Deploys the systemd template service (`vncserver@.service`)
5. Manages service state per user based on the `service_enabled` flag

Each user gets:

- Their own VNC configuration in `~/.config/tigervnc/config`
- Their own startup script in `~/.config/tigervnc/xstartup`
- Their own password file (if password is set) in `~/.config/tigervnc/passwd`
- Their own systemd service instance

## Connecting to VNC Sessions

After configuration, users can connect to their VNC sessions:

- Display `:1` → Port `5901`
- Display `:2` → Port `5902`
- Display `:N` → Port `590N`

Example connection:

```bash
vncviewer hostname:1  # Connect to user on display :1
vncviewer hostname:2  # Connect to user on display :2
```

## License

MIT / BSD

## Author Information

This role was created in 2025 by Robin Plugge.
