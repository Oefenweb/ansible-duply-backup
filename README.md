## duply-backup

[![Build Status](https://travis-ci.org/Oefenweb/ansible-duply-backup.svg?branch=master)](https://travis-ci.org/Oefenweb/ansible-duply-backup) [![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-duply--backup-blue.svg)](https://galaxy.ansible.com/Oefenweb/duply-backup)

Set up [duplicity](http://duplicity.nongnu.org/) backups using [duply](http://duply.net/) in Debian-like systems.

#### Requirements

None

#### Variables

* `duply_backup_profiles`: [default: `{}`]: Duply backup profiles
* `duply_backup_profiles.key`: The name of the profile (e.g. `etc`)
* `duply_backup_profiles.key.conf`: Conf declarations
* `duply_backup_profiles.key.conf.gpg_key`: Encrypt with key (**optional**, omitting will disable encryption)
* `duply_backup_profiles.key.conf.gpg_pw`: Symmetric encryption using passphrase only (**optional**)
* `duply_backup_profiles.key.conf.gpg_keys_enc`: Public key to encrypt to (**optional**)
* `duply_backup_profiles.key.conf.gpg_key_sign`: A secret key for signing (**optional**)
* `duply_backup_profiles.key.conf.gpg_pw_sign`: Signing key passphrase (**optional**)
* `duply_backup_profiles.key.conf.gpg`: Override the used gpg executable, defaults to `gpg` (**optional**)
* `duply_backup_profiles.key.conf.gpg_opts`: GPG options passed from duplicity to gpg process (**optional**)
* `duply_backup_profiles.key.conf.gpg_test`: Disable preliminary tests (**optional**, set to `false` to disable)
* `duply_backup_profiles.key.conf.target`: Location of the backup target, [see duplicity manpage](http://duplicity.nongnu.org/duplicity.1.html#sect7)
* `duply_backup_profiles.key.conf.target_user`: Username of the backup target (**optional**)
* `duply_backup_profiles.key.conf.target_pass`: Password of the backup target (**optional**)
* `duply_backup_profiles.key.conf.export`: Environment variables to set (**optional**)
* `duply_backup_profiles.key.conf.export.{n}.name`: Name of the variable (e.g. `AWS_ACCESS_KEY_ID`)
* `duply_backup_profiles.key.conf.export.{n}.value`: Value of the variable (e.g. `26E1A49CFE81D46C83C2`)
* `duply_backup_profiles.key.conf.source`: Base directory to backup
* `duply_backup_profiles.key.conf.dupl_precmd`: A command that runs duplicity (**optional**)
* `duply_backup_profiles.key.conf.python`: Override the used python interpreter, defaults to `python` (**optional**)
* `duply_backup_profiles.key.conf.exclude_if_present`: Exclude if present file name (**optional**)
* `duply_backup_profiles.key.conf.max_age`: Time frame for old backups to keep (**optional**)
* `duply_backup_profiles.key.conf.max_full_backups`: Number of full backups to keep (**optional**)
* `duply_backup_profiles.key.conf.max_fulls_with_incrs`: Number of full backups for which incrementals will be kept for (**optional**)
* `duply_backup_profiles.key.conf.max_fullbkp_age`: Forces a full backup if last full backup reaches a specified age (**optional**)
* `duply_backup_profiles.key.conf.volsize`: Sets the size of backup chunks (**optional**, in MB's)
* `duply_backup_profiles.key.conf.verbosity`: Verbosity of output (**optional**)
* `duply_backup_profiles.key.conf.temp_dir`: Temporary file space. At least the size of the biggest file in backup for a successful restoration process (**optional**)
* `duply_backup_profiles.key.conf.arch_dir`: Defines a folder that holds unencrypted meta data of the backup, enabling new incrementals without the need to decrypt backend metadata first (**optional**)
* `duply_backup_profiles.key.conf.time_separator`: Enables duplicity `--time-separator` option (**optional**, **deprecated**, set to `true` to enable)
* `duply_backup_profiles.key.conf.short_filenames`: Enables duplicity `--short-filenames` option (**optional**, **deprecated**, set to `true` to enable)
* `duply_backup_profiles.key.conf.dupl_params`: Additional duplicity command line options. Don't forget to leave a separating space char at the end (**optional**)
* `duply_backup_profiles.key.pre`: Pre script (**optional**)
* `duply_backup_profiles.key.post`: Post script (**optional**)
* `duply_backup_profiles.key.excludes`: A list of glob patterns of included or excluded files / folders (**optional**)

* `duply_backup_gpg_pub_keys`: [default: `[]`]: Public keys to import
* `duply_backup_gpg_ownertrusts`: [default: `[]`]: Owner trusts to import
* `duply_backup_gpg_sec_keys`: [default: `[]`]: Private keys to import

* `duply_backup_jobs`: [default: `[]`]: Duply backup jobs (scheduled by `cron.d`)
* `duply_backup_jobs.{n}.name`: [required]: Description of a crontab entry, should be unique, and changing the value will result in a new cron task being created (e.g. `duply-backup-etc`)
* `duply_backup_jobs.{n}.job`: [required]: The command to execute (e.g. `/usr/local/bin/duply etc backup`)
* `duply_backup_jobs.{n}.state`: [default: `present`]: Whether to ensure the job is present or absent
* `duply_backup_jobs.{n}.day`: [default: `*`]: Day of the month the job should run (`1-31`, `*`, `*/2`)
* `duply_backup_jobs.{n}.hour`: [default: `*`]: Hour when the job should run (e.g. `0-23`, `*`, `*/2`)
* `duply_backup_jobs.{n}.minute`: [default: `*`]: Minute when the job should run (e.g. `0-59`, `*`, `*/2`)
* `duply_backup_jobs.{n}.month`: [default: `*`]: Month of the year the job should run (e.g `1-12`, `*`, `*/2`)
* `duply_backup_jobs.{n}.weekday`: [default: `*`]: Day of the week that the job should run (e.g. `0-6` for Sunday-Saturday, `*`)

## Dependencies

None

## Recommended

* `ansible-duplicity` ([see](https://github.com/Oefenweb/ansible-duplicity), to get the latest version of `duplicity`)
* `ansible-duply` ([see](https://github.com/Oefenweb/ansible-duply), to get the latest version of `duply`)

#### Example(s)

##### Simple configuration (GPG disabled)

```yaml
---
- hosts: all
  roles:
  - duply-backup
  vars:
    duply_backup_profiles:
      etc:
        conf:
          target: 'file:///tmp/duply-backup/target'
          source: '/etc'
        excludes:
          - '+ /etc/skel'
          - '- **'
```

##### Advance configuration (GPG enabled)

On your Ansible control machine machine in `../../../files/duply-backup/`.

###### Generate a GPG key pair
```sh
gpg --batch --gen-key <<EOF
> %echo Generating a GPG key
> Key-Type: RSA
> Key-Length: 4096
> Subkey-Type: RSA
> Subkey-Length: 4096
> Name-Real: Duplicity Backup
> Name-Comment: localhost.localdomain
> Name-Email: root@localhost.localdomain
> Expire-Date: 0
> Passphrase: QUsNMnD6GHUTFSBruSwbJpZBhto=
> %commit
> %echo Done
> EOF
gpg: Generating a GPG key
.....+++++
........+++++
.....+++++
.........................+++++
gpg: key E6564C6E marked as ultimately trusted
gpg: Done
```

###### Export the public key
```sh
gpg --output E6564C6E.pub.asc --armor --export E6564C6E;
```

###### Export the ownertrust (value 6, ultimately trusted)
```sh
echo "$(gpg --with-fingerprint E6564C6E.pub.asc | grep fingerprint | tr -d '[:space:]' | awk 'BEGIN { FS = "=" } ; { print $2 }'):6:" 1> E6564C6E.ownertrust.txt;
```

###### Export the private key
```sh
gpg --output E6564C6E.sec.asc --armor --export-secret-key E6564C6E;
```

```yaml
---
- hosts: all
  roles:
  - duply-backup
  vars:
    duply_backup_profiles:
      etc:
        conf:
          gpg_key: 'E6564C6E'
          gpg_pw: 'QUsNMnD6GHUTFSBruSwbJpZBhto='
          gpg_key_sign: 'E6564C6E'
          gpg_pw_sign: 'QUsNMnD6GHUTFSBruSwbJpZBhto='
          gpg_opts: '--pinentry-mode loopback'
    
          target: 'file:///data/backup/etc'
          source: '/etc'
    
          max_age: 1W
          max_fullbkp_age: 1M
          
          verbosity: 5
          
          arch_dir: '/data/backup/.duply-cache'
        pre: ../../../files/duply-backup/pre
        post: ../../../files/duply-backup/post
        excludes:
          - '+ /etc/skel'
          - '- **'
    
    duply_backup_gpg_pub_keys:
      - ../../../files/duply-backup/E6564C6E.pub.asc
    duply_backup_gpg_ownertrusts:
      - ../../../files/duply-backup/E6564C6E.ownertrust.txt
    duply_backup_gpg_sec_keys:
      - ../../../files/duply-backup/E6564C6E.sec.asc
    duply_backup_jobs:
      - name: duply-backup-etc
        job: /usr/local/bin/duply etc backup
        minute: "0"
```

#### License

MIT

#### Author Information

Mischa ter Smitten

#### Feedback, bug-reports, requests, ...

Are [welcome](https://github.com/Oefenweb/ansible-duply-backup/issues)!
