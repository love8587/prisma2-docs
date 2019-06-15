# PostgreSQL data source connector

The PostgreSQL data source connector connects Prisma to a PostgreSQL database server.

## Example

To connect to a PostgreSQL database server, you need to configure a [`datasource`]() block in your [project file]():

```groovy
datasource pg {
  provider = "postgres"
  url      = env(POSTGRES_URL)
}

// ... the file should also contain a data model definition and (optionally) generators
```

The fields passed to the `datasource` block are:

- `provider`: Specifies the `postgres` data source connector.
- `url`: Specifies the [connection string](#connection-string) for the PostgreSQL database server. In this case, we're [using an environment variable]() to provide the connection string.

Find more information on the `datasource` fields [here]().

## Connection details

### Connection string

The connection string needs to follow the [official format](https://www.postgresql.org/docs/10/libpq-connect.html#id-1.7.3.8.3.6) for PostgreSQL connection strings:

```
postgresql://[user[:password]@][netloc][:port][,...][/dbname][?param1=value1&...]
```

A few examples (from the [official docs](ttps://www.postgresql.org/docs/10/libpq-connect.html)) are:

```
postgresql://
postgresql://localhost
postgresql://localhost:5433
postgresql://localhost/mydb
postgresql://user@localhost
postgresql://user:secret@localhost
postgresql://other@localhost/otherdb?connect_timeout=10&application_name=myapp
postgresql://host1:123,host2:456/somedb?target_session_attrs=any&application_name=myapp
```

### Configuration options

- **Host**: The IP address/domain of your database server, e.g. `localhost`.
- **Post**: The port on which your database server listens, e.g. `5432`.
- **Database**: The name of the database target schema. 
- **Schema**: The name target schema. 
- **User**: The database user, e.g. `admin`.
- **Password**: The password for the database user.
- **SSL**: Whether or not your database server uses SSL.