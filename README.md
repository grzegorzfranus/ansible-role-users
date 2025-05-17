# Ansible Role: Users

|Source|Version|CI|License|
|------|-------|-------|-------|
|[![Source Code](https://img.shields.io/badge/source-github-blue.svg)](https://github.com/grzegorzfranus/ansible-role-users)|[![Version](https://img.shields.io/github/v/release/grzegorzfranus/ansible-role-users)](https://github.com/grzegorzfranus/ansible-role-users/releases)|[![tests](https://github.com/grzegorzfranus/ansible-role-users/actions/workflows/ci.yml/badge.svg)](https://github.com/grzegorzfranus/ansible-role-users/actions)|[![Repository License](https://img.shields.io/badge/license-apache2.0-brightgreen.svg)](LICENSE)|

This Ansible role manages user accounts on Linux systems. It provides capabilities for creating users with custom home directories, shells, comments, and other parameters. The role also includes secure password generation and optional SSH key management.

## Main Actions

- Create and manage users with customizable parameters
- Securely generate random passwords
- Manage SSH authorized keys for users
- Set password expiration policies
- Remove users with optional home directory removal

## Requirements

### Supported operating systems
List of officially supported operating systems:
| OS Family | Version | Status |
|-----------|---------|---------|
| Ubuntu | 24.04 (Noble) | ![✓](https://img.shields.io/badge/✓-brightgreen.svg) |
| Ubuntu | 22.04 (Jammy) | ![✓](https://img.shields.io/badge/✓-brightgreen.svg) |
| Debian | 12 (Bookworm) | ![✓](https://img.shields.io/badge/✓-brightgreen.svg) |
| Debian | 11 (Bullseye) | ![✓](https://img.shields.io/badge/✓-brightgreen.svg) |
| Rocky Linux | 9 | ![✓](https://img.shields.io/badge/✓-brightgreen.svg) |
| Oracle Linux | 9 | ![✓](https://img.shields.io/badge/✓-brightgreen.svg) |

### Ansible version

Ansible >= 2.15

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
This role already handles privilege escalation for tasks that require it. You can invoke the role in your playbook like:
```yaml
- hosts: servers
  roles:
    - role: ansible-role-users
```

## Role Variables

### General Settings

| Variable | Description | Default |
|----------|-------------|---------|
| `users_role_action` | Role execution control (Options: 'all', 'create', 'remove') | `"all"` |
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

### Password Generation Settings

| Variable | Description | Default |
|----------|-------------|---------|
| `users_generate_password` | Generate random passwords for new users | `true` |
| `users_password_length` | Length of generated passwords | `16` |
| `users_password_store_path` | Path to store generated passwords (empty = don't store) | `""` |
| `users_password_chars` | Character set used for password generation | `"abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*()-_=+[]{}|;:,.<>?"` |
| `users_password_hash_algorithm` | Password hashing algorithm (sha512, sha256, md5) | `"sha512"` |

### SSH Key Management

| Variable | Description | Default |
|----------|-------------|---------|
| `users_manage_ssh_keys` | Whether to manage SSH keys for users | `false` |
| `users_ssh_key_type` | Default SSH key type (rsa, ed25519, ecdsa, dsa) | `"ed25519"` |
| `users_ssh_key_bits` | Key size for RSA keys (1024, 2048, 4096) | `4096` |
| `users_ssh_directory` | Path to SSH directory within user home | `".ssh"` |
| `users_ssh_authorized_keys_file` | Path to authorized_keys file | `".ssh/authorized_keys"` |
| `users_ssh_directory_mode` | Permissions for .ssh directory | `"0700"` |
| `users_ssh_authorized_keys_mode` | Permissions for authorized_keys file | `"0600"` |

### System Paths and Commands

| Variable | Description | Default |
|----------|-------------|---------|
| `users_home_base` | Base directory for regular user homes | `"/home"` |
| `users_system_home_base` | Base directory for system user homes | `"/var/lib"` |
| `users_pam_config_dir` | PAM configuration directory | `"/etc/pam.d"` |
| `users_login_defs_path` | Path to login.defs file | `"/etc/login.defs"` |
| `users_chage_command` | Command to set account expiration | `"chage"` |
| `users_lock_command` | Command to lock user accounts | `"usermod --lock"` |
| `users_unlock_command` | Command to unlock user accounts | `"usermod --unlock"` |

### User Dictionaries

| Variable | Description | Default |
|----------|-------------|---------|
| `users_dict` | Dictionary of users to create/manage | `{}` |
| `users_remove_dict` | Dictionary of users to remove | `{}` |

## User Parameters

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
| `password_expire_max` | Maximum password age in days | `users_password_max_age` |
| `password_expire_min` | Minimum password age in days | `users_password_min_age` |
| `expires` | Account expiration date (YYYY-MM-DD) | (never expires) |
| `ssh_keys` | List of SSH authorized keys | (none) |
| `force` | Whether to force operations | `false` |
| `remove` | Whether to remove home dir when removing user | `false` |
| `no_log` | Whether to suppress logging of task | `true` |
| `password_length` | Custom length for generated password | `users_password_length` |

## Secure Password Management

This role implements several security best practices for password management:

1. **Random Password Generation**: Uses Ansible's `password` lookup plugin to generate cryptographically secure random passwords.
2. **Secure Hashing**: All passwords are hashed using SHA-512 with a random salt.
3. **No Plain-Text Storage**: Generated passwords are not stored in variables longer than necessary.
4. **No Logging**: Password operations use `no_log: true` to prevent password exposure in logs.
5. **Optional Storage**: If you need to retrieve passwords, you can specify a secure path to store them, protected with strict permissions.

## Retrieving Generated Passwords

When `users_generate_password` is enabled and `users_password_store_path` is set, the role will store generated passwords in the specified file. This file will have strict permissions (0600) and will be owned by root.

For example with `users_password_store_path: "/root/user_passwords.txt"`, the generated passwords will be stored in that file with format:

```
# Generated Passwords - 2023-05-01T10:15:30Z
User: johndoe Password: Tb8y6tG$jK2p!9Lm
User: appuser Password: W4e@7Jh*zPq5$3xN
```

For maximum security, you should retrieve this file, store it securely (e.g., in a password manager), and then delete it from the server.

## Role Tags

The role uses the following tags for task selection:

| Tag | Description |
|-----|-------------|
| `always` | Tasks that always run (variable loading, validation) |
| `setup` | User account setup and creation tasks |
| `init` | Initial environment setup |
| `validate` | Variable validation tasks |
| `check` | Verification and checking tasks |
| `create` | Tasks related to user creation |
| `users` | All user-related tasks (both creation and removal) |
| `cleanup` | Tasks for user account removal |
| `remove` | Tasks specifically for removing users |

These tags can be used to selectively run parts of the role, for example:

```yaml
# Only run user creation tasks
ansible-playbook playbook.yml --tags "create"

# Skip user removal tasks
ansible-playbook playbook.yml --skip-tags "remove,cleanup"
```

## Example Playbooks

### Creating Users with Dictionary Format

```yaml
- hosts: all
  roles:
    - role: ansible-role-users
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

### Removing Users with Dictionary Format

```yaml
- hosts: all
  roles:
    - role: ansible-role-users
      users_role_action: "remove"
      users_remove_dict:
        olduser:
          remove_home: true
        tempuser: {}  # Use default settings
```

### Complete Example with Dictionary Format

```yaml
- hosts: all
  roles:
    - role: ansible-role-users
      users_create_home: true
      users_remove_home: true
      users_default_shell: "/bin/bash"
      users_password_max_age: 90
      users_password_min_age: 7
      users_create_group: true
      users_default_groups: ["users"]
      users_append_groups: true
      users_generate_password: true
      users_password_length: 16
      users_password_store_path: "/root/user_passwords.txt"
      users_manage_ssh_keys: true
      users_ssh_key_type: "ed25519"
      
      users_dict:
        admin:
          comment: "System Administrator"
          uid: 1001
          groups: ["wheel", "sudo"]
          shell: "/bin/bash"
          ssh_keys:
            - "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPM4xxxxxxxxxx admin@example.com"
        
        appuser:
          comment: "Application User"
          system: true
          home: "/opt/app"
          shell: "/bin/false"
      
      users_remove_dict:
        olduser:
          remove_home: true
        tempuser: {}
```

### Example with All Available User Parameters

```yaml
- hosts: all
  roles:
    - role: ansible-role-users
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
          
          # Misc settings
          force: false                     # Don't force operations
          remove: false                    # Don't remove home when state=absent
          no_log: true                     # Don't log sensitive information
          
          # Custom parameters (not used by user module but passed to set_fact)
          password_length: 20              # Custom length for generated password
```

## License

Apache-2.0

## Author Information

This role was created by [Grzegorz Franus](https://github.com/grzegorzfranus).

## Contributing

Contributions, bug reports, and feature requests are welcome!

- Fork the repository and create your branch from `main`.
- Make your changes with clear, descriptive commit messages.
- Ensure your code passes all Molecule and lint tests.
- Submit a pull request describing your changes and the motivation.
- For major changes, please open an issue first to discuss what you would like to change.

If you have questions or suggestions, feel free to open an issue or contact the author via GitHub.