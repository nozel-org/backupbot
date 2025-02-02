# backupbot

`backupbot` is a easy to use tool for creating (automatic) backups on FreeBSD. Currently files and mysql databases are supported, but more features can be added in the future.

## Features

* **Easy to use**: get started in just a couple of minutes.
* **Backup files and mysql databases**: daily and weekly backups with optional compression, splitting and encryption.
* **Configurable settings**: customize your backups, ownership, permissions, retention, schedule and random delay.
* **Made for FreeBSD**: compatible with basic shell.

## How to use

It's quite easy! Adjust the settings of `/usr/local/etc/backupbot.conf` to taste and run `backupbot --backup` to start the backup process. To effectuate the chosen schedule for automatic backups, use `backupbot --cron` and `backupbot` will take care of the rest.

If both `mysql` and `files` features have been enabled, the output of the backup will look something like this:
```
root@server:~ # ls -all -h /data/backup/
drwxr-xr-x  2 root  wheel   512B Jul  1 15:36 .
drwxr-xr-x  4 root  wheel   1.0K Jun 29 23:52 ..
-rw-r--r--  1 root  wheel   162K Jul  1 03:00 200701T0300-blog.sql.xz
-rw-r--r--  1 root  wheel   896M Jul  1 03:00 200701T0300-files.tar.xz
-rw-r--r--  1 root  wheel    66K Jul  1 03:00 200701T0300-cloud_test.sql.xz
-rw-r--r--  1 root  wheel    16M Jul  1 03:00 200701T0300-wp.sql.xz
```

## How to install

Use [botmanager](https://codeberg.org/nozel/botmanager) or install manually:

1. Copy `backupbot` to `/usr/local/bin/backupbot` (owner=`root`, group=`wheel`, permissions=`555` (read & execute).
2. Copy `backupbot.conf` to `/usr/local/etc/backupbot.conf` and adjust the settings to taste.
3. Optionally add the chosen schedule to a automatic cronjob with `backupbot --cron`.

This will look something like:
```
# install backupbot
wget https://codeberg.org/nozel/backupbot/raw/branch/master/backupbot -O /usr/local/bin/backupbot
chown root:wheel /usr/local/bin/backupbot
chmod 555 /usr/local/bin/backupbot
wget https://codeberg.org/nozel/backupbot/src/branch/master/backupbot.conf -O /usr/local/etc/backupbot.conf
nano /usr/local/etc/backupbot.conf
backupbot --cron
```

## Support

The [manual](https://codeberg.org/nozel/backupbot/src/branch/master/MANUAL.md) provides some more insight in to backupbot. If you have questions, suggestion or find bugs, please let us know via Issues.

## Changelog

### 1.8.1-RELEASE (06-12-2024)

- Fixed a bug with compression in feature MySQL #40.

### 1.8.0-RELEASE (01-12-2024)

- Fixed general styling issues and typos.
- Made cron file location configurable #30.
- Refactored error messages to a general error function #32.
- Removed old references to serverbot #31.
- Added support for logger #34.
- Made preparations for custom log file support #38.
- Made preparations for zfs snapshot support #28.
- Added support for the lz4 compression algorithm via lz4 #36.
- Added support for the Zstandard compression algorithm via zstd #36.
- Added support for the LZMA compression algorithm via lrzip #36. This requires the package lrzip to be installed.
- Added support for the LZMA compression algorithm via lzip #36. This requires the package lzip to be installed.
- Added support for the LZO compression algorithm via lzop #36. This requires the package lzop to be installed.
- Added support for the gzip compression algorithm via pigz (parallel) #36. This requires the package pigz to be installed.
- Added support for the bzip2 compression algorithm via pbzip2 (parallel) #36. This requires the package pbzip2 to be installed.
- Added support for the LZMA compression algorithm via plzip (parallel) #36. This requires the package plzip to be installed.
- Added an option to automatically split file backups in to smaller parts #37.

### 1.7.1-RELEASE (21-05-2023)

- Fixed layout inconsistency in log format.
- Fixed legacy reference to /usr/bin in option_cron.

### 1.7.0-RELEASE (18-12-2022)

- Added configurable automatic permissions of backups.
- Log entries are now more informational.
- Added a optional random delay for backups to spread out the potential load of multiple servers starting their backups at the same time.
- Error messages are more clear and refer to the relevant variable in the configuration file.
- Replaced echo with printf for consistency.

### 1.6.0-RELEASE (27-05-2022)

- Added configurable automatic ownership of backups.
- Added rudimentary logging of backupbot's actions.

### 1.5.1-RELEASE (20-01-2022)

- Fixed a bug that would show an error when using --version or --help was used if the configuration file wasn't configured properly.

### 1.5.0-RELEASE (18-01-2022)

- Refactored encryption feature to be configurable for every backup feature instead of it being a general setting.
- Simplified the configuration of encryption by reducing the required configuration variable to one.
- Improved default settings and removed unneeded checks on default values.
- Fixed a bug where choosing 00:00 as the daily backup time would not work properly.
- Expanded and improved the backupbot manual.

### 1.4.0-RELEASE (16-01-2022)

- Added daily backup and weekly backup cycles.
- Extended automatic cron generation to include daily and weekly backup cycles.
- Extended retention to include daily and weekly backup cycles.
- Changed backup file names to reflect whether its a daily, weekly or manual backup.
- Changed the order of backup file name components to be more easy to read.
- Made default configuration file more compact and easy to read.
- Made some variable names more consistent.

### 1.3.0-RELEASE (15-01-2022)

- Added more compression alternatives: no compression, gzip, bzip2 and xz.

### 1.2.2-RELEASE (14-01-2022)

- Fixed a bug in retention feature.
- Switched from STABLE to RELEASE tag for releases.

### 1.2.1-STABLE (11-01-2022)

- Refactored retention feature to keep working when user switches from encrypted to unencrypted backups and vice versa.

### 1.2.0-STABLE (16-10-2021)

- Added support for automatic removal of old backups/retention.

### 1.1.0-STABLE (19-01-2021)

- Added support for symmetrical encryption of backups.

### 1.0.0-STABLE (01-07-2020)

- First stable release.
- Added support for backing up files.
- Added support for backing up mysql databases.
- Added support for automatic backup based on cron.
- Added creation of cronjob based on backupbot.conf.
- Added configurable settings for backup features and cron in backupbot.conf.
