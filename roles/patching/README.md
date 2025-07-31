# Patching

Role that facilitates the patching of a given server.

## Requirements

Your ansible environment should be setup according to the Linux team standard. In order to setup your environment, run the "setup-ansible-environment" playbook. This can be found at */common/ansible/enclave_data/setup-ansible-env.yml* on every major enclave.

## Role Variables

- `check_mountpoints` - Used in conjunction with `recon.yml`. Configure whether to check server mountpoints. Defaults to *true*.
- `do_not_restart` - Used in conjunction with `patch.yml`. Configure whether to not restart servers. Defaults to *false*.
- `force_restart` - Used in conjunction with `patch.yml`. Configure whether to force restart servers. Defaults to *false*.
- `update_bigfix` - Used in conjunction with `patch.yml`. COnfigure whether to update the BigFix client. Defaults to *true*. This option will be ignored if `bigfix_rpm` and `bigfix_license` is undefined, of which these values are usually defined in Enclave Data.
- `zabbix` - Used in conjunction with `patch.yml`. Configure whether to create a Zabbix maintenance window for target servers during patching operations.
- `recipient` - Used in conjunction with `patch.yml` and `pre_patch_email.yml` and `post_patch_email.yml`. Recipient for email notifications.
- `app_name` - Used in conjunction with `patch.yml` and `pre_patch_email.yml` and `post_patch_email.yml`. Application name of servers being patched. Used in email notifications.
- `prod_status` - Used in conjunction with `patch.yml` and `pre_patch_email.yml` and `post_patch_email.yml`. Production status of servers being patched. Used in email notifications. 
- `np_start_date` - Used in conjunction with `pre_patch_email.yml`. Start date of non-production patching.
- `r_start_date` - Used in conjunction with `pre_patch_email.yml`. Start date of rolling production patching.
- `p_start_date` - Used in conjunction with `pre_patch_email.yml`. Start date of production patching.
- `exclude_list` - Used in conjunction with `patch.yml`. List of packages to exclude from patching.
- `disable_repo` - Used in conjunction with `patch.yml`. Configure which repo should be excluded from patching.
- `package_cleanup` - Used in conjunction with `patch.yml`. Configure whether to clean up old kernels from */boot*. Defaults to *false*.

Additonally, when calling this role, it is always recommended to use `tasks_from`. This allows you to choose the tasks that are actually carried out, instead of defaulting to the main tasks file.

- `package_facts.yml` - A set of tasks that grab an the ID of a phpIPAM record via a given IP.
- `patch.yml` - A set of tasks that facilitate the removal of a server record in phpIPAM.
- `post_patch_mmail.yml` - A set of tasks that facilitates the renaming of a hostname inside an existing record in IPAM.
- `pre_patch_mail.yml` - A set of tasks that send out a pre-patching email notification to customers notifying them of what servers are being patched at what time.
- `recon.yml` - A set of tasks to recon a server for issues prior to patching.
- `repo_facts.yml` - A set of tasks to gather information about enabled repositories.

## Dependencies

No dependencies.

## Example Playbook

```yaml
---
- name: Setup Environment
  hosts: "{{ aap | default('localhost') }}"
  become: false
  gather_facts: false
  collections: linux.main
  environment:
    TMPDIR: "{{ lookup('env', 'HOME') }}/ansible/tmp"

## Playbook Tasks
  tasks:
    - name: Make sure playbooks directory is up to date
      ansible.builtin.git:
        repo: "git@{{ (hostvars[inventory_hostname]['servers']['git'].keys()| list)[0] }}:linux-team/playbooks.git"
        dest: "{{ playbook_dir }}/../"
        update: true
        force: true
        version: "{{ playbook_branch | default('HEAD') }}"
      when: git | default('true') | bool
      notify:
        - failed_playbooks

    - name: Update or clone roles
      ansible.builtin.include_role:
        name: clone_or_update_roles
      when: git | default('true') | bool

  handlers:
    - name: failed_playbooks
      ansible.builtin.fail:
        msg: Playbooks directory was not up to date with git server. It has been updated. Please run new-build playbook again.

- hosts: "localhost"
  gather_facts: false
  tasks:
    - name: Add Host to Inventory Group 'aap'
      ansible.builtin.add_host:
        name: '{{ host_target }}'
        groups: aap
      when: host_target is defined and host_target

- name: Patching Playbook
  hosts: "{{ target | default('all') }}"
  collections: linux.main
  gather_facts: true
  become: true
  tasks:
  - name: Import Patching Role
    include_role:
      name: patching
```

## License

No License.

## Author Information

- Tyler Kness-Miller (trkness-miller@ado.gov)
