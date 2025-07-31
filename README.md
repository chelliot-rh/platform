## Ansible Collection - fbi.platform_infra

This collection provides roles to automate Red Hat Satellite registration and platform infrastructure tasks.

---

## ✅ Collection Requirements

- Red Hat Enterprise Linux host(s)
- Ansible 2.14+ or AAP 2.4+
- Installed collections:
  - `redhat.satellite`
- Satellite server credentials and activation key

---

## 📦 Included Roles

| Role Name   | Purpose                                                      |
|-------------|--------------------------------------------------------------|
| `sat_reg`   | Registers a host to Red Hat Satellite using activation keys  |

---

## 📦 Role: `sat_reg`

This Ansible role automates the generation and execution of a Red Hat Satellite registration command using the `redhat.satellite.registration_command` module. It supports activation keys, lifecycle environments, and optional configuration for Insights and package updates.

### Role Requirements

- Requires access to a Red Hat Satellite server.
- Ensure the `redhat.satellite` collection is installed.
- The target host must be reachable and have the necessary permissions to register with Satellite.
- Credentials (`sat_org_admin_account` and `sat_org_admin_account_password`) should be securely stored using Ansible Vault or AAP credentials.

### Role Variables

| Variable                        | Description                                                                 | Required | Default |
|----------------------------------|-----------------------------------------------------------------------------|----------|---------|
| `sat_org_admin_account`          | Satellite organization admin username                                       | ✅       | —       |
| `sat_org_admin_account_password` | Satellite organization admin password                                       | ✅       | —       |
| `activation_key_name`            | Activation key used for registration                                        | ✅       | —       |
| `satellite_org_name`             | Name of the Satellite organization                                          | ✅       | —       |
| `satellite_host`                 | FQDN of the Satellite server                                                | ✅       | —       |
| `sat_reg_lifeycle_env`           | Lifecycle environment for registration                                      | ❌       | `false` |
| `sat_reg_insights_enabled`       | Whether to enable Red Hat Insights during registration                      | ❌       | `false` |
| `sat_reg_update_packages`        | Whether to update packages during registration                              | ❌       | `false` |
| `sat_reg_validate_certs`         | Whether to validate SSL certificates                                        | ❌       | `true`  |

---

## 🚀 Example: Register a Host to Satellite

```yaml
- name: Register host to Red Hat Satellite
  hosts: localhost
  gather_facts: false
  vars:
    sat_org_admin_account: admin
    sat_org_admin_account_password: secret
    activation_key_name: rhel-activation-key
    satellite_org_name: MyOrg
    satellite_host: satellite.example.com
    sat_reg_lifeycle_env: Dev
    sat_reg_insights_enabled: true
    sat_reg_update_packages: true
    sat_reg_validate_certs: true
  roles:
    - role: sat_reg
```

---

## 🔧 Tasks Overview

```yaml
- name: Generate registration command
  redhat.satellite.registration_command:
    username: "{{ sat_org_admin_account }}"
    password: "{{ sat_org_admin_account_password }}"
    activation_keys:
      - "{{ activation_key_name }}"
    organization: "{{ satellite_org_name }}"
    server_url: "https://{{ satellite_host }}"
    force: true
    validate_certs: "{{ sat_reg_validate_certs }}"
    lifecycle_environment: "{{ sat_reg_lifeycle_env }}"
    # packages: <packages to install when registered>
    # setup_remote_execution: false
    setup_insights: "{{ sat_reg_insights_enabled }} | default (false)"
    update_packages: "{{ sat_reg_update_packages }}"
  register: command

- name: Debug
  ansible.builtin.debug:
    msg: "{{ command.registration_command }}"

- name: Perform registration
  ansible.builtin.command: "{{ command.registration_command }}"
  register: script_result
  changed_when: "'changed' in script_result.stdout"
```

**Note:**  


---

## 📁 Structure

```
platform_infrastructure_automation/
├── galaxy.yml
├── README.md
├── meta/
│   └── runtime.yml
├── plugins/
│   └── README.md
├── roles/
│   └── sat_reg/
│       ├── defaults/main.yml
│       ├── vars/main.yml
│       ├── tasks/main.yml
│       ├── tasks/reg_to_sat.yml
│       ├── handlers/main.yml
│       ├── meta/main.yml
│       ├── tests/
│       │   ├── inventory
│       │   └── test.yml
│       └── README.md
```