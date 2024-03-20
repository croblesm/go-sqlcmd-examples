# Getting started SQLCMD
> This repository provides examples of how to use the `SQLCMD` command to manage SQL Server in a container.

## Prerequisites

- [Create a GitHub Codespaces environment (Optional)](https://github.com/features/codespaces)
- [Install Docker (Not required for GitHub Codespaces)](https://docs.docker.com/engine/install)
- [Install SQLCMD](https://learn.microsoft.com/en-us/sql/tools/sqlcmd/sqlcmd-utility?view=sql-server-ver16&tabs=go%2Clinux&pivots=cs1-bash#tabpanel_2_go)

Here you have the instructions to install the prerequisites in Ubuntu 20.04 with GitHub Codespaces:

## Installing SQLCMD for Ubuntu

```bash
curl https://packages.microsoft.com/keys/microsoft.asc | sudo tee /etc/apt/trusted.gpg.d/microsoft.asc
sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/20.04/prod.list)"
sudo apt-get update
sudo apt-get install sqlcmd
```

## Usage

The new version of `SQLCMD`, allows to create a new container, and restore a database to that container to create a new local copy of a database, for development or testing.

The command is quite simple, you need to use the `create` command, and specify the `mssql` provider. The `mssql` provider has several options, such as the `--tag` option to specify the version of SQL Server, the `--using` option to specify the database to restore, and the `--user-database` option to specify the name of the user database.

```bash
Usage:
  sqlcmd create mssql [flags]
  sqlcmd create mssql [command]
```

## Examples

Let's see a few examples:

- Start by retrieving all the available tags (container images) for SQL Server:

    ```bash
    sqlcmd create mssql get-tags
    ```
- Create a new container using the latest version (2022) of SQL Server, and default options:

    ```bash
    sqlcmd create mssql --accept-eula 
    ```

- Create a new container using the 2019 version of SQL Server, and default options:

    ```bash
    sqlcmd create mssql --tag 2019-latest --accept-eula
    ```

- Create SQL Server, download and attach AdventureWorks sample database:

    ```bash
    sqlcmd create mssql --using https://aka.ms/AdventureWorksLT.bak --accept-eula
    ```

- Create SQL Server, download and attach AdventureWorks sample database with different database name

    ```bash
    sqlcmd create mssql --using https://aka.ms/AdventureWorksLT.bak,adventureworks --accept-eula
    ```

- Create SQL Server with an empty user database

    ```bash
    sqlcmd create mssql --user-database db1 --accept-eula
    ```

- Install/Create SQL Server with full logging

    ```bash
    sqlcmd create mssql --verbosity 4 --accept-eula
    ```

## Connect to database

First you need to get the connection string, you can use the `config connection-strings` command to get the connection string for the container:

```bash
sqlcmd config connection-strings
```
Grab the line corresponding to SQLCMD (last line), and then connect to your database as follows:

```bash
export 'SQLCMDPASSWORD=<PasswordGoesHere>'; sqlcmd -S 127.0.0.1,1433 -U <Username> -d <Database>
```

## Flag

When creating the container, you can customize the container using the following flags:

```bash
Flags:
      --accept-eula                     Accept the SQL Server EULA
      --architecture string             Specifies the image CPU architecture (default "amd64")
      --cached                          Don't download image.  Use already downloaded image
      --collation string                The SQL Server collation (default "SQL_Latin1_General_CP1_CI_AS")
  -c, --context-name string             Context name (a default context name will be created if not provided)
      --errorlog-wait-line string       Line in errorlog to wait for before connecting (default "The default language")
  -h, --help                            help for mssql
      --hostname string                 Explicitly set the container hostname, it defaults to the container ID
      --name string                     Specify a custom name for the container rather than a randomly generated one
      --os string                       Specifies the image operating system (default "linux")
      --password-encryption string      Password encryption method (none) in sqlconfig file (default "none")
      --password-length int             Generated password length (default 50)
      --password-min-number int         Minimum number of numeric characters (default 10)
      --password-min-special int        Minimum number of special characters (default 10)
      --password-min-upper int          Minimum number of upper characters (default 10)
      --password-special-chars string   Special character set to include in password (default "!@#$%&*")
      --port int                        Port (next available port from 1433 upwards used by default)
      --registry string                 Container registry (default "mcr.microsoft.com")
      --repo string                     Container repository (default "mssql/server")
      --tag string                      Tag to use, use get-tags to see list of tags (default "latest")
  -u, --user-database string            Create a user database and set it as the default for login
      --using string                    Download (into container) and attach database (.bak) from URL

Global Flags:
  -?, --?                  help for backwards compatibility flags (-S, -U, -E etc.)
      --sqlconfig string   configuration file (default "/Users/carlos/.sqlcmd/sqlconfig")
      --verbosity int      log level, error=0, warn=1, info=2, debug=3, trace=4 (default 2)
      --version            print version of sqlcmd
```

For more information about the new version of `SQLCMD`, please visit the [Quickstart: Create a new local copy of a database in a container with sqlcmd](aka.ms/go-sqlcmd-qs).
