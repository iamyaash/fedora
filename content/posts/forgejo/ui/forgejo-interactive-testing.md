+++
title = 'Interactive Forgejo Testing'
date = 2025-04-25T21:42:44+05:30
draft = false
tags = ["docs", "forgejo"]
summary = "Summary of how to setup an interactive Forgejo testing environment to see if your changes work!"
+++

# [Interactive Testing](https://forgejo.org/docs/next/contributor/testing/#interactive-testing-during-development)
We can run a Forgejo instance locally for testing:
```sh
TAGS='sqlite sqlite_unlock_notify' make watch
```

This will initiate a Forgejo instance locally, but it won't give you the database to store any information. The database setup & initialization must be done manually. _In my case, it's my first time interacting with databases on my own, so I will just note down everything for future reference_.

## Install & Setup `mysql` Database
**Install `mysql-server`**:
```sh
sudo dnf install mysql-server
```

### Check if it's enabled & running:
```sh
sudo systemctl status mysqld
```
**If `mysqld` is inactive**:
```sh
sudo systemctl enable mysqld
```
```sh
sudo systemctl start mysqld
```

**Run the secure installation**:
```sh
sudo mysql_secure_installation
```

### Initialize `mysqld`
```sh
sudo mysql -u root -p
```
- `-u` username
- `-p` password

### Create the Database and user for Forgejo

1. Create a Database

```sh
CREATE DATABASE forgejo CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
```
* **Creates a new database** named `forgejo`.
* `CHARACTER SET utf8mb4`: Ensures full Unicode support, including emojis and multilingual characters.
* `COLLATE utf8mb4_general_ci`: Sets the default text sorting and comparison rules (case-insensitive).

2. Create a User

```sh
CREATE USER 'forgejouser'@'localhost' IDENTIFIED BY 'strongpass';
```

* **Creates a new database user** named `forgejouser`.
* `'localhost'` means the user can only connect from the local machine.
* `IDENTIFIED BY 'strongpass'`: Sets the password for the user.

3. Grant Privileges

```sh
GRANT ALL PRIVILEGES ON forgejo.* TO 'forgejouser'@'localhost';
```

* Gives the user **full access (read/write/create/delete)** to **all tables in the `forgejo` database**.
* Does **not** affect other databases — it's scoped to just `forgejo`.

4. Reload Privileges

```sh
FLUSH PRIVILEGES;
```

* **Forces the database to reload** the privileges table so the changes take immediate effect.

### Initial Database Configuration in Forgejo
Ensure that your database is properly configured using `MySQL` (_or any compatible local database you have set up_).

**Database Settings**

| **Setting**   | **Value**     |
| ------------- | ------------- |
| Database Type | `MySQL`       |
| Host          | `localhost`   |
| Username      | `forgejouser` |
| Password      | `strongpass`  |
| Database Name | `forgejo`     |

> **Note:** These values may vary depending on your specific database configuration.
