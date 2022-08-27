## Mysql-Backup :

Backup mysql database from container :
```
docker exec "CONTAINER-NAME" /usr/bin/mysqldump -u root --password=root "DATABASE-NAME" > "CONTAINER-NAME-DATE"
```
>You can zip your backup file :
```
bzip2 -k "CONTAINER-NAME-DATE"
```
Restore Backup :
>You can unzip your backup file :
```
bzip2 -d "CONTAINER-NAME-DATE.bz2"
```
```
cat "CONTAINER-NAME-DATE" | "CONTAINER-NAME" docker exec -i  /usr/bin/mysql -u root --password=root "DATABASE-NAME"
```
