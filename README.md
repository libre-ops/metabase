Metabase provisioning role
==========================

This is an Ansible role for provisioning [Metabase](https://metabase.com), an open-source tool for Business Intelligence and analytics.

See the latest Metabase docs [here.](https://metabase.com/docs/latest) 


Requirements
------------

You'll need to install Java OpenJDK version 8 or higher. It's been intentionally left out of the requirements in `meta/main.yml`
so you can install it via whatever role you want, instead of enforcing a specific installation method.


Defaults
--------

Check out all the defaults [here.](defaults/main.yml)


Setup
-----

By default Metabase uses an embedded H2 database. For a production environment you should use your own. This role defaults to using Postgresql, but any can be used. 
If you want to skip that step and use the built-in embedded H2 database, just define: `use_own_database: false`

Note: this database is for storing Metabase's own application data and settings, not the database to be analyzed.

If you roll your own, the database will have to be created before running this role, and you can override the following variables:
```
metabase_db: metabase
metabase_db_type: postgres
metabase_db_host: localhost
metabase_db_port: 5432
metabase_db_user: metabase
metabase_db_pass: changeme
```

This role will also set up the initial admin user for Metabase. You can override these variables (or use the defaults for testing):
```
metabase_admin:
  first_name: Metabase
  last_name: Admin
  email: example@example.com
  password: metabase123
```

Configuring datasets
--------------------

Metabase comes with a sample dataset, but if you have your data ready you can optionally pass in a list of databases to add during installation, in this format:

```
metabase_databases:
  - name: Business Data
    engine: postgres
    dbname: analyze-me
    host: localhost
    port: 5432
    user: postgres
    password: changeme
    ssl: false
```

Note: these can be configured or changed later within the app, so it's not essential.


Example playbook
----------------

```
- name: Provision Metabase
  hosts: webservers

  roles:
    - role: libre_ops.metabase
```

You can also use this role to install Metabase on your local machine, for example:

```
- name: Install Metabase Locally
  hosts: 127.0.0.1
  connection: local

  roles:
    - role: libre_ops.metabase
      vars: 
        use_own_database: false
```
