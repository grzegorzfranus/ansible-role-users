# Changelog

All notable changes to this User Management role will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.4] - 2025-11-29

### Fixed 🔧
- Replaced deprecated top-level fact variables with `ansible_facts[]` syntax to resolve `INJECT_FACTS_AS_VARS` deprecation warning
  - `ansible_hostname` → `ansible_facts['hostname']`
  - `ansible_date_time.*` → `ansible_facts['date_time']['*']`
  - `ansible_os_family` → `ansible_facts['os_family']`
- Updated all task files, defaults, and molecule tests to use the new fact access pattern

### Changed 🔄
- Updated minimum Ansible version requirement from 2.15 to 2.17
- Verified full compatibility with Ansible 2.20

### Documentation 📚
- Added note about Ansible internal deprecation warning (`to_native` import in `authorized_key` module)
- Clarified that the deprecation warning is an Ansible core issue, not a role defect
- Updated README.md with Ansible version requirements and known issues section

### Compatibility ✅
- Tested and validated with ansible-core 2.17, 2.18, 2.19, and 2.20
- Full compliance with ansible-core 2.24 deprecation requirements
- The `ansible.builtin.authorized_key` module deprecation warning is cosmetic and does not affect functionality

## [1.0.3] - 2025-08-08

### Changed 🔄
- Respect configured `users_password_hash_algorithm` and remove deterministic salt usage in password hashing
- Use list type for `user.groups` instead of comma-joined string
- Simplify SSH directory group ownership default to rely on system defaults
- Remove non-idempotent `chage` command task (use `user.expires` instead)

### Added ✅
- New `users_overwrite_existing` default (false): when a user already exists, do not update password or other attributes unless `force` (per user) or `users_overwrite_existing` is true
- Password and attribute updates now conditionally apply only when creating a user or when overwrite is enabled
- Replace deprecated/legacy loops with modern style only where needed (kept structure consistent)
 - Password files are now written only when at least one password is generated (new users or overwrite)

### Fixed 🔧
- Molecule: Fix play name casing to start with uppercase (ansible-lint name[casing])
- Use `ansible.builtin.command` FQCN and add justified `# noqa command-instead-of-module` for systemd readiness check
- Replace unnecessary `shell` tasks in verify with modules/`command` invocations
- Address YAML long-line warnings by folding messages in `tasks/assert.yml`
- Remove `md5` from allowed hash algorithms in assertions and docs
- Clean `meta/main.yml` by removing non-standard `galaxy_tags` under platform entries

### Security 🔐
- Disallow weak `md5` hashing; only `sha512` and `sha256` accepted
- Narrow potential overuse of salts by letting `password_hash` generate a secure random salt

### Linting ✅
- ansible-lint: 0 failures on role and Molecule files
- yamllint: warnings addressed in assertions file

## [1.0.2] - 2025-06-27

### Changed 🔄
- **Professional Workflow Naming** - Renamed `galaxy.yml` to `publish-to-galaxy.yml` for better clarity
- **CI Pipeline Rebranding** - Renamed `ci.yml` to `test-and-validation.yml` with professional naming
- **Enhanced CI/CD Presentation** - Added emoji indicators throughout all GitHub Actions workflows
- **Improved Job Organization** - Renamed workflow jobs for better semantic meaning and clarity
- **Step Name Clarity** - Enhanced step descriptions with more specific and professional language

### Fixed 🔧
- **CI Badge Reference** - Updated README.md CI status badge to point to renamed `test-and-validation.yml` workflow
- **Documentation Consistency** - Fixed broken badge link that was still referencing old `ci.yml` workflow file

### Workflow Improvements 🚀
- **Galaxy Publishing**: Changed to `📦 Publish to Ansible Galaxy` with descriptive emoji
- **Testing Pipeline**: Updated to `🧪 Test & Validation Pipeline` for professional appearance
- **Job Naming**: Consistent professional naming across all workflows
- **Step Emojis**: Added relevant emoji to all workflow steps:
  - 📥 Check out the codebase
  - 🐍 Set up Python 3
  - 📦 Install dependencies / Install Ansible dependencies
  - 🔍 Lint code
  - 🧪 Molecule test
  - 🚀 Upload role to Ansible Galaxy / Run molecule test

### Repository Organization 📁
- **Workflow Consolidation**: Two professionally named workflows for complete CI/CD pipeline
- **Consistency**: Aligned all workflow naming and emoji usage for cohesive presentation
- **Documentation**: Enhanced workflow readability and maintenance across the board
- **Badge Accuracy**: Ensured CI status badges reflect current workflow structure

### Validation Enhancements 🧪
- **Professional Assert Naming**: Updated assert.yml task names with 🧪 emoji for consistency
- **Enhanced Error Messages**: Improved fail_msg with ❌ emoji and detailed variable information
- **Success Feedback**: Added success_msg with ✅ emoji showing validated values and configuration details
- **Verbose Validation**: Removed `quiet: true` parameter for better debugging visibility
- **Descriptive Error Context**: Enhanced error messages with security recommendations and valid examples
- **Troubleshooting Support**: Added helpful format examples and valid ranges in validation messages

### Task Enhancement 📋
- **Main Task Emojis**: Added descriptive emojis to all main.yml task names for better readability
  - 🔧 OS-specific variable gathering
  - 🧪 Role variable validation
  - 👥 User account creation
  - 🗑️ User account removal
- **Visual Task Flow**: Enhanced task identification with semantic emojis matching functionality
- **Consistent Naming**: Aligned main tasks with professional emoji standards across the role

### Documentation Enhancement 📚

## [1.0.1] - 2025-05-18

### Added ✅
- Initial user management functionality
- Support for user creation and removal
- SSH key management capabilities
- Password generation and handling
- Comprehensive testing with Molecule

### Security Features 🔐
- Secure password generation
- SSH key management
- Proper file permissions
- User validation and safety checks

### Technical Implementation 🛠️
- Multi-platform support (Ubuntu, Debian, Rocky Linux)
- Molecule testing framework integration
- Ansible best practices implementation
- Clean task organization and handlers

## [1.0.0] - 2025-05-18

### Added ✅
- **Initial Release** - Complete user management role for Linux systems
- **User Creation** - Support for creating users with custom configurations
- **User Removal** - Safe user deletion with optional home directory cleanup
- **SSH Key Management** - Automated SSH key deployment and management
- **Password Handling** - Secure password generation and setting
- **Multi-Platform Support** - Compatibility with Ubuntu, Debian, and Rocky Linux
- **Comprehensive Testing** - Full Molecule test suite with multi-distribution support
- **Documentation** - Complete README with usage examples and variable documentation

### Initial Features 🚀
- **User Management**: Create and remove users with configurable parameters
- **Security**: SSH key management and secure password handling
- **Flexibility**: Customizable user attributes (shell, home directory, groups)
- **Validation**: Input validation and error handling
- **Testing**: Automated testing across multiple Linux distributions
- **CI/CD**: GitHub Actions workflows for continuous integration 