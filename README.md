## Elasticsearch Logstash Index Management

This is a collection of bash scripts for managing [Elasticsearch](http://www.elasticsearch.org) indices. They are specifically designed around the daily index pattern used in [Logstash](http://logstash.net).

Support is integrated for uploading and restoring index backups with [boto_rsync](https://github.com/seedifferently/boto_rsync).

### elasticsearch-remove-old-indices.sh

This script generically walks through the indices, sorts them lexicographically, and deletes anything older than the configured number of indices.

### elasticsearch-backup-index.sh

Backup handles making a backup and a restore script for a given index. The default is yesterday's index, or you can pass a specific date for backup. You can optionally keep the backup locally, and/or push it to s3. If you want to get tricky, you can override the s3cmd command, and you could have this script push the backups to a storage server on your network (or whatever you want, really).

### elasticsearch-restore-index.sh

Restore handles retrieving a backup file and restore script (from S3), and then executing the restore script locally after download.

## Cron

Something like this might be helpful, assuming you placed the scripts in the /opt/es/ directory (formatted for an /etc/cron.d/ file):

    00 7 * * * root /bin/bash /opt/es/elasticsearch-backup-index.sh -b "s3://es-bucket" -i "/opt/logstash/server/data/elasticsearch/nodes/0/indices"
    00 9 * * * root /bin/bash /opt/es/elasticsearch-remove-old-indices.sh -i 21



