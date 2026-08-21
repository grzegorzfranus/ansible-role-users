# Ansible Role: Users

|Source|Version|CI|License|
|------|-------|--|-------|
|[![Source Code](https://img.shields.io/badge/source-github-blue.svg)](https://github.com/grzegorzfranus/ansible-role-users)|[![Version](https://img.shields.io/github/v/release/grzegorzfranus/ansible-role-users)](https://github.com/grzegorzfranus/ansible-role-users/releases)|[![CI](https://github.com/grzegorzfranus/ansible-role-users/actions/workflows/ci.yml/badge.svg)](https://github.com/grzegorzfranus/ansible-role-users/actions/workflows/ci.yml)|[![Repository License](https://img.shields.io/badge/license-apache2.0-brightgreen.svg)](LICENSE)|

This Ansible role manages user accounts on Linux systems. It provides capabilities for creating users with custom home directories, shells, comments, password policies, per-user sudoers privilege drop-in files in `/etc/sudoers.d/`, and SSH authorized keys management.

## ✨ Features

- 👥 **User Account Management**: Create, modify, and remove user accounts with full control
- 🛡️ **Per-User Sudoers**: Validated `/etc/sudoers.d` drop-in files generated per user (boolean shorthand or custom rules)
- 🔐 **Secure Password Generation**: Cryptographically secure random password creation
- 🔑 **SSH Key Management**: Automated SSH authorized_keys deployment and management
- 🏠 **Home Directory Control**: Flexible home directory creation, customization, and cleanup
- 👤 **User Profiles**: Complete user configuration including shell, groups, and permissions
- 🛡️ **Password Policies**: Configurable password aging and expiration policies
- 📊 **System Integration**: Seamless integration with system groups and permissions
- 🔄 **Idempotent Operations**: Safe execution with no unintended changes on re-runs
- 📝 **Comprehensive Logging**: Detailed operation logging with security-conscious output
- 🚀 **CI/CD Integration**: Full Molecule test suite for automated testing

## 🎯 Main Actions

- Create and manage users with customizable parameters
- Generate validated per-user sudoers drop-in files in `/etc/sudoers.d/`
- Securely generate random passwords
- Manage SSH authorized keys for users
- Set password expiration policies
- Remove users with optional home directory and sudoers drop-in cleanup

## 📋 Requirements

- **Ansible**: 2.17 or higher
- **Python**: 3.9 or higher on target hosts
- **Privileges**: sudo/root access on target hosts

### Supported operating systems
List of officially supported operating systems for this role:

| OS Family | Version | Status |
|-----------|---------|---------|
| Ubuntu | 26.04 (Resolute) | ![✓](https://img.shields.io/badge/✓-brightgreen.svg) |
| Ubuntu | 24.04 (Noble) | ![✓](https://img.shields.io/badge/✓-brightgreen.svg) |
| Ubuntu | 22.04 (Jammy) | ![✓](https://img.shields.io/badge/✓-brightgreen.svg) |
| Debian | 13 (Trixie) | ![✓](https://img.shields.io/badge/✓-brightgreen.svg) |
| Debian | 12 (Bookworm) | ![✓](https://img.shields.io/badge/✓-brightgreen.svg) |
| Debian | 11 (Bullseye) | ![✓](https://img.shields.io/badge/✓-brightgreen.svg) |
| EL (RHEL, Rocky, Alma, Oracle) | 9 | ![✓](https://img.shields.io/badge/✓-brightgreen.svg) |
| EL (RHEL, Rocky, Alma, Oracle) | 8 | ![✓](https://img.shields.io/badge/✓-brightgreen.svg) \* |

> \* **EL8 Compatibility Constraints**:
>
> - **Ansible & Python compatibility**: EL8 defaults to Python 3.6. Support for target Python 3.6 was dropped in `ansible-core` >= 2.17.
>   - To run on EL8 with the system's default Python 3.6, you must use `ansible-core` <= 2.16.
>   - If running `ansible-core` >= 2.17, EL8 targets require Python >= 3.7. However, the system `python3-dnf` package manager bindings on EL8 are compiled exclusively for Python 3.6 and will not be available on newer Python interpreters. This will cause tasks using the `dnf` module (such as installing packages or user management configurations that rely on it) to fail.
> - **Molecule Testing**: Due to these Python version compatibility constraints, EL8 is not officially tested in the role's Molecule test suite (which runs a newer Ansible version in CI).

### Ansible version

Ansible >= 2.17 (tested up to 2.21)

### Python version

Python >= 3.9

### Python dependencies

The following Python modules are required:

| Module | Purpose | Installation |
|--------|---------|-------------|
| `passlib` | Required for secure password hashing | `pip install passlib` |

### Setup module
The role uses facts gathered by Ansible on the remote host. If you disable the Setup module in your playbook, the role will not work properly.

### Root access
This role requires root access for user management tasks. Make sure you are using a user with root privileges.

## 🚀 Quick Start

### 1. Basic User Creation with Sudo Access

```yaml
---
- name: Create System Users
  hosts: all
  become: true
  roles:
    - role: grzegorzfranus.users
      vars:
        users_manage_sudoers: true
        users_dict:
          johndoe:
            comment: "John Doe"
            groups: ["sudo"]
            sudoers: true
```

### 2. Run the playbook

```bash
ansible-playbook -i inventory playbook.yml
```

## ⚙️ Configuration

### Default Configuration

The role is pre-configured with secure and sensible defaults:

```yaml
# Role execution control (Options: 'all', 'create', 'remove')
users_role_action: "all"

# Shell configuration
users_default_shell: "/bin/bash"

# Password aging policy
users_password_max_age: 90
users_password_min_age: 0

# SSH key management
users_manage_ssh_keys: false

# Sudoers management
users_manage_sudoers: false
users_sudoers_directory: "/etc/sudoers.d"
users_sudoers_file_mode: "0440"
users_sudoers_validate: true
users_sudoers_purge_disabled: true
```

## 📌 Role Properties

| Property | Value | Description |
|----------|-------|-------------|
| **Idempotent** | ✅ Yes | Running the role multiple times only modifies users if their state/details change. |
| **Atomic** | ❌ No | Users are created/removed sequentially. A failure mid-run may leave users partially managed. |
| **Check Mode** | ✅ Supported | All user management actions support check mode simulation. |
| **Diff Mode** | ✅ Supported | Configuration templates and files support diff mode preview. |

## 📤 Role Output

This role does not set any public output facts. All internal facts use the double-underscore prefix.

## 📊 Variables

### General Settings

| Variable | Description | Default |
|----------|-------------|---------|
| `users_role_action` | Role execution control (Options: `'all'`, `'create'`, `'remove'`) | `"all"` |
| `users_create_home` | Whether to create home directories for new users | `true` |
| `users_remove_home` | Whether to remove home directories when removing users | `true` |

### User Default Configuration

| Variable | Description | Default |
|----------|-------------|---------|
| `users_default_shell` | Default shell for new users | `"/bin/bash"` |
| `users_password_max_age` | Maximum password age before requiring change (days) | `90` |
| `users_password_min_age` | Minimum password age before allowing change (days) | `0` |
| `users_create_group` | Create a group with the same name as the user | `true` |
| `users_default_groups` | Additional groups to add users to | `[]` |
| `users_append_groups` | Append to groups instead of replacing them | `true` |
| `users_overwrite_existing` | Overwrite attributes for existing users (password, shell, groups, etc.) | `false` |

### Password Generation Settings

| Variable | Description | Default |
|----------|-------------|---------|
| `users_generate_password` | Generate random passwords for new users | `true` |
| `users_password_length` | Length of generated passwords | `16` |
| `users_password_store_file_name` | Filename for storing passwords on Ansible controller | Template with hostname and timestamp |
| `users_store_passwords_controller` | Whether to store passwords on Ansible controller | `false` |
| `users_password_store_remote_path` | Path to store generated passwords on remote hosts | `""` |
| `users_store_passwords_remote` | Whether to store passwords on remote hosts | `false` |
| `users_password_chars` | Character set used for password generation | `"abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*()-_=+[]{}|;:,.<>?"` |
| `users_password_hash_algorithm` | Password hashing algorithm (`sha512`, `sha256`) | `"sha512"` |

### SSH Key Management

| Variable | Description | Default |
|----------|-------------|---------|
| `users_manage_ssh_keys` | Whether to manage SSH keys for users | `false` |
| `users_ssh_key_type` | Default SSH key type (`rsa`, `ed25519`, `ecdsa`, `dsa`) | `"ed25519"` |
| `users_ssh_key_bits` | Key size for RSA keys (`1024`, `2048`, `4096`) | `4096` |
| `users_ssh_directory` | Path to SSH directory within user home | `".ssh"` |
| `users_ssh_authorized_keys_file` | Path to authorized_keys file | `".ssh/authorized_keys"` |
| `users_ssh_directory_mode` | Permissions for .ssh directory | `"0700"` |
| `users_ssh_authorized_keys_mode` | Permissions for authorized_keys file | `"0600"` |

### Sudoers Management

| Variable | Description | Default |
|----------|-------------|---------|
| `users_manage_sudoers` | Master switch for per-user sudoers drop-in file management | `false` |
| `users_sudoers_directory` | Directory holding per-user sudoers drop-in files | `"/etc/sudoers.d"` |
| `users_sudoers_file_prefix` | Optional filename prefix controlling include order (e.g. `"10-"`) | `""` |
| `users_sudoers_file_mode` | Permissions of generated drop-in files (`"0440"`, `"0400"`) | `"0440"` |
| `users_sudoers_default_rule` | Privilege specification used when a user only enables sudoers without custom rules | `"ALL=(ALL) NOPASSWD:ALL"` |
| `users_sudoers_validate` | Validate every rendered file with `visudo -cf %s` before installing | `true` |
| `users_sudoers_purge_disabled` | Remove drop-in file when user sudoers block is disabled or absent | `true` |

### Internal Constants (System Paths and Commands)

| Variable | Description | Default |
|----------|-------------|---------|
| `__users_home_base` | Base directory for regular user homes | `"/home"` |
| `__users_system_home_base` | Base directory for system user homes | `"/var/lib"` |
| `__users_pam_config_dir` | PAM configuration directory | `"/etc/pam.d"` |
| `__users_login_defs_path` | Path to login.defs file | `"/etc/login.defs"` |
| `__users_chage_command` | Command to set account expiration | `"chage"` |
| `__users_lock_command` | Command to lock user accounts | `"usermod --lock"` |
| `__users_unlock_command` | Command to unlock user accounts | `"usermod --unlock"` |

### User Dictionaries

| Variable | Description | Default |
|----------|-------------|---------|
| `users_dict` | Dictionary of users to create/manage | `{}` |
| `users_remove_dict` | Dictionary of users to remove | `{}` |

## 👤 User Parameters

Each user in the `users_dict` can have the following parameters:

| Parameter | Description | Default |
|-----------|-------------|---------|
| `name` | Username (automatically set from the dictionary key) | (required) |
| `comment` | User description (GECOS field) | (empty) |
| `uid` | User ID | (auto assigned) |
| `group` | Primary group | (same as username) |
| `groups` | List of supplementary groups | `users_default_groups` |
| `append` | Whether to append groups or replace them | `users_append_groups` |
| `shell` | User's login shell | `users_default_shell` |
| `home` | Home directory path | `/home/username` |
| `create_home` | Whether to create home directory | `users_create_home` |
| `move_home` | Whether to move home if it exists with different path | `false` |
| `system` | Whether to create a system user | `false` |
| `state` | Whether user should exist | `present` |
| `password` | User password (will be hashed) | (generated if not provided) |
| `update_password` | When to update password (always, on_create) | `on_create` |
| `force` | Overwrite existing user attributes when user already exists | `users_overwrite_existing` |
| `password_expire_max` | Maximum password age in days | `users_password_max_age` |
| `password_expire_min` | Minimum password age in days | `users_password_min_age` |
| `expires` | Account expiration date (YYYY-MM-DD) | (never expires) |
| `ssh_keys` | List of SSH authorized keys | (none) |
| `sudoers` | Per-user sudoers configuration (boolean shorthand or dictionary) | (none) |
| `remove` | Whether to remove home dir when removing user | `false` |
| `no_log` | Whether to suppress logging of task | `true` |
| `password_length` | Custom length for generated password | `users_password_length` |

## ♻️ Overwrite Behavior for Existing Users

By default, the role does not modify existing users. This means their password, shell, groups, and other attributes remain unchanged unless you explicitly enable overwriting:

- Global toggle: set `users_overwrite_existing: true` to allow updates for all existing users
- Per-user override: set `force: true` within a specific user entry in `users_dict`

When neither is set, only new users are created and existing users are left intact. Password updates use `update_password: on_create` unless overwrite is enabled for the user.

## 🔐 Per-User Sudoers Management

When `users_manage_sudoers: true` is enabled, the role creates drop-in configuration files in `/etc/sudoers.d/` for users configured in `users_dict`.

### Configuration Formats

1. **Boolean Shorthand**: Set `sudoers: true` to generate a standard passwordless full sudo rule, or `sudoers: false` to disable sudoers for that user:
   ```yaml
   users_dict:
     admin:
       comment: "System Administrator"
       sudoers: true
     guest:
       comment: "Guest Account"
       sudoers: false
   ```
   Renders file `/etc/sudoers.d/admin`:
   ```text
   # Ansible managed
   admin ALL=(ALL) NOPASSWD:ALL
   ```

2. **Full Dictionary Specification**: Use a dictionary for granular control over rules, defaults, and aliases:
   ```yaml
   users_dict:
     deployer:
       comment: "CI/CD Deployment Service Account"
       sudoers:
         enabled: true
         state: present
         comment: "Restricted service management"
         extra_lines:
           - "Cmnd_Alias NGINX_OPS = /usr/bin/systemctl restart nginx, /usr/bin/systemctl reload nginx"
         defaults:
           - "!requiretty"
         rules:
           - "ALL=(root) NOPASSWD: NGINX_OPS"
           - "ALL=(root) NOPASSWD: /usr/bin/systemctl status nginx"
   ```
   Renders file `/etc/sudoers.d/deployer`:
   ```text
   # Ansible managed
   # Restricted service management
   Cmnd_Alias NGINX_OPS = /usr/bin/systemctl restart nginx, /usr/bin/systemctl reload nginx
   Defaults:deployer !requiretty
   deployer ALL=(root) NOPASSWD: NGINX_OPS
   deployer ALL=(root) NOPASSWD: /usr/bin/systemctl status nginx
   ```

3. **Disabling and Purging Drop-Ins**: Set `enabled: false`, `state: absent`, or `sudoers: false` to purge existing drop-in files:
   ```yaml
   users_dict:
     legacyuser:
       sudoers:
         enabled: false
   ```

### Sudoers Dictionary Schema

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `enabled` | `bool` | `true` | Master toggle for generating the drop-in file for this user |
| `state` | `str` | `"present"` | State of the drop-in file (`"present"` or `"absent"`) |
| `rules` | `list[str]` | `[users_sudoers_default_rule]` | List of sudo privilege specifications appended after username (must be a list of strings) |
| `defaults` | `list[str]` | `[]` | List of sudo Defaults entries rendered as `Defaults:<username> <entry>` (must be a list of strings) |
| `extra_lines` | `list[str]` | `[]` | List of raw lines (aliases, comments) rendered before Defaults and rules (must be a list of strings) |
| `comment` | `str` | `""` | Optional comment rendered in the drop-in file header |

### Security, Directory Normalization & Naming Constraints

- **Directory Ownership & Normalization**: The role automatically normalizes ownership and permissions of the drop-in directory (`users_sudoers_directory`) to `root:root 0750`.
- **Permissions**: Drop-in files are created with mode `0440` (or `0400`) owned by `root:root`. Sudo ignores files with insecure permissions.
- **Validation**: All rendered files are automatically checked with `visudo -cf %s` before deployment (`users_sudoers_validate: true`).
- **Backup Copies**: The role runs with `backup: true`, writing rotated copies into `/etc/sudoers.d/` on change. Sudo safely ignores these files because their names contain dots and end with `~`, though they accumulate over time unless cleaned up.
- **Filename Validation**: The combined filename (`users_sudoers_file_prefix ~ username`) must not contain a dot (`.`), as sudo strictly ignores files containing dots.
- **Injection Protection**: Runtime assertions verify that `rules`, `defaults`, and `extra_lines` are lists of strings and do not contain embedded newline characters.

## 🔐 Secure Password Management

This role implements several security best practices for password management:

1. **Random Password Generation**: Uses Ansible's `password` lookup plugin to generate cryptographically secure random passwords.
2. **Secure Hashing**: All passwords are hashed using the configured algorithm (`sha512` by default) with a random salt.
3. **No Plain-Text Storage**: Generated passwords are not stored in variables longer than necessary.
4. **No Logging**: Password operations use `no_log: true` to prevent password exposure in logs.
5. **Flexible Storage Options**: Passwords can be stored on either the Ansible controller, remote hosts, or both.

## 📝 Retrieving Generated Passwords

Note: Password files are created only when at least one password is generated (i.e., when creating new users or when overwriting existing users with `force: true` or `users_overwrite_existing: true`).

When `users_generate_password` is enabled, passwords can be stored in the following ways:

### On the Ansible Controller

When `users_store_passwords_controller` is set to `true` and `users_password_store_file_name` is defined, passwords will be stored in a file in the playbook directory on the Ansible controller.

By default, the filename includes the hostname and a timestamp to ensure uniqueness:
```
hostname_YYYY-MM-DD_HH-MM-SS
```

For example: `webserver1_2023-11-15_14-30-22`

### On Remote Hosts

When `users_store_passwords_remote` is set to `true` and `users_password_store_remote_path` is defined, passwords will be stored on each remote host. The filename will include the hostname: `{users_password_store_remote_path}_{hostname}`.

### Password File Format

All password files have the following format:

```
# Generated Passwords - 2023-05-01T10:15:30Z
# Host: server1.example.com
User: johndoe Password: Tb8y6tG$jK2p!9Lm
User: appuser Password: W4e@7Jh*zPq5$3xN
```

All password files have strict permissions (0600) and on remote hosts are owned by root.

### Security Considerations

For maximum security:
- Use Ansible Vault to encrypt variables containing paths
- Retrieve password files after user creation
- Store them securely (e.g., in a password manager)
- Delete them from both the Ansible controller and remote hosts

## 🏷️ Role Tags

The role uses the following tags for task selection:

| Tag | Description |
|-----|-------------|
| `always` | Tasks that always run (variable loading, validation) |
| `setup` | User account setup and creation tasks |
| `init` | Initial environment setup |
| `validate` | Variable validation tasks |
| `check` | Verification and checking tasks |
| `create` | Tasks related to user creation |
| `users` | All user-related tasks (creation, removal, sudoers) |
| `sudoers` | Per-user sudoers drop-in file management |
| `configure` | Sudoers and user configuration tasks |
| `cleanup` | Tasks for user account removal |
| `remove` | Tasks specifically for removing users |

These tags can be used to selectively run parts of the role, for example:

```bash
# Only run user creation tasks
ansible-playbook playbook.yml --tags "create"

# Only manage sudoers drop-in files
ansible-playbook playbook.yml --tags "sudoers"

# Skip user removal tasks
ansible-playbook playbook.yml --skip-tags "remove,cleanup"
```

## 📖 Example Playbooks

### Creating Users with Dictionary Format

```yaml
- hosts: all
  become: true
  roles:
    - role: grzegorzfranus.users
      users_dict:
        johndoe:
          comment: "John Doe"
          groups: ["wheel", "developers"]
          # Password will be auto-generated

        janedoe:
          comment: "Jane Doe"
          shell: "/bin/zsh"
          password: "secure_password"  # Will be properly hashed
          home: "/opt/custom/janedoe"

        devuser:
          comment: "Developer User"
          groups: ["developers"]
          ssh_keys:
            - "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPM4xxxxxxxxxx user@example.com"
            - "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC/xxxxxxxx user@example.com"
```

### Granting Restricted Sudo Privileges

```yaml
- hosts: all
  become: true
  roles:
    - role: grzegorzfranus.users
      vars:
        users_manage_sudoers: true
        users_dict:
          admin:
            comment: "System Administrator"
            sudoers: true  # Full passwordless sudo

          deployer:
            comment: "CI/CD Service Account"
            sudoers:
              comment: "Restricted systemd lifecycle operations"
              extra_lines:
                - "Cmnd_Alias NGINX_OPS = /usr/bin/systemctl restart nginx, /usr/bin/systemctl reload nginx"
              defaults:
                - "!requiretty"
              rules:
                - "ALL=(root) NOPASSWD: NGINX_OPS"
                - "ALL=(root) NOPASSWD: /usr/bin/systemctl status nginx"
```

### Removing Users with Dictionary Format

```yaml
- hosts: all
  become: true
  roles:
    - role: grzegorzfranus.users
      vars:
        users_role_action: "remove"
        users_manage_sudoers: true
        users_remove_dict:
          olduser:
            remove_home: true
          tempuser: {}  # Use default settings
```

### Basic Example with Password Storage in Current Directory

```yaml
- hosts: all
  become: true
  roles:
    - role: grzegorzfranus.users
      vars:
        # Enable password storage on controller (using default filename with timestamp)
        users_store_passwords_controller: true

        # Define users to create
        users_dict:
          admin:
            comment: "System Administrator"
            groups: ["sudo"]

          appuser:
            comment: "Application User"
            shell: "/bin/false"
```

### Example with Custom Password Filename

```yaml
- hosts: all
  become: true
  roles:
    - role: grzegorzfranus.users
      vars:
        # Enable password storage on controller with custom filename
        users_store_passwords_controller: true
        users_password_store_file_name: "passwords_{{ inventory_hostname }}.txt"

        # Define users to create
        users_dict:
          admin:
            comment: "System Administrator"
            groups: ["sudo"]
```

### Complete Example with Dictionary Format, Password Storage, and SSH Keys

```yaml
- hosts: all
  become: true
  roles:
    - role: grzegorzfranus.users
      vars:
        users_create_home: true
        users_default_shell: "/bin/bash"
        users_password_max_age: 90
        users_password_min_age: 7
        users_create_group: true
        users_default_groups: ["users"]

        # Password generation and storage settings
        users_generate_password: true
        users_password_length: 16
        users_store_passwords_controller: true
        users_password_store_file_name: "users_{{ inventory_hostname }}_{{ ansible_facts['date_time']['date'] }}.txt"

        # Store passwords on remote hosts
        users_store_passwords_remote: true
        users_password_store_remote_path: "/root/local_passwords/user_passwords.txt"

        # SSH key management
        users_manage_ssh_keys: true

        # Sudoers management
        users_manage_sudoers: true

        # Users to create
        users_dict:
          admin:
            comment: "System Administrator"
            uid: 1001
            groups: ["wheel", "sudo"]
            shell: "/bin/bash"
            force: true
            sudoers: true
            ssh_keys:
              - "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPM4xxxxxxxxxx admin@example.com"

          appuser:
            comment: "Application User"
            system: true
            home: "/opt/app"
            shell: "/bin/false"

        # Users to remove
        users_remove_dict:
          olduser:
            remove_home: true
```

### Example with All Available User Parameters

```yaml
- hosts: all
  become: true
  roles:
    - role: grzegorzfranus.users
      vars:
        users_dict:
          fullexample:
            # Basic user information
            comment: "Full Example User with All Parameters"
            uid: 1500                        # User ID

            # Group settings
            group: "customgroup"             # Primary group (will be created if it doesn't exist)
            groups: ["wheel", "developers"]  # Secondary groups
            append: true                     # Add to groups without replacing existing ones

            # Shell & home settings
            shell: "/bin/zsh"                # Login shell
            home: "/opt/users/fullexample"   # Custom home directory
            create_home: true                # Create home directory
            move_home: false                 # Don't move if home exists at another location

            # System & state settings
            system: false                    # Regular user (not system user)
            state: "present"                 # Ensure user exists

            # Password settings
            password: "SecureP@ss123"        # Will be hashed automatically
            update_password: "on_create"     # Only set password when user is created
            password_expire_max: 60          # Maximum password age in days
            password_expire_min: 1           # Minimum password age in days
            expires: "2025-12-31"            # Account expiration date

            # SSH settings (requires users_manage_ssh_keys: true)
            ssh_keys:
              - "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIKu/sLk+n6ViTmGwTwZI5Bmev7SMYcQ713K3ScPAFKGz user@host"

            # Sudoers settings (requires users_manage_sudoers: true)
            sudoers:
              enabled: true
              comment: "Full access admin"
              rules:
                - "ALL=(ALL) NOPASSWD:ALL"

            # Misc settings
            force: false                     # Don't force operations
            remove: false                    # Don't remove home when state=absent
            no_log: true                     # Don't log sensitive information

            # Custom parameters (not used by user module but passed to set_fact)
            password_length: 20              # Custom length for generated password
```

## CI/CD Pipeline

This repository uses centralized, reusable GitHub Actions workflows from the `main` branch of [grzegorzfranus/github-workflows](https://github.com/grzegorzfranus/github-workflows) for quality assurance, security scanning, and release automation. Consuming the `main` branch ensures upstream workflow changes and security enhancements take effect immediately without requiring a version bump in this repository.

### CI Pipeline (`ansible-ci.yml@main`)

Runs on every Pull Request in a two-tier gate pattern:

1. **Branch Name Lint** — enforces naming conventions (`feature/`, `bugfix/`, `fix/`, `hotfix/`, `release/`, `chore/`, `docs/`, `refactor/`, `test/`, `build/`, `ci/`, `perf/`, `revert/`)
2. **PR Title Lint** — enforces [Conventional Commits](https://www.conventionalcommits.org/) format (`feat:`, `fix:`, `ci:`, etc.)
3. **YAML Syntax Lint** — validates YAML formatting via `yamllint`
4. **Ansible Lint** — checks Ansible best practices and role standards
5. **Galaxy Metadata Validation** — verifies `meta/main.yml` schema and requirements (`ansible-meta-validate.yml`)
6. **Security Scanning** — TruffleHog secret detection and Trivy IaC scanning (`ansible-security.yml`)
7. **Molecule Integration Tests** — executes Molecule test matrix across Ubuntu 26.04, Ubuntu 24.04, Ubuntu 22.04, Debian 13, Debian 12, Debian 11, and Rocky Linux 9 (`ansible-molecule.yml`)
8. **Merge Check Gate** — single authoritative status check aggregating all results for branch protection

### Release & Publish Pipeline (`ansible-publish.yml@main`)

Automated via [Release Please](https://github.com/googleapis/release-please):

1. **Push to `main`** → Release Please creates or updates a Release PR with automated changelog generation
2. **Release PR Validation** → validates YAML syntax and actions schema before setting `Merge Check` status
3. **Merge Release PR** → creates Git version tag and GitHub Release automatically
4. **Ansible Galaxy Publish** → publishes tagged release to Ansible Galaxy via `ansible-publish.yml@main` with exponential backoff retry logic

## 🛡️ Security Features

- ✅ **Visudo Syntax Validation**: All `/etc/sudoers.d/` drop-in files are validated with `visudo -cf %s` before application
- ✅ **Strict Drop-In Permissions**: Sudoers files are created with `0440` (read-only by owner and group, no write access)
- ✅ **Least Privilege Support**: Granular sudo rules, command restrictions, and custom `Defaults:` per user
- ✅ **Secure Password Hashing**: SHA-512 with cryptographically secure random salts
- ✅ **No Plain-Text Storage**: Generated passwords are never stored in plain text in variables
- ✅ **Controlled Logging**: Sensitive operations use `no_log: true` to prevent credential exposure
- ✅ **Automatic Drop-In Cleanup**: Sudoers drop-in files are automatically purged when accounts are disabled or removed
- ✅ **Privilege Escalation**: Secure sudo handling for user management operations
- ✅ **SSH Key Validation**: Proper SSH key format validation and secure permissions (`0700`/`0600`)
- ✅ **Home Directory Permissions**: Secure default permissions for user home directories
- ✅ **Password Policies**: Configurable aging policies to enforce password rotation
- ✅ **Audit Trail**: Comprehensive logging for security compliance and troubleshooting

### Enhanced Security Configuration

```yaml
# Enable secure password generation with strong policies
users_generate_password: true
users_password_length: 20
users_password_hash_algorithm: "sha512"
users_password_max_age: 60
users_password_min_age: 7

# SSH key management with secure defaults
users_manage_ssh_keys: true
users_ssh_key_type: "ed25519"
users_ssh_directory_mode: "0700"
users_ssh_authorized_keys_mode: "0600"

# Secure password storage (choose one approach)
users_store_passwords_controller: false  # Don't store on controller
users_store_passwords_remote: false      # Don't store on remote hosts
```

## 🔍 Verification

After deployment, verify user management and sudoers configuration:

### Verify Sudoers Drop-In Configuration

```bash
# Verify active sudo permissions for a user
sudo -l -U username

# Validate syntax of all sudoers configuration files
sudo visudo -c

# Inspect generated drop-in files and permissions (must be 0440)
ls -la /etc/sudoers.d/
```

### Check User Account Status

```bash
# Verify user was created
id username

# Check user's groups
groups username

# Verify user's shell
getent passwd username | cut -d: -f7

# Check password aging policy
chage -l username

# Verify home directory permissions
ls -la /home/username
```

### Verify SSH Key Configuration

```bash
# Check SSH directory permissions
ls -la /home/username/.ssh/

# Verify authorized_keys file
cat /home/username/.ssh/authorized_keys

# Test SSH key authentication (from another host)
ssh -i private_key username@hostname
```

### Check Password Generation

```bash
# If passwords stored on remote host
sudo cat /path/to/password/file

# If passwords stored on controller
cat local_password_file.txt

# Verify password hash in shadow file
sudo grep username /etc/shadow
```

### Verify User Removal

```bash
# Check user no longer exists
id removed_username 2>/dev/null || echo "User successfully removed"

# Verify home directory was removed (if configured)
ls -la /home/removed_username 2>/dev/null || echo "Home directory successfully removed"

# Verify sudoers drop-in file was removed
ls /etc/sudoers.d/removed_username 2>/dev/null || echo "Sudoers drop-in successfully removed"
```

## ⚠️ Known Issues

### Ansible Deprecation Warning (ansible-core 2.17+)

When using this role with ansible-core 2.17 or later, you may see the following deprecation warning:

```
[DEPRECATION WARNING]: Importing 'to_native' from 'ansible.module_utils._text' is deprecated.
This feature will be removed from ansible-core version 2.24.
Use ansible.module_utils.common.text.converters instead.
```

**This warning is NOT a defect in this role.** It originates from Ansible's internal `ansible.builtin.authorized_key` module code. Key points:

- ✅ **No action required** - The role functions correctly despite the warning
- ✅ **Cosmetic only** - This is a deprecation notice, not an error
- ✅ **Will be fixed by Ansible** - The Ansible core team will update the module before version 2.24
- ✅ **No role code changes needed** - The deprecated import is in Ansible's internal module code

To suppress this warning temporarily, you can set the environment variable:
```bash
export ANSIBLE_DEPRECATION_WARNINGS=False
```

Or in your `ansible.cfg`:
```ini
[defaults]
deprecation_warnings = False
```

**Note:** Suppressing warnings is not recommended for production use as it may hide other important deprecation notices.

## 📁 File Structure

```
ansible-role-users/
├── .github/
│   ├── workflows/
│   │   ├── ci.yml                     # CI pipeline
│   │   └── release.yml                # Release Please + Galaxy publish
│   └── dependabot.yml                 # Dependabot configuration for GitHub Actions
├── defaults/
│   └── main.yml                       # Default variables (Sections 1-6)
├── handlers/
│   └── main.yml                       # Service handlers
├── meta/
│   ├── main.yml                       # Role metadata
│   └── argument_specs.yml             # Argument specs validation
├── molecule/                          # Molecule testing framework
│   └── default/                       # Default testing scenario (prepare, converge, verify)
├── tasks/
│   ├── main.yml                       # Main orchestration and flow control
│   ├── assert.yml                     # Variable validation (Sections 1-6)
│   ├── create.yml                     # User creation tasks
│   ├── sudoers.yml                    # Per-user sudoers drop-in tasks
│   └── remove.yml                     # User removal tasks
├── templates/
│   └── sudoers/
│       └── user.j2                    # Jinja2 template for /etc/sudoers.d drop-in
└── vars/
    └── main.yml                       # Internal variables/constants (__users_*)
```

## 🔧 Troubleshooting

### Sudoers File Ignored by Sudo

If sudo ignores a drop-in file:
1. Verify the filename does not contain a dot (`.`): `sudo` explicitly ignores all files with dots in `/etc/sudoers.d/`.
2. Check file permissions: ensure the file has mode `0440` (or `0400`) and is owned by `root:root`:
   ```bash
   ls -la /etc/sudoers.d/
   chmod 0440 /etc/sudoers.d/<filename>
   ```

### Sudoers Syntax Validation Failure

If task deployment fails during `visudo` validation:
1. Verify rules syntax manually: `visudo -cf /path/to/rendered/file`
2. Ensure there are no unexpected newlines in custom `rules` or `defaults` entries.

### User Creation Issues

If a user cannot be created, verify that there are no conflicting UIDs or usernames:

```bash
# Check if username already exists
getent passwd username

# Check if UID already exists
getent passwd | grep :1500:
```

### SSH Key Authentication Failures

If SSH key authentication is not working, verify that the `.ssh` directory and `authorized_keys` file have the correct permissions:

```bash
# Check permissions on home and .ssh
ls -la /home/username
ls -la /home/username/.ssh
```

## 🤝 Contributing

Contributions, bug reports, and feature requests are welcome!

- Fork the repository and create your branch from `main`
- Use [Conventional Commits](https://www.conventionalcommits.org/) for commit messages
- Centralized workflows from the `main` branch of [github-workflows](https://github.com/grzegorzfranus/github-workflows) are used to run CI/CD pipelines
- Ensure your code passes all CI checks (YAML lint, Ansible lint, Molecule tests)
- Submit a pull request describing your changes

## 📝 License

This project is licensed under the Apache-2.0 License - see the LICENSE file for details.

## 👥 Author Information

This role was created by [Grzegorz Franus](https://github.com/grzegorzfranus).
