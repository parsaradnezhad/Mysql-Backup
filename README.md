## Mysql-Backup :

I am gonna use this image for back-up mysql database : https://hub.docker.com/r/imixs/backup

this is a back-up image with lots of features that you can read in image overview.

### How to use :

We can add this as a service to our docker-compose so we have always backup if we have mysql there.

```
backup:
     image: imixs/backup
     environment:
      SETUP_CRON: "SET THIS AS YOUR NEEDS"
      BACKUP_DB_TYPE: "MYSQL"
      BACKUP_DB_USER: “$DB_USER”
      BACKUP_DB_PASSWORD: “$DB_PASSWORD”
      BACKUP_DB_HOST: “$DB_HOST”
      BACKUP_LOCAL_ROLLING: “5”
```

this image has a lots of env variables which done different things but which we used above is enough for us.

All backups are located in the following local directory:
```
/root/backups/
```
In the backup space, the files are located at:
```
/$BACKUP_ROOT_DIR/$BACKUP_SERVICE_NAME/
```
Each backup file has a time stamp prefix indicating the backup time:
```
2018-01-07_03:00_dump.tar.gz
```

For more information and features check image overview.
