# MySQL data source connector

The MySQL data source connector connects Prisma to a MySQL database server.

## Example

To connect to a MySQL database server, you need to configure a [`datasource`]() block in your [project file]():

```groovy
datasource pg {
  provider = "postgres"
  url      = env(POSTGRES_URL)
}

// ... the file should also contain a data model definition and (optionally) generators
```

The fields passed to the `datasource` block are:

- `provider`: Specifies the `postgres` data source connector.
- `url`: Specifies the [connection string](#connection-string) for the MySQL database server. In this case, we're [using an environment variable]() to provide the connection string.

Find more information on the `datasource` fields [here]().

## Connection details

### Configuration options

- **Host**: The IP address/domain of your database server, e.g. `localhost`.
- **Post**: The port on which your database server listens, e.g. `5432`.
- **Database**: The name of the database. 
- **User**: The database user, e.g. `admin`.
- **Password**: The password for the database user.
- **SSL**: Whether or not your database server uses SSL.