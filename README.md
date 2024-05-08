# Template Ansible

```text
ansible-project/
├── ansible.cfg              # Main configuration file for Ansible
├── inventory                # Inventory directory
│   ├── hosts                # Default inventory file, can be YAML or INI
│   ├── production.yml       # Production environment inventory
│   └── staging.yml          # Staging environment inventory
├── playbooks                # Directory containing all your playbooks
│   ├── site.yml             # Main playbook that includes others
│   ├── webservers.yml       # Playbook for webserver setup
│   └── databases.yml        # Playbook for database setup
├── roles                    # Directory for Ansible roles
│   ├── common               # Role for common tasks across all nodes
│   │   ├── tasks            # Tasks subdirectory
│   │   │   └── main.yml     # Main list of tasks to be executed
│   │   ├── handlers         # Handlers subdirectory
│   │   │   └── main.yml     # Handlers for service restarts, etc.
│   │   ├── defaults         # Default variables
│   │   │   └── main.yml     # Default variables for the role
│   │   ├── vars             # Other variables
│   │   │   └── main.yml     # Main variables file for the role
│   │   ├── files            # Files for deployment
│   │   ├── templates        # Jinja2 templates
│   │   │   └── ntp.conf.j2  # Sample template file
│   │   └── meta             # Role dependencies
│   │       └── main.yml     # Metadata about role, including dependencies
│   └── webserver            # Role specific to webserver setup
│       ├── ...
├── group_vars               # Directory for group-specific variables
│   ├── group1.yml           # Variables for 'group1'
│   └── all.yml              # Variables that apply to all groups
└── host_vars                # Directory for host-specific variables
    ├── host1.yml            # Variables specific to 'host1'
    └── host2.yml            # Variables specific to 'host2'
```

## Tasks

### Docker Environment Setup

This Ansible playbook is designed to automate the setup and configuration of a Docker environment on Debian-based systems. The primary goals are to ensure that all necessary dependencies and configurations are properly installed and set up. Here's a brief overview of what the playbook does:

- **Variable Validation:** Checks to ensure all required variables (`docker_key_url`, `docker_key_path`, `dkrs_packages`) are defined, ensuring that the playbook does not run with missing dependencies.
- **Directory Setup:** Ensures the keyring directory exists, which is essential for storing Docker's GPG keys securely.
- **GPG Key Management:** Performs checks to see if the Docker GPG key is already imported. If not, it downloads and imports the key, setting appropriate permissions.
- **Repository Configuration:** Adds the Docker repository to the system's software sources, enabling Docker package installation.
- **Docker Installation:** Installs Docker packages specified by the user, which can include Docker Engine, CLI, and Containerd.
- **User Group Configuration:** Adds a specified non-root user to the Docker group, allowing them to execute Docker commands without requiring root privileges.

## System Configuration Optimization
This Ansible playbook is tailored to optimize critical system settings that affect performance and security. It primarily focuses on managing swap usage and adjusting kernel parameters. Here’s an overview of the playbook's operations:

- **Disable Swap:** Temporarily disables swap to improve system performance, especially useful for performance-sensitive applications like databases or in-memory caches. This task ensures that swap is turned off without modifying any system files.
- **Permanently Disable Swap:** Modifies the `/etc/fstab` file to permanently disable swap by commenting out any active swap entries. This helps prevent swap from being reactivated on system reboot.
- **Kernel Parameter Adjustment:** Increases the `vm.max_map_count` kernel setting, which is crucial for applications like Elasticsearch that require a higher limit on memory-mapped files. This setting is permanently applied and ensures it persists across reboots.


## Commands

```shell
clear && ansible-playbook -i inventories/hosts.yaml playbooks/main_recipe.yaml
```

###  Fix role into playbooks

```shell
cd playbooks
ln -s ../roles roles
```
