# CORD platform-install

This repository contains [Ansible](http://docs.ansible.com) playbooks for
installing and configuring software components that build a CORD POD:
OpenStack, ONOS, and XOS.

It is used as a sub-module of the [main CORD
repository](https://github.com/opencord/cord).

### Credentials

Credentials will be autogenerated and placed in the `credentials/` directory
when the playbooks are run, where the credential name is the filename, and the
contents of the file is the password.

For most profiles the XOS admin user is named `xosadmin@opencord.org`.

### Creating a new CORD profile

To create a new CORD profile, you should create a .yaml variables file in
`profile_manifests/` with the name of your profile (ex: `my-profile.yaml`), and
populate it with your configuration.

### Making changes and lint checking your changes

Before commit, please run `./scripts/lintcheck.sh .` in the repo root, which
will perform the same [ansible-lint](https://pypi.python.org/pypi/ansible-lint)
check that Jenkins performs when in review in gerrit.

## Specific profiles notes

### opencloud

Used as a part of the [OpenCloud](http://www.opencloud.us/) deployment. Similar
to `rcord`.

### rcord

This is a part of the [R-CORD](https://github.com/opencord/cord) deployment -
start using the steps specified in that repo.

This profile is designed to integrate XOS with physical infrastructure pieces
like MaaS, OpenStack, and ONOS.  See the [CORD-in-a-Box Quick Start
Guide](https://github.com/opencord/cord/blob/master/docs/quickstart.md) for how
to set up a virtual multi-node R-CORD pod on a single host.

### ecord

E-CORD description goes here.

### mcord

M-CORD description goes here.

## Design Notes for Developers

### Variables used in platform-install

`cord_profile`: name of the profile_manifest to use.

Paths on configuration node (where playbooks are run, may also be build node)

 - `config_cord_dir` location on configuration node of cord dir
 - `config_cord_profile_dir` location on configuration node of cord_profile dir
 - `pki_dir`, `ssh_pki_dir`: where SSL and SSH certificates are created on
   config node
 - `credentials_dir` - location where autogenerated passwords file are created

Paths on head node (target system operated on by playbooks)

 - `head_cord_dir` - where the `cord` directory is copied to on the head node
   (deprecated when we reach container-only deploys)
 - `head_cord_profile_dir` - location of the `cord_profile` directory on the
   head node
 - `head_onos_cord_dir` - location of the `onos-cord` directory on the head
   node
 - `head_onos_fabric_dir` - location of the `onos-fabric` directory on the head
   node

### Style notes

Prefix every role file with the yaml start block and comment with name of file
relative to role base.

```
---
# rolename/tasks/main.yml
```

When using templates, put the template filename and path within the role as a
comment in the template so that it's easy to determine which template was used
to create a file after it's been created.

If you use a variable that isn't created by the ansible setup task, define it
in the role defaults file. The default value for anay variable must be the same
across all role defaults.

Use the YAML style syntax for tasks, not the older `=` syntax, as the former is
more likely to indent and syntax highlight properly in most editors.

Roles should always have the same outcome, so avoid using conditionals or tags
to change the behavior of a role. These features should only be used for
avoiding time-consuming tasks in an idempotent manner.

