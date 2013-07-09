## Logstash ElasticSearch Backup Management

This collection of scripts can be useful in managing your [Logstash](http://logstash.net) [Elasticsearch](http://www.elasticsearch.org) indexes for backup, cleanup, and hot restore.

### Dependencies

Index uploading and restoring for backups is done through Amazon S3 with [boto_rsync](https://github.com/seedifferently/boto_rsync).

## Scripts

### logstash-elasticsearch-cleanup.sh

This script generically walks through the indices, sorts them lexicographically, and deletes anything older than the configured number of indices.

### logstash-elasticsearch-backup.sh

Backup handles making a backup and a restore script for a given index. The default is yesterday's index, or you can pass a specific date for backup. You can optionally keep the backup locally, and/or push it to s3. If you want to get tricky, you can override the s3cmd command, and you could have this script push the backups to a storage server on your network (or whatever you want, really).

### logstash-elasticsearch-restore.sh

Restore handles retrieving a backup file and restore script (from S3), and then executing the restore script locally after download.

## Cron

Here's an example cron script

    00 7 * * * root /bin/bash /opt/logstash-elasticsearch-backup/logstash-elasticsearch-backup.sh -b "s3://bucket" -i "/opt/logstash/server/data/elasticsearch/nodes/0/indices"
    00 9 * * * root /bin/bash /opt/logstash-elasticsearch-backup/logstash-elasticsearch-cleanup.sh -i 21



