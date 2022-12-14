## Files:

We have 4 important file:
1. crontab.conf
2. Dockerfile
3. script.sh

- crontab.conf:

this will run our scrirpt monthly
```
$cron_time /script.sh
```
- Dockerfile:
```
FROM mysql:8.0-debian
RUN apt-get update && apt-get install -y cron
ADD crontab.conf /crontab.conf
ADD script.sh /script.sh
COPY entry.sh /entry.sh
RUN chmod 755 /script.sh /entry.sh
RUN /usr/bin/crontab /crontab.conf
CMD ["/usr/sbin/cron -f -l 8"]
```
- script.sh:
```
#!/bin/bash
dt=$(date +'%Y%m%d_%H%M')
echo "Backup starts: $(date + "%Y-%m-%d %H:%M:%S")"


# execute the backup command:
mysqldump -h $mysql_host -P $mysql_port -u $mysql_user -p $mysql_password $mysql_db --all-databases > $backup_dir/mysql_backup_"$dt".sql

find $backup_dir -mtime +7 -type f -name '*.sql' -exec rm -rf {} \;
echo "Backup ends: $(date + "%Y-%m-%d %H:%M:%S")"
```

### docker-compose file:
it should be a file like this:
```
cron_mysql:
  image: cron_mysql
  container_name: cron_mysql
  restart: unless-stopped
  user: root
  tty: true
  environment:
    - mysql_password="your_mysql_password"
    - mysql_db="your_mysql_db"
    - mysql_user="your_mysql_user"
    - mysql_host="your_mysql_host"
    - mysql_port="your_mysql_port"
    - backup_dir="your_backup_path"
    - cron_time="your_back_up_time"
  volumes:
    - /etc/localtime:/etc/localtime:ro
    - /var/docker/mysql:/var/lib/mysql
    - /var/docker/backup/mysql:/opt/mysql/backup
  networks:
    - app-network
  depends_on:
    - mysql
```
