## Mysql-Backup :

Backup mysql database from container :
```
docker exec "$CONTAINER-NAME" /usr/bin/mysqldump -u root --password="$PASSWORD" --all-databases > "$CONTAINER-NAME-DATE"
```
>You can zip your backup file :
```
bzip2 -k "$CONTAINER-NAME-DATE"
```
Restore Backup :
>You can unzip your backup file :
```
bzip2 -d "$CONTAINER-NAME-DATE".bz2
```
```
cat "$CONTAINER-NAME-DATE" | "$CONTAINER-NAME" docker exec -i  /usr/bin/mysql -u root --password="$PASSWORD" --all-databases
```

### Conclusion :

In our case , because we only need back-up from mysql databases and there is not anything else which has value for us , the best case scenario is the commands that i have said above , which back-up all databases of mysql in any container we want and compress it ; this will let us to restore our backup easily at anytime we want.
