
# Data API builder and MS SQL (dab-mssql)

Try out Data API builder with SQL Server. Includes .NET 6, DAB CLI, extensions, dependencies and an a SQL container initialized with the Library database that can be used to try out Data API builder.

## Options

| Options Id | Description | Type | Default Value |
|-----|-----|-----|-----|
| imageVariant | .NET version: | string | 6.0-focal |

This template references an image that was [pre-built](https://containers.dev/implementors/reference/#prebuilding) to automatically include needed devcontainer.json metadata.

* **Image**: mcr.microsoft.com/devcontainers/dotnet ([source](https://github.com/devcontainers/images/tree/main/src/dotnet))
* **Applies devcontainer.json contents from image**: Yes ([source](https://github.com/devcontainers/images/blob/main/src/dotnet/.devcontainer/devcontainer.json))

## Using this template

This template creates two containers, one for Data API builder (.NET) and one for Microsoft SQL Server. You will be connected to the Ubuntu or Debian container, and from within that container the MS SQL container will be available on **`localhost`** port 1433. The Data API builder container also includes supporting scripts in the `.devcontainer/mssql` folder used to configure the `Library` database. 

The SQL container is deployed from the latest developer edition of Microsoft SQL 2022. The database(s) are made available directly in the Codespace/VS Code through the MSSQL extension with a connection labeled "LocalDev".  The default `sa` user password is set to `P@ssw0rd`. The default SQL port is mapped to port `1433` in `.devcontainer/docker-compose.yml`.

#### Changing the sa password

To change the `sa` user password, change the value in `.devcontainer/docker-compose.yml` and `.devcontainer/devcontainer.json`.

#### Database deployment

By default, a  demo database is created titled "Library".  To add additional database objects or data through T-SQL during Codespace configuration, edit the file `.devcontainer/mssql/library.azure-sql.sql` or place additional `.sql` files in the `.devcontainer/mssql/` folder. *Large numbers of scripts may take a few minutes following container creation to complete, even when the SQL server is available the database(s) may not be available yet.*

Alternatively, .dacpac files placed in the `./bin/Debug` folder will be published as databases in the container during Codespace configuration. [SqlPackage](https://docs.microsoft.com/sql/tools/sqlpackage) is used to deploy a database schema from a data-tier application file (dacpac), allowing you to bring your application's database structures into the dev container easily. *The publish process may take a few minutes following container creation to complete, even when the server is available the database(s) may not be available yet.*

### Adding another service

You can add other services to your `.devcontainer/docker-compose.yml` file [as described in Docker's documentation](https://docs.docker.com/compose/compose-file/#service-configuration-reference). However, if you want anything running in this service to be available in the container on localhost, or want to forward the service locally, be sure to add this line to the service config:

```yaml
# Runs the service on the same network as the database container, allows "forwardPorts" in devcontainer.json function.
network_mode: service:db
```

### Using the forwardPorts property

By default, web frameworks and tools often only listen to localhost inside the container. As a result, we recommend using the `forwardPorts` property to make these ports available locally.

```json
"forwardPorts": [9000]
```

The `ports` property in `docker-compose.yml` [publishes](https://docs.docker.com/config/containers/container-networking/#published-ports) rather than forwards the port. This will not work in a cloud environment like Codespaces and applications need to listen to `*` or `0.0.0.0` for the application to be accessible externally. Fortunately the `forwardPorts` property does not have this limitation.

---

_Note: This file was auto-generated from the [devcontainer-template.json](https://github.com/devcontainers/templates/blob/main/src/dotnet-mssql/devcontainer-template.json).  Add additional notes to a `NOTES.md`._