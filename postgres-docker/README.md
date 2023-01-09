

## Now run the below command to run postgreSQL and pgAdmin4 in a detached mode
```
docker compose up -d
To view the logs , use command
 docker logs -f local_pgdb
```

To configure pgadmin – open a browser and go to – http://localhost:5050/ . In the connection details for hostname give the container name of postgreSQL