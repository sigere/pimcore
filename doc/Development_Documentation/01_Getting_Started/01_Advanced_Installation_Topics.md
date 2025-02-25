# Advanced Installation Topics

To fully automate the installation process, options can be passed in the CLI as parameters, rather than adding them interactively. 

The `--no-interaction` flag will prevent any interactive prompts:

```
./vendor/bin/pimcore-install --admin-username admin --admin-password admin \
  --mysql-username username --mysql-password password --mysql-database pimcore \
  --no-interaction
```

To avoid having to pass sensitive data (e.g. DB password) as command line option, you can also set each parameter as env
variable. See `./vendor/bin/pimcore-install` for details. Example:

```
$ PIMCORE_INSTALL_MYSQL_USERNAME=username PIMCORE_INSTALL_MYSQL_PASSWORD=password ./vendor/bin/pimcore-install \
  --admin-username admin --admin-password admin \
  --mysql-database pimcore \
  --no-interaction
```

### Preconfiguring the installer

You can preconfigure the values used by the installer by adding a config file which sets values for the database
credentials. This is especially useful when installing Pimcore on platforms where credentials are available via env vars
instead of having direct access to them. To preconfigure the installer, add a config file in `config/installer.yaml` 
(note: the file can be of any format supported by Symfony's config, so you could also use xml or php as the format), then configure the `pimcore_installer` tree:

```yaml
# config/installer.yaml

pimcore_install:
    parameters:
        database_credentials:
            user:                 username
            password:             password
            dbname:               pimcore
            
            # env variables can be directly read with the %env() syntax
            # see https://symfony.com/blog/new-in-symfony-3-2-runtime-environment-variables
            host:                 "%env(DB_HOST)%"
            port:                 "%env(DB_PORT)%"
```

## Set a timezone
Make sure to set the corresponding timezone in your configuration. 
It will be used for displaying date/time values in the admin backend.

```yaml
pimcore:
    general:
        timezone: Europe/Berlin
```
