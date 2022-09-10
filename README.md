## Files:

We have 4 important file:
1. crontab.conf
2. Dockerfile
3. entry.sh
4. script.sh

- crontab.conf:
```
0 0 1 * * /script.sh
```
- Dockerfile:
```
FROM mysql:5.7.26
RUN apt-get update && apt-get install -y cron
ADD crontab.conf /crontab.conf
ADD script.sh /script.sh
COPY entry.sh /entry.sh
RUN chmod 755 /script.sh /entry.sh
RUN /usr/bin/crontab /crontab.conf
CMD [/entry.sh]
```
- entry.sh:
```
#!/bin/sh
/usr/sbin/cron -f -l 8
```
- script.sh:
```
#!/bin/bash
mysql_password=your_mysql_password
mysql_db=your_mysql_db
mysql_user=your_mysql_user
mysql_host=your_mysql_host
mysql_port=your_mysql_port
backup_dir=your_backup_path

dt=$(date +'%Y%m%d_%H%M')
echo "Backup starts: $(date + "%Y-%m-%d %H:%M:%S")"


# execute the backup command:
mysqldump -h $mysql_host -P $mysql_port -u $mysql_user -p $mysql_password $mysql_db --single-transaction --quick --lock-tables=false > $backup_dir/mysql_backup_$dt.sql

find $backup_dir -mtime +7 -type f -name '*.sql' -exec rm -rf {} \;
echo "Backup ends: $(date + "%Y-%m-%d %H:%M:%S")"
```
