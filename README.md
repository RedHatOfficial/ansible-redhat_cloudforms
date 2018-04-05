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

#### Options
| parameter                    | required | default | choices | comments
|------------------------------|----------|---------|---------|-------------------------------------------------------------------
| cfme_addiitonal_repositories | No       |         |         | Additional repositories to configure when performing the migration

### migrate-5-8-to-5-9.yml
Performs a migration from CFME 5.8 to 5.9 utilizing steps from [Migrating to Red Hat CloudForms 4.6](https://access.redhat.com/documentation/en-us/red_hat_cloudforms/4.6/html/migrating_to_red_hat_cloudforms_4.6/).  This playbook does not currently perform a backup, resize the disks or handle database replication scenarios.

#### Assumptions
* Appliances have already been backed up per [General Configuration Section 4.4.5.1](https://access.redhat.com/documentation/en-us/red_hat_cloudforms/4.5/html/general_configuration/configuration#backing-up-and-restoring-a-database).
* Disks have already been resized. (Only necessary if migrating from CFME 5.8.0.17 or earlier)
* Environments are not utilizing database replication.

#### Required groups
* cfme
* cfme-appliancees
* cfme-databases

#### Options
| parameter                    | required | default | choices | comments
|------------------------------|----------|---------|---------|-------------------------------------------------------------------
| cfme_addiitonal_repositories | No       |         |         | Additional repositories to configure when performing the migration

### rolling-update.yml
Performs an update/upgrade of all packages on the CFME appliances and performs a reboot if necessary.

#### Required groups
* cfme
* cfme-appliancees
* cfme-databases

#### Options
| parameter                    | required | default | choices | comments
|------------------------------|----------|---------|---------|-------------------------------------------------------------------
| cfme_addiitonal_repositories | No       |         |         | Additional repositories to configure when performing the update

### smoke-test-service-provision.yml
Smoke tests Service template provisioning and retirment.

#### Required groups
* cfme-ui-appliances

#### Options
| parameter                                 | required | default | comments
|-------------------------------------------|----------|---------|----------------------------------------------------------
| cfme\_api\_user                           | Yes      |         | API user
| cfme\_api\_password                       | Yes      |         | API password
| cfme\_service\_catalog\_name              | Yes      |         | Service Catalog that contains the Service Tempalte to test
| cfme\_service\_template\_name             | Yes      |         | Service Teamplte to test
| cfme\_provision\_service\_dialog\_options | Yes      |         | Hash of dialog options to pass to the Service Template creation request
| cfme\_provision\_service\_retries         | No       | 60      | Number of attempts at waiting for Provision Service task to complete
| cfme\_provision\_service\_delay           | No       | 60      | Number of seconds between attempts at waiting for Provion Service task to complete
| cfme\_retire\_service\_retries            | No       | 60      | Number of attempts at waiting for Retire Service task to complete
| cfme\_retire\_service\_delay              | No       | 60      | Number of seconds between attempts at waiting for Retire Service task to complete

### start-services.yml
Start all of the DB services then all of the Appliance services.

#### Required groups
* cfme-appliancees
* cfme-databases

### stop-services.yml
Stop all of the Appliance services then all of the DB services.

#### Required groups
* cfme-appliancees
* cfme-databases

### full-stop-start-services.yml
Stop all of the Appliance services, then all of the DB services, then start all of the DB services, then all of the Appliance services, then perform a helath check.

#### Required groups
* cfme-appliancees
* cfme-databases

### health-check.yml
Checks the health of the Appliances.

#### Required groups
* cfme-appliancees

### gather-logs.yml
Gathers relevant logs from all of the appliances and puts them in an archive on the first DB host. Optionally emails the archive.

#### Required groups
* cfme-databases

#### Options
| parameter                                 | required | default | comments
|-------------------------------------------|----------|-------------------------------------------------------------------
| cfme\_gather\_logs\_smtp\_host            | No       |         | SMTP host to send log archive email through. If not specified no email will be sent.
| cfme\_gather\_logs\_smtp\_port            | No       |         | SMTP port to send log archive email through. If not specified no em
    ail will be sent.
| cfme\_gather\_logs\_email\_from           | No       |         | Email addresses to send the log archive email from. If not specified no em
    ail will be sent.
| cfme\_gather\_logs\_email\_to             | No       |         | Email addresses to send the log archive email to. If not specified no em
    ail will be sent.
