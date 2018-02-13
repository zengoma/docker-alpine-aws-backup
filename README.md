# docker-alpine-cron

Dockerfile for backing up directories to aws S3 bucket (based on xordiv's alpine cron)
Installed packages: dcron wget rsync ca-certificates aws-cli

** Please see the docker-compose file for a full working example of an s3 backup  

#### Environment variables:

AWS_ACCESS_KEY_ID=<your-aws-id>
AWS_SECRET_ACCESS_KEY=<your-aws-key>
AWS_DEFAULT_REGION=<eu-west-1>
AWS_DEFAULT_OUTPUT=<json>

CRON_STRINGS - strings with cron jobs. Use "\n" for newline (Default: undefined)   
CRON_TAIL - if defined cron log file will read to *stdout* by *tail* (Default: undefined)   
By default cron running in foreground

Optional: (These are only used in my docker-compose example)
AWS_BACKUP_BUCKET=<your-backet>
AWS_BACKUP_PATH=/backups

#### Cron files
- /etc/cron.d - place to mount custom crontab files  

When image will run, files in */etc/cron.d* will copied to */var/spool/cron/crontab*.   
If *CRON_STRINGS* defined script creates file */var/spool/cron/crontab/CRON_STRINGS*  

#### Log files
Log file by default placed in /var/log/cron/cron.log

#### Simple usage:
```
docker run --name="alpine-cron-sample" -d \
-v /path/to/app/conf/crontabs:/etc/cron.d \
-v /path/to/app/scripts:/scripts \
xordiv/docker-alpine-cron
```

#### With scripts and CRON_STRINGS
```
docker run --name="alpine-cron-sample" -d \
-e 'CRON_STRINGS=* * * * * root /scripts/myapp-script.sh'
-v /path/to/app/scripts:/scripts \
xordiv/docker-alpine-cron
```

#### Get URL by cron every minute
```
docker run --name="alpine-cron-sample" -d \
-e 'CRON_STRINGS=* * * * * root wget https://sample.dockerhost/cron-jobs'
xordiv/docker-alpine-cron
```
