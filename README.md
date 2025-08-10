# Ansible Role: Users

|Source|Version|Tests|License|
|------|-------|-------|-------|
|[![Source Code](https://img.shields.io/badge/source-github-blue.svg)](https://github.com/grzegorzfranus/ansible-role-users)|[![Version](https://img.shields.io/github/v/release/grzegorzfranus/ansible-role-users)](https://github.com/grzegorzfranus/ansible-role-users/releases)|[![tests](https://github.com/grzegorzfranus/ansible-role-users/actions/workflows/test-and-validation.yml/badge.svg)](https://github.com/grzegorzfranus/ansible-role-users/actions)|[![Repository License](https://img.shields.io/badge/license-apache2.0-brightgreen.svg)](LICENSE)|

This Ansible role manages user accounts on Linux systems. It provides capabilities for creating users with custom home directories, shells, comments, and other parameters. The role also includes secure password generation and optional SSH key management.

## ✨ Features

- 👥 **User Account Management**: Create, modify, and remove user accounts with full control
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
- Securely generate random passwords
- Manage SSH authorized keys for users
- Set password expiration policies
- Remove users with optional home directory removal

## 📋 Requirements

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
This role already handles privilege escalation for tasks that require it. You can invoke the role in your playbook like (Galaxy name recommended):
```yaml
- hosts: servers
  roles:
    - role: grzegorzfranus.users
```

## ⚙️ Role Variables

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
| `users_password_hash_algorithm` | Password hashing algorithm (sha512, sha256) | `"sha512"` |

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
| `remove` | Whether to remove home dir when removing user | `false` |
| `no_log` | Whether to suppress logging of task | `true` |
| `password_length` | Custom length for generated password | `users_password_length` |
## ♻️ Overwrite Behavior for Existing Users

By default, the role does not modify existing users. This means their password, shell, groups, and other attributes remain unchanged unless you explicitly enable overwriting:

- Global toggle: set `users_overwrite_existing: true` to allow updates for all existing users
- Per-user override: set `force: true` within a specific user entry in `users_dict`

When neither is set, only new users are created and existing users are left intact. Password updates use `update_password: on_create` unless overwrite is enabled for the user.


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

## 📖 Example Playbooks

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

### Basic Example with Password Storage in Current Directory

```yaml
- hosts: all
  roles:
    - role: ansible-role-users
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
  roles:
    - role: ansible-role-users
      # Enable password storage on controller with custom filename
      users_store_passwords_controller: true
      users_password_store_file_name: "passwords_{{ inventory_hostname }}.txt"
      
      # Define users to create
      users_dict:
        admin:
          comment: "System Administrator"
          groups: ["sudo"]
```

### Complete Example with Dictionary Format and Password Storage

```yaml
- hosts: all
  roles:
    - role: ansible-role-users
      # User default settings
      users_create_home: true
      users_default_shell: "/bin/bash"
      users_password_max_age: 90
      users_password_min_age: 7
      users_create_group: true
      users_default_groups: ["users"]
      
      # Password generation and storage settings
      users_generate_password: true
      users_password_length: 16
      
      # Store passwords on Ansible controller with custom filename
      users_store_passwords_controller: true
      users_password_store_file_name: "users_{{ inventory_hostname }}_{{ ansible_date_time.date }}.txt"
      
      # Store passwords on remote hosts
      users_store_passwords_remote: true
      users_password_store_remote_path: "/root/local_passwords/user_passwords.txt"
      
      # SSH key management
      users_manage_ssh_keys: true
      
      # Users to create
      users_dict:
        admin:
          comment: "System Administrator"
          uid: 1001
          groups: ["wheel", "sudo"]
          shell: "/bin/bash"
          # Overwrite this user on subsequent runs
          force: true
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

### 🧪 Test & Validation Pipeline
- **Workflow**: `.github/workflows/test-and-validation.yml`
- **Name**: `🧪 Test & Validation Pipeline`
- **Purpose**: Automated testing using Molecule across multiple distributions
- **Triggers**: Push to main branch, pull requests to main/feature/release/bugfix/hotfix branches

### 📦 Galaxy Publishing
- **Workflow**: `.github/workflows/publish-to-galaxy.yml`
- **Name**: `📦 Publish to Ansible Galaxy`
- **Purpose**: Automated role publishing to Ansible Galaxy
- **Triggers**: Tagged releases (v*)

## 🛡️ Security Features

- ✅ **Secure Password Hashing**: SHA-512 with cryptographically secure random salts
- ✅ **No Plain-Text Storage**: Generated passwords are never stored in plain text
- ✅ **Controlled Logging**: Sensitive operations use `no_log: true` to prevent exposure
- ✅ **Privilege Escalation**: Secure sudo handling for user management operations
- ✅ **SSH Key Validation**: Proper SSH key format validation and secure deployment
- ✅ **Home Directory Permissions**: Secure default permissions for user home directories
- ✅ **Password Policies**: Configurable aging policies to enforce password rotation
- ✅ **Account Expiration**: Support for time-based account expiration
- ✅ **Group Management**: Secure group membership and permission assignment
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

After deployment, verify the user management operations completed successfully:

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

# Check user's group was removed (if it was a unique group)
getent group removed_username 2>/dev/null || echo "User group successfully removed"
```

## 🤝 Contributing

Contributions, bug reports, and feature requests are welcome!

- Fork the repository and create your branch from `main`.
- Make your changes with clear, descriptive commit messages.
- Ensure your code passes all Molecule and lint tests.
- Submit a pull request describing your changes and the motivation.
- For major changes, please open an issue first to discuss what you would like to change.

If you have questions or suggestions, feel free to open an issue or contact the author via GitHub.

## 📝 License

This project is licensed under the Apache-2.0 License - see the LICENSE file for details.

## 👥 Author Information

This role was created by [Grzegorz Franus](https://github.com/grzegorzfranus).
