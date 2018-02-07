# ansible-redhat_cloudforms
Ansible playbooks for Red Hat CloudForms Management Engine (CFME).

## Playbooks
Playbooks provided by this project.

### migrate-5-7-to-5-8.yml
Performs a migration from CFME 5.7 to 5.8 utilizing steps from [Migrating to Red Hat CloudForms 4.5](https://access.redhat.com/documentation/en-us/red_hat_cloudforms/4.5/html/migrating_to_red_hat_cloudforms_4.5/).  This playbook does not currently perform a backup, resize the disks or handle database replication scenarios.

#### Assumptions
* Appliances have already been backed up.
* Disks have already been resized.
* Environments are not utilizing database replication.

#### Required groups
* cfme
* cfme-appliancees
* cfme-databases
