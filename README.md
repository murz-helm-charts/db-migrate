# db-migrate

A Helm chart to migrate data from one database to another, supports MySQL,
MariaDB, PostgreSQL.

For now it uses [pgloader](https://github.com/dimitri/pgloader) tool to migrate data from MySQL or other databases to PostgreSQL.

Also it can use [etlalchemy](https://github.com/seanharr11/etlalchemy) tool to migrate data from any database type to another, but this is not stable yet.

To make the migration, set the desired source and target database types in the
`values.yaml` file like this:

```yaml
env:
  SOURCE_DATABASE_TYPE: "mysql"
  SOURCE_DATABASE_NAME: "my_database"
  SOURCE_DATABASE_USER: "root"
  SOURCE_DATABASE_PASSWORD: "my_mysql_password"

  TARGET_DATABASE_TYPE: "postgresql"
  TARGET_DATABASE_NAME: "pg_default"
  TARGET_DATABASE_USER: "postgres"
  TARGET_DATABASE_PASSWORD: "my_pg_password"

source-mariadb:
  enabled: true
  image:
  tag: 10.5.13-debian-10-r32
  primary:
    persistence:
      existingClaim: "my-source-database-pvc"
target-postgresql:
  enabled: true
  primary:
    persistence:
      existingClaim: "my-target-database-pvc"
```

And then - deploy the chart:

```
helm install my-migration oci://registry-1.docker.io/murznn/db-migrate
```

It will create a one-time Kubernetes job that will try to make the migration.
See logs if it is failed, fix configuration and redeploy the chart to repeat
the job.
