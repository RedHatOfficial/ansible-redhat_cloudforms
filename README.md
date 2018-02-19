# ansible-redhat_cloudforms
Ansible playbooks for Red Hat CloudForms Management Engine (CFME).

## Playbooks
Playbooks provided by this project.

### migrate-5-7-to-5-8.yml
Performs a migration from CFME 5.7 to 5.8 utilizing steps from [Migrating to Red Hat CloudForms 4.5](https://access.redhat.com/documentation/en-us/red_hat_cloudforms/4.5/html/migrating_to_red_hat_cloudforms_4.5/).  This playbook does not currently perform a backup, resize the disks or handle database replication scenarios.

#### Assumptions
* Appliances have already been backed up per [General Configuration Section 4.4.5.1](https://access.redhat.com/documentation/en-us/red_hat_cloudforms/4.5/html/general_configuration/configuration#backing-up-and-restoring-a-database).
* Disks have already been resized.
* Environments are not utilizing database replication.
* Appliances have had memory increased to 12Gb for CloudForms 4.5 and newer appliances per requirement.

#### Required groups
* cfme
* cfme-appliancees
* cfme-databases

### migrate-5-8-to-5-9.yml
Performs a migration from CFME 5.8 to 5.9 utilizing steps from [Migrating to Red Hat CloudForms 4.6 Beta](https://access.redhat.com/documentation/en-us/red_hat_cloudforms/4.6-beta/html/migrating_to_red_hat_cloudforms_4.6_beta/).  This playbook does not currently perform a backup, resize the disks or handle database replication scenarios.

#### Assumptions
* Appliances have already been backed up per [General Configuration Section 4.4.5.1](https://access.redhat.com/documentation/en-us/red_hat_cloudforms/4.5/html/general_configuration/configuration#backing-up-and-restoring-a-database).
* Disks have already been resized. (Only necessary if migrating from CFME 5.8.0.17 or earlier)
* Environments are not utilizing database replication.

#### Required groups
* cfme
* cfme-appliancees
* cfme-databases
