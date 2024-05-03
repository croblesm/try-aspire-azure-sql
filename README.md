# Dev Container - .NET with Aspire and Azure SQL

[![Open in Remote - Containers](https://img.shields.io/static/v1?label=Remote%20-%20Containers&message=Open&color=blue&logo=visualstudiocode)](https://vscode.dev/redirect?url=vscode://ms-vscode-remote.remote-containers/cloneInVolume?url=https://github.com/microsoft/vscode-remote-try-dab)

This repository offers a Dev Container template, enabling you to effortlessly explore **.NET Aspire** and **Azure SQL**. In just a few simple steps, you can dive into the capabilities of these powerful technologies.

A **development container** (Dev Container for short) is a running [Docker](https://www.docker.com) container with a well-defined tool/runtime stack and its prerequisites.You can try out Dev Containers with **[GitHub Codespaces](https://github.com/features/codespaces)** or **[Visual Studio Code Dev Containers](https://aka.ms/vscode-remote/containers)**.

Check out our diverse range of Dev Container templates on the [containers.dev/templates](https://containers.dev/templates) website. Additionally, you can also experiment with other sample projects available under [vscode-remote-try-*](https://github.com/search?q=org%3Amicrosoft+vscode-remote-try-&type=Repositories).

> **Note:** If you already have a Codespace or Dev Container, you can jump to the [About this template](#about-this-template) section.

## Setting up the development container

### GitHub Codespaces

Follow these steps to open this sample in a Codespaces:

1. Click the Code drop-down menu and select the **Codespaces** tab.
2. Click on **Create codespaces on main** at the bottom of the pane.

For more info, check out the [GitHub documentation](https://docs.github.com/en/free-pro-team@latest/github/developing-online-with-codespaces/creating-a-codespace#creating-a-codespace).

### Visual Studio Code - Dev Containers

If you already have VS Code and Docker installed, you can click the badge above or [here](https://vscode.dev/redirect?url=vscode://ms-vscode-remote.remote-containers/cloneInVolume?url=https://github.com/microsoft/vscode-remote-try-sqlserver) to get started. Clicking these links will cause VS Code to automatically install the Dev Containers extension if needed, clone the source code into a container volume, and spin up a dev container for use.

Follow these steps to open this sample in a container using the VS Code Dev Containers extension:

1. If this is your first time using a development container, please ensure your system meets the pre-reqs (i.e. have Docker installed) in the [getting started steps](https://aka.ms/vscode-remote/containers/getting-started).
2. To use this repository, you can either open the repository in an isolated Docker volume:
    - Press <kbd>F1</kbd> and select the **Dev Containers: Try a Sample...** command.
    - Choose the ".NET Core" sample, wait for the container to start, and try things out!
        > **Note:** Under the hood, this will use the **Dev Containers: Clone Repository in Container Volume...** command to clone the source code in a Docker volume instead of the local filesystem. [Volumes](https://docs.docker.com/storage/volumes/) are the preferred mechanism for persisting container data.
   Or open a locally cloned copy of the code:
    - Clone this repository to your local filesystem.
    - Press <kbd>F1</kbd> and select the **Dev Containers: Open Folder in Container...** command.
    - Select the cloned copy of this folder, wait for the container to start, and try things out!

### About this template

This Dev Container includes a preconfigured database, specifically tailored for use with the Aspire component. This database serves as a foundation for demonstrating how the Aspire component interacts with Azure SQL.

This template creates two containers, one for the Dev Container that includes .NET and Aspire, and one for Microsoft SQL Server. You will be connected to the Ubuntu, and from within that container, the MS SQL container will be available on **`localhost`** port 1433. The Dev Container also includes supporting scripts in the `.devcontainer/sql` folder used to configure the `Library` sample database.

The SQL container is deployed from the latest developer edition of Microsoft SQL 2022. The database(s) are made available directly in the Codespace/VS Code through the MSSQL extension with a connection labeled "LocalDev". The default `sa` user password is set using the .devcontainer/.env file. The default SQL port is mapped to port `1433` in `.devcontainer/docker-compose.yml`.

.NET Aspire is a cutting-edge framework crafted for developing cloud-native applications, with a primary focus on containerized environments. Engineered with .NET Core, Aspire empowers developers to architect scalable and resilient applications optimized for cloud deployment.

By harnessing the power of Aspire, developers can prototype and refine their applications locally using this Dev Container environment. Subsequently, deploying these applications to Azure becomes a streamlined process, facilitated by the native integration between .NET Aspire and Azure SQL Database.

**Note:**
While the SQL Server container employs a standard version of SQL Server, all database development within this Dev Container can be validated for Azure SQL Database using the SQL Database Project. The SQL Database project is preconfigured with the target platform set as Azure SQL Database.

#### Visual Code Tasks

We have added several tasks to this repository to help with common actions. You can access these tasks by opening the Command Palette in VS Code. Here's how:

1. Press <kbd>F1</kbd> or <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> to open the Command Palette.
2. Type "Run Task" and select "Tasks: Run Task".
3. Choose the task you want to run from the list.

##### Execute SQL Query

This task opens the `verifyDatabase.sql` file in your workspace and executes the SQL query in it. It uses the `ms-mssql.mssql` extension to execute the query. This task is part of the build group and is the default task that runs when you run the build task group.

##### Build SQL Database project

This task builds the SQL Database project. It runs the command `dotnet build` in the `database/Library` directory of your workspace.

This task is optional, but it is useful to verify the database schema. You can use this SQL Database project to make changes to the database schema and deploy it to the SQL Server container.

##### Deploy SQL Database Project

This task involves deploying the SQL Database project to your SQL Server container. It executes the `postCreateCommand.sh` script found in the `.devcontainer/sql` directory of your workspace.

The `postCreateCommand.sh` script requires one argument: the path to the directory containing the .dacpac file for the SQL Database project. In this scenario, that directory is `database/Library/bin/Debug`.

It utilizes the sqlpackage command-line utility to update the database schema using the .dacpac file, employing authentication credentials from the `.env` file situated in the `.devcontainer` directory.

##### Trust HTTPS certificate for .NET Aspire

This task trusts the HTTPS certificate for the .NET Aspire project. It runs the command `dotnet dev-certs https --trust`.

##### Update .NET SDK

This task updates the .NET SDK. This update ensures that .NET Aspire can leverage the latest advancements and enhancements provided by the .NET ecosystem, enabling smoother development experiences and potentially addressing any compatibility issues with newer versions of .NET or its related components.

#### Changing the sa password

To adjust the sa password, you need to modify the `.env` file located within the `.devcontainer` directory. This password is crucial for the creation of the SQL Server container and the deployment of the Library database using the `database/Library/bin/Debug/Library.dacpac` file.

The password must comply with the following rules:

- It should have a minimum length of 8 characters.
- It should include characters from at least three of the following categories: uppercase letters, lowercase letters, numbers, and non-alphanumeric symbols.

#### Database deployment

By default, a demo database is created titled "Library". To add additional database objects or data through T-SQL during Codespace configuration, edit the file `.devcontainer/sql/library.azure-sql.sql` or place additional `.sql` files in the `.devcontainer/mssql/` folder. *Large numbers of scripts may take a few minutes following container creation to complete, even when the SQL server is available the database(s) may not be available yet.*

You can also use the SQL Database Projects extension to deploy the database schema. The `Library.sqlproj` project is located in the `database/Library` folder and can be built using the `Build SQL Database project` task. The `verifyDatabase.sql` file in the `database/Library` folder can be used to verify the database schema.

Alternatively, .dacpac files placed in the `./bin/Debug` folder will be published as databases in the container during Dev Container configuration. [SqlPackage](https://docs.microsoft.com/sql/tools/sqlpackage) is used to deploy a database schema from a data-tier application.

### Adding another service

You can add other services to your `.devcontainer/docker-compose.yml` file [as described in Docker's documentation](https://docs.docker.com/compose/compose-file/#service-configuration-reference). However, if you want anything running in this service to be available in the container on localhost, or want to forward the service locally, be sure to add this line to the service config:

```yaml
# Runs the service on the same network as the database container, allows "forwardPorts" in devcontainer.json function.
network_mode: service:db
```

### Using the forwardPorts property

By default, web frameworks and tools often only listen to localhost inside the container. As a result, we recommend using the `forwardPorts` property to make these ports available locally.

This project uses the `5000` and `5001` ports for DAB, and the port `1433` for SQL Server:

```json
"forwardPorts": [5000, 5001, 8000, 1433]
```
> **Note:** You can add additional ports to this list as needed.

The `ports` property in `docker-compose.yml` [publishes](https://docs.docker.com/config/containers/container-networking/#published-ports) rather than forwards the port. This will not work in a cloud environment like Codespaces and applications need to listen to `*` or `0.0.0.0` for the application to be accessible externally. Fortunately the `forwardPorts` property does not have this limitation.