# Borg + RClone Autobackup

Easily automate backups using Borg + RClone.

## Usage Examples

### Docker Run

```
docker run \
    -v /home/mannkind:/data:ro \
    -e BACKUP_NAME="frodo" \
    -e BACKUP_LOCATION="b2:AllMyBorgBackups/frodo" \
    -e BACKUP_SCHEDULE="0 2 * * *" \
    -e BACKUP_ENCRYPTION_KEY="One ring to rule them all oh you know the rest" \
    -e BACKUP_PRUNE="--keep-daily=7 --keep-weekly=4" \
    -e BACKUP_NOW="true" \
    -e B2_ID="kiacup6326is" \
    -e B2_KEY="12xd5t3891tqh1qqw1qq3kmhl9hbd9lfugz2j32d" \
    --restart unless-stopped \
    mannkind/borg-rclone-autobackup
```

### Docker Compose

```
version: 3
services:
    borg-rclone-autobackup:
        image: mannkind/borg-rclone-autobackup
        restart: unless-stopped
        volumes:
            - "/home/mannkind:/data:ro"
        environment:
            BACKUP_NAME: "frodo"
            BACKUP_LOCATION: "b2:AllMyBorgBackups/frodo"
            BACKUP_SCHEDULE: "0 2 * * *"
            BACKUP_ENCRYPTION_KEY: "One ring to rule them all one ring to find them one ring to bring them all and in the darkness bind them"
            BACKUP_PRUNE: "--keep-daily=7 --keep-weekly=4"
            BACKUP_NOW: "true"
            B2_ID: "kiacup6326is"
            B2_KEY:"12xd5t3891tqh1qqw1qq3kmhl9hbd9lfugz2j32d"
```

## Volumes

The following volumes can be used

  * /data - The data to backup
  * /backups - The location of the backups; backup in the container by default

## Environment Variables

The following environment variables setup Borg

  * `BACKUP_NAME`: The backup name
  * `BACKUP_ENCRYPTION_KEY`: The backup encryption key
  * `BACKUP_SCHEDULE`: Cron scheduled string
  * `BACKUP_LOCATION`: The backup location e.g. `b2:my-backup/some-host`
  * `BACKUP_PRUNE`: The backup prune string e.g. `--keep-daily=7 --keep-weekly=4`
  * `BACKUP_NOW`: Runs the backup immediately, instead of waiting for the scheduled time
  * `BORG_CUSTOM_ARGS`: python-style list with extra args to pass to borg, e.g. `"['--exclude','path/to/exclude1','--exclude','/path/to/exclude2']"

The following environment variables setup B2 as the RClone target

  * `B2_ID`: The backblaze id
  * `B2_KEY`: The backblaze key

The following environment variables set up Google Cloud Storage as the RClone target:

  * `GCS_PROJECT_NUMBER`: The project number for your Google Cloud project.

The following environment variables configure email alerts:

 * `EMAIL_HOST`: SMTP server URL
 * `EMAIL_USER`: SMTP username
 * `EMAIL_PASS`: SMTP password
 * `EMAIL_USE_TLS`: Whether to use TLS for the SMTP connection, if not set SSL is used.
 * `EMAIL_PORT`: SMTP port
 * `EMAIL_FROM`: SMTP from email address (normally the same as `EMAIL_USER`)
 * `EMAIL_TO`: Where to send email alerts
 * `EMAIL_TEST`: Send a test email and exit without taking a backup
 * `EMAIL_ENABLED`: Whether to email alerts after backups


The variable `BACKUP_VERBOSE` can be set to `True` to enable extra verbosity during backups.

Mount a Google Cloud service account private key, in json format, to `/google-service-account.json` and for the backup location, use format `gcs:<bucketname>/<path>`.