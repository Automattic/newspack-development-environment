# Newspack Development Environment

These are instructions used for setting up the for [Newspack](https://newspack.blog/) development environment.

## VVV setup

### VVV Install

The basic instructions for installing VVV are in their [documentation](https://varyingvagrantvagrants.org/docs/en-US/installation/software-requirements/). Complete the setup per their instructions before continuing below.

### Site Setup

The VVV comes bundled with the `wordpress-trunk` site which contains all the WP test dependencies used in this project. Newspack environment uses this site for its development.

If the `vvv-custom.yml` file does not exist, you can create it by copying `vvv-config.yml` to `vvv-custom.yml`.

Edit the `vvv-custom.yml` in the main VVV folder, then make sure the following site is available:
```
sites:

  wordpress-trunk:
    skip_provisioning: false
    description: "An svn based WP Core trunk dev setup, useful for contributor days, Trac tickets, patches"
    repo: https://github.com/Varying-Vagrant-Vagrants/custom-site-template-develop.git
    hosts:
      - trunk.wordpress.test
```

Be sure to indent correctly as whitespaces matter in YAML files, and VVV prefers to indent using 2 spaces.

Here the host `trunk.wordpress.test` host is used. In case this host name is already in use on your local machine, change it to a different one, and then proceed to the next step.

All other sites can be removed and commented out by adding the `#` character at the beginning of the lines.  

### The Setup Script

Next change directory to point inside your Vagrant instance folder (next to the `vvv-custom.yml` file) and run the following command:
```
bash <(curl -s https://raw.githubusercontent.com/Automattic/newspack-development-environment/master/setup_newspack.sh)
```

If a site different than `wordpress-trunk` was used, now is the time to update the `./config/homebin/vagrant_provision_custom` file, and change the `WP_PATH` path. 

### Provisioning

To finish up, run `vagrant up --provision`.
Or if your VVV is already up and running, run `vagrant reload --provision`.

## Using VVV

### DB Access

#### phpMyAdmin
To use the readily available phpMyAdmin which gets installed with VVV by default:
* http://vvv.test/database-admin/
* user: `root`
* pass: `root`

#### Local DB client

The following parameters' values are taken from default VVV installation. If you have modified those, or perchance run several VVV machines in parallel, you may need to update the following values.

If you're using a different DB client, you can either create a MariaDB/MySQL DB connection via an SSH tunnel, or use the "direct connection" functionality (on port 3306, created for new VVV versions) which and has a simpler setup.

A DB connection via an SSH tunnel:
* SSH host: `192.168.50.4`
* SSH user: `vagrant`
* SSH password: `vagrant` 
* DB host: `127.0.0.1`
* DB port: `3306`
* DB user: `root`
* DB pass: `root`

To use the direct connection, create a use the following credentials (based on the default VVV configuration values):
* host: `192.168.50.4`
* port: `3306`
* user: `external`
* pass: `external`

### VVV Dashboard and Docs

Additional tools are available in VVV at [http://vvv.test](http://vvv.test): phpMyAdmin, phpMemcachedAdmin, Opcache Status, MailHog, Webgrind, PHP Info, PHP Status.

### Main Workflow

To develop a new feature, plugin or theme, use a standard GitHub workflow:
* create a new feature branch
* push the branch to origin
* create a Pull Request
* pass the code review
* merge the feature branch to master
* delete the feature branch
