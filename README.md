<div align="center">

# Enterprise Git Multi-Account Management

## Professional Guide for Managing Multiple GitHub Accounts in Enterprise Environments

</div>

This comprehensive guide outlines enterprise-grade procedures for configuring and managing multiple GitHub accounts within a single development environment. It provides detailed instructions for SSH key management, account configuration, and repository handling, ensuring secure and efficient workflows for development teams managing multiple accounts across personal, professional, and client environments.

## Table of Contents

- [Core Capabilities](#core-capabilities)
- [Environment Setup](#environment-setup)
  - [System Requirements](#system-requirements)
  - [Initial Configuration](#initial-configuration)
    - [Environment Assessment](#environment-assessment)
    - [Configuration Reset Protocol](#configuration-reset-protocol)
- [Getting Started](#-getting-started)
  - [Prerequisites](#-prerequisites)
  - [Initial Setup](#-initial-setup)
    - [Cleaning Existing Configurations](#cleaning-existing-configurations)
- [SSH Key Generation and Configuration](#-ssh-key-generation-and-configuration)
  - [Creating SSH Keys](#creating-ssh-keys)
  - [SSH Configuration Setup](#ssh-configuration-setup)
  - [Adding SSH Keys to GitHub](#adding-ssh-keys-to-github)
  - [Verifying Connections](#verifying-connections)
  - [Repository Configuration](#repository-configuration)
- [Enterprise Multi-Account Configuration](#enterprise-multi-account-configuration)
  - [Configuring Multiple Remote Repositories](#configuring-multiple-remote-repositories)
  - [Existing Repository Setup](#existing-repository-setup)

### Core Capabilities:

- 🔐 Enterprise-grade SSH key management
- ⚡ Streamlined multi-account workflows
- 🔄 Advanced configuration management
- 💼 Cross-account repository handling
- 🛠️ Environment-specific configurations

## Environment Setup

### System Requirements

Ensure your development environment meets these prerequisites:

- Git (Latest stable version)
  - [Windows Installation](https://git-scm.com/download/win)
  - [macOS Installation](https://git-scm.com/download/mac)
  - [Linux Installation](https://git-scm.com/download/linux)
- Administrative access to all relevant GitHub accounts
- Command-line interface proficiency
- Understanding of Git fundamentals

### Initial Configuration

#### Environment Assessment

```bash
# Review existing configuration
git config --list --show-origin

# Analyze SSH environment
ls -la ~/.ssh/
```

#### Configuration Reset Protocol

```bash
# Remove global configurations
git config --global --unset-all user.name
git config --global --unset-all user.email

# Clear system-specific credentials
git credential-manager uninstall  # Windows
git credential-osxkeychain erase # macOS
```

## 🚀 Getting Started

### 📋 Prerequisites

Before starting, ensure you have:

- Git installed on your computer ([Download Git](https://git-scm.com/downloads))
  - [Windows](https://git-scm.com/download/win)
  - [macOS](https://git-scm.com/download/mac)
  - [Linux/Unix](https://git-scm.com/download/linux)
- Access to both GitHub accounts
- Terminal/Git Bash (for Windows users)
- Basic understanding of Git commands

> **Note**: If you prefer a GUI interface, Git comes with built-in tools (git-gui, gitk), but there are also various [third-party GUI clients](https://git-scm.com/downloads/guis) available.

### 🔄 Initial Setup

#### Cleaning Existing Configurations

If you already have Git configured, follow these steps to clean your existing setup:

##### 1. Review Current Configuration

```bash
git config --list
```

##### 2. Remove Global Git Configurations

```bash
# Remove individual settings
git config --global --unset user.name
git config --global --unset user.email

# Or remove all global configurations
git config --global --unset-all
```

##### 3. Clean SSH Configuration

```bash
# Navigate to SSH directory
cd ~/.ssh

# List existing SSH files
ls

# Remove existing SSH keys and configurations
# Note: Replace these with your actual key names
rm id_rsa*
rm id_ed25519*
rm known_hosts
rm config
```

##### 4. Clear Credential Storage

###### Windows Users

```bash
# Clear Windows Credentials Manager
cmdkey /delete:LegacyGeneric:git:https://github.com
```

###### macOS Users

```bash
# Remove from Keychain
git credential-osxkeychain erase
host=github.com
protocol=https
# Press Return when prompted
```

##### 5. GitHub CLI Cleanup (Optional)

```bash
# If you have GitHub CLI installed
gh auth logout
```

> **⚠️ Warning**: These commands will remove all existing Git configurations and SSH keys. Make sure to backup any important keys before proceeding.

## 🔑 SSH Key Generation and Configuration

### Creating SSH Keys

Generate unique SSH keys for each GitHub account:

#### Primary Account

```bash
ssh-keygen -t ed25519 -C "your-primary-email@example.com"
# When prompted:
# File location: ~/.ssh/id_ed25519_primary
# Passphrase: Enter a secure passphrase (recommended) or leave empty
```

#### Secondary Account

```bash
ssh-keygen -t ed25519 -C "your-secondary-email@example.com"
# When prompted:
# File location: ~/.ssh/id_ed25519_secondary
# Passphrase: Enter a secure passphrase (recommended) or leave empty
```

### SSH Configuration Setup

Create or edit your SSH config file:

```bash
# Create/edit SSH config file
nano ~/.ssh/config
```

Add the following configuration:

```bash
# Primary GitHub Account
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_primary

# Secondary GitHub Account
Host github-secondary
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_secondary
```

### Adding SSH Keys to GitHub

#### Primary Account

1. Copy the SSH public key:

```bash
cat ~/.ssh/id_ed25519_primary.pub
```

2. Navigate to GitHub → Settings → SSH and GPG keys → New SSH key
3. Paste the key and save

#### Secondary Account

1. Copy the SSH public key:

```bash
cat ~/.ssh/id_ed25519_secondary.pub
```

2. Navigate to GitHub → Settings → SSH and GPG keys → New SSH key
3. Paste the key and save

### Verifying Connections

Test both SSH connections:

```bash
# Test primary account
ssh -T git@github.com

# Test secondary account
ssh -T git@github-secondary
```

### Repository Configuration

#### For Primary Account Projects

```bash
git remote add origin git@github.com:username/repository.git
git config user.email "your-primary-email@example.com"
git config user.name "Your First Name"
```

#### For Secondary Account Projects

```bash
git remote add origin git@github-secondary:username/repository.git
git config user.email "your-secondary-email@example.com"
git config user.name "Your Second Name"
```

## Enterprise Multi-Account Configuration

### Configuring Multiple Remote Repositories

This section outlines the enterprise-grade configuration process for managing multiple GitHub accounts within a single project environment.

#### 1. Repository Initialization

```bash
# Navigate to project directory
cd /path/to/project

# Initialize repository (if not existing)
git init

# Configure primary enterprise account
git config user.email "primary.account@enterprise.com"
git config user.name "Enterprise Account"
```

#### 2. Remote Repository Configuration

```bash
# Configure primary enterprise remote
git remote add enterprise git@github.com:enterprise-org/repository.git

# Configure secondary remote (e.g., development environment)
git remote add development git@github-secondary:dev-org/repository.git

# Verify remote configurations
git remote -v
```

#### 3. Remote Repository Operations

```bash
# Enterprise repository operations
git push enterprise main     # Deploy to enterprise environment
git pull enterprise main     # Sync from enterprise environment

# Development repository operations
git push development main    # Deploy to development environment
git pull development main    # Sync from development environment
```

> **Important**: Ensure branch names align with your organization's naming conventions. Common examples include `main`, `master`, or `production`.

#### Enterprise Best Practices

- Implement consistent remote naming conventions across teams
- Maintain separate deployment workflows for each environment
- Regularly audit remote configurations using `git remote -v`
- Document remote-specific access controls and permissions
- Consider implementing git hooks for environment-specific validations

### Existing Repository Setup

When working with an existing repository in your GitHub account, follow these steps:

#### 1. Get Repository URL

1. Navigate to the repository on GitHub
2. Click the "Code" button
3. Select SSH as the protocol
4. Copy the URL (format: `git@github.com:username/repository.git`)

#### 2. Clone Repository

```bash
# Clone using your configured SSH host
# Example using secondary account:
git clone git@github-secondary:username/repository.git

# Navigate to repository
cd repository
```

#### 3. Configure Local Repository

```bash
# Set user email and name for this repository
git config user.email "your-account@example.com"
git config user.name "Your Account Name"

# Verify configuration
git config --list
```

#### 4. Normal Git Operations

```bash
# Standard git workflow
git add .
git commit -m "Your descriptive commit message"
git push origin main  # Use appropriate branch name (main/master)
```

> **Note**: Ensure you're using the correct SSH host (`github.com` or `github-secondary`) based on which account owns the repository.
