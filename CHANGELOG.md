# Changelog

All notable changes to this collection will be documented in this file.

## [4.0.0] - 2025-06-27

### Added
- Initial release of the `fbi.platform_infra` Ansible collection ([galaxy.yml](platform_infrastructure_automation/galaxy.yml)).
- Role: `sat_reg` for Red Hat Satellite registration ([roles/sat_reg/README.md](platform_infrastructure_automation/roles/sat_reg/README.md)).
  - Tasks for generating and executing Satellite registration commands ([roles/sat_reg/tasks/reg_to_sat.yml](platform_infrastructure_automation/roles/sat_reg/tasks/reg_to_sat.yml)).
  - Default and variable files ([roles/sat_reg/defaults/main.yml](platform_infrastructure_automation/roles/sat_reg/defaults/main.yml), [roles/sat_reg/vars/main.yml](platform_infrastructure_automation/roles/sat_reg/vars/main.yml)).
  - Test playbook and inventory ([roles/sat_reg/tests/test.yml](platform_infrastructure_automation/roles/sat_reg/tests/test.yml), [roles/sat_reg/tests/inventory](platform_infrastructure_automation/roles/sat_reg/tests/inventory)).
- Role: `patching` for patch management ([roles/patching/tasks/main.yml](platform_infrastructure_automation/roles/patching/tasks/main.yml), [roles/patching/tasks/patch.yml](platform_infrastructure_automation/roles/patching/tasks/patch.yml)).
  - Package facts and patching logic ([roles/patching/tasks/package_facts.yml](platform_infrastructure_automation/roles/patching/tasks/package_facts.yml)).
  - Defaults for patching behavior ([roles/patching/defaults/main.yml](platform_infrastructure_automation/roles/patching/defaults/main.yml)).
- CI/CD pipeline for building and publishing the collection ([.gitlab-ci.yml](platform_infrastructure_automation/.gitlab-ci.yml)).
- Documentation:
  - Collection overview ([README.md](platform_infrastructure_automation/README.md)).
  - Plugins directory info ([plugins/README.md](platform_infrastructure_automation/plugins/README.md)).
- Example deployment playbooks for lab environments ([deploy-lab-jeff/deploy-gitlab.yml](deploy-lab-jeff/deploy-gitlab.yml), [deploy-lab-jeff/deploy-grafana.yml](deploy-lab-jeff/deploy-grafana.yml), [deploy-lab-jeff/deploy-rhbk.yml](deploy-lab-jeff/deploy-rhbk.yml), [deploy-lab-jeff/reg_to_sat.yml](deploy-lab-jeff/reg_to_sat.yml)).
- Ansible configuration for Galaxy servers ([deploy-lab-jeff/ansible.cfg](deploy-lab-jeff/ansible.cfg)).
- Collection requirements ([deploy-lab-jeff/collections/requirements.yml](deploy-lab-jeff/collections/requirements.yml)).

### Changed
- N/A (initial release)

### Fixed
- N/A (initial release)

---

> For detailed role and task documentation, see the respective `README.md` files in