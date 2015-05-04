## duply-backup

[![Build Status](https://travis-ci.org/Oefenweb/ansible-duply-backup.svg?branch=master)](https://travis-ci.org/Oefenweb/ansible-duply-backup) [![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-duply--backup-blue.svg)](https://galaxy.ansible.com/list#/roles/3585)

Set up [duplicity](http://duplicity.nongnu.org/) backups using [duply](http://duply.net/) in Debian-like systems.

#### Requirements

None

#### Variables

None

## Dependencies

None

## Recommended

* `ansible-duplicity` ([see](https://github.com/Oefenweb/ansible-duplicity), to get the latest version of `duplicity`)
* `ansible-duply` ([see](https://github.com/Oefenweb/ansible-duply), to get the latest version of `duply`)

#### Example

```yaml
---
- hosts: all
  roles:
  - duply-backup
```

#### License

MIT

#### Author Information

Mischa ter Smitten

#### Feedback, bug-reports, requests, ...

Are [welcome](https://github.com/Oefenweb/ansible-duply-backup/issues)!
