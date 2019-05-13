# ansible-redhat_cloudforms
This is a *community supported* repository of ansible playbooks for automating the management of ManageIQ and/or Red Hat CloudForms Management Engine (CFME).

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
| cfme_additional_repositories | No       |         |         | Additional repositories to configure when performing the migration
| sat6_org_id                  | No       |         |         | Satellite 6 organization ID (when using activation key below)
| sat6_activation_key          | No       |         |         | Satellite 6 activation key (instead of direct subscribe to repos)

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
| cfme_additional_repositories | No       |         |         | Additional repositories to configure when performing the migration
| sat6_org_id                  | No       |         |         | Satellite 6 organization ID (when using activation key below)
| sat6_activation_key          | No       |         |         | Satellite 6 activation key (instead of direct subscribe to repos)

### rolling-update.yml
Performs an update/upgrade of all packages on the CFME appliances and performs a reboot if necessary.

#### Required groups
* cfme
* cfme-appliancees
* cfme-databases

#### Options
| parameter                    | required | default | choices | comments
|------------------------------|----------|---------|---------|-------------------------------------------------------------------
| cfme_additional_repositories | No       |         |         | Additional repositories to configure when performing the update
| sat6_org_id                  | No       |         |         | Satellite 6 organization ID (when using activation key below)
| sat6_activation_key          | No       |         |         | Satellite 6 activation key (instead of direct subscribe to repos)

### simultaneous-update.yml
Performs an update/upgrade of all packages on all CFME appliances at the same time, and performs a reboot if necessary.

#### Required groups
* cfme
* cfme-appliancees
* cfme-databases

#### Options
| parameter                    | required | default | choices | comments
|------------------------------|----------|---------|---------|-------------------------------------------------------------------
| cfme_addiitonal_repositories | No       |         |         | Additional repositories to configure when performing the update
| sat6_org_id                  | No       |         |         | Satellite 6 organization ID (when using activation key below)
| sat6_activation_key          | No       |         |         | Satellite 6 activation key (instead of direct subscribe to repos)

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
| parameter                       | required | default | comments
|---------------------------------|----------|---------|-------------------------------------------------------------------------------------
| cfme\_gather\_logs\_smtp\_host  | No       |         | SMTP host to send log archive email through. If not specified no email will be sent.
| cfme\_gather\_logs\_smtp\_port  | No       |         | SMTP port to send log archive email through. If not specified no email will be sent.
| cfme\_gather\_logs\_email\_from | No       |         | Email addresses to send the log archive email from. If not specified no email will be sent.
| cfme\_gather\_logs\_email\_to   | No       |         | Email addresses to send the log archive email to. If not specified no email will be sent

### install-vddk.yml
Installs the VMware Virtual Disk Development Kit (VDDK).  This is required to enable Smart State Analysis (SSA) for VMware hosts.

* You will need a VMware login to download the VDDK.  We do not provide this due to VMware licensing requirements.

#### Options
| parameter        | required | default | comments
|------------------|----------|---------|--------------------------------------------------------------------
| cfme\_vddk\_path | Yes      |         | This is the file path, on the control node, of the downloaded VDDK.

### uninstall-vddk.yml
Uninstalls the VMware Virtual Disk Development Kit (VDDK).  This can be utilized in conjunction with install-vddk.yml when an upgrade of the VDDK is required.

### install-webmks-sdk.yml
Installs the VMware HTML Console SDK.  This is required to enable WebMKS for VMware Console Support.

* You will need a VMware login to download the SDK.  We do not provide this due to VMware licensing requirements.

#### Options
| parameter        | required | default | comments
|------------------|----------|---------|--------------------------------------------------------------------
| cfme\_webmks_sdk\_path | Yes      |         | This is the file path, on the control node, of the downloaded SDK.

### uninstall-webmks-sdk.yml
Removes the VMware HTML Console SDK.  This disables  WebMKS for VMware Console Support.

### change_vmdb_password.yml
Changes the VMDB PostgreSQL password, updates the database.yml files, and restarts evmserverd.

#### Assumptions
* vmdb_password is defined in files/vmdb_password.yml which is encrypted using Ansible Vault.
* The password of the vault is `smartvm`.
* The password of the vault can be changed with `ansible-vault rekey files/vmdb_password.yml`.
* The VMDB password to be used can be set by editing the vault with `ansible-vault edit files/vmdb_password.yml`.
* The playbook can be run with `ansible-playbook --vault-id @prompt playbooks/change_vmdb_password.yml` to prompt for the Ansible Vault password.

#### Required groups
* cfme-appliancees
* cfme-databases

#### Options
| parameter        | required | default | comments
|------------------|----------|---------|--------------------------------------------------------------------
| vmdb\_password   | Yes      |         | This is the new password to use for the VMDB PostgreSQL account.

### disable-medium-ssl-ciphers.yml
Disables Medium SSL Ciphers in the CloudForms webserver configuration (/etc/httpd/conf.d/manageiq-https-application.conf).
* Addresses Tenable finding https://www.tenable.com/plugins/nessus/42873

#### Required groups
* cfme-appliancees
