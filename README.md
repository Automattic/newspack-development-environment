# Newspack Development Environment

What follows are steps to guide you through setting up a local instance for [Newspack](https://newspack.blog/) testing.

Most of the steps are one-time setup that you may or may not need to do depending on your existing configuration and what you have installed.

All of this assumes you are working on a Mac or a Linux. Before starting, be sure you have a github.com account and general experience cloning repositories. All code snippets should be run in the Terminal application.


# Install the Prerequisites

**Note:** this environment was thoroughly tested with the following versions of software:
- VirtualBox: 6.0.4 (recommended version), 5.2.18 (minimum version required)
- Vagrant: 2.2.5 (recommended version), 2.2.4 (minimum version required)

In case your versions of software don't match these, and your setup attempt does't complete successfully, please consider up- or downgrading to these recommended versions.


## Install VirtualBox
   
To determine if you have VirtualBox and which version, run:

```
virtualbox --version
```

This will open the VirtualBox application if you have it already. If not, install from https://www.virtualbox.org/


## Install Vagrant
   
To determine if you have Vagrant already, run:

```
vagrant --version 
```

If you don’t, install from https://www.vagrantup.com/downloads.html. (Download the file appropriate to your system and follow the instructions after).


## Reboot

As recommended by the [VVV documentation](https://varyingvagrantvagrants.org/docs/en-US/installation/software-requirements/):

> _"If you don’t reboot your machine after installing/updating Vagrant and VirtualBox, there can be networking issues. A full power cycle will ensure all components are fully installed and loaded"_.

reboot your computer now, and then come back to continue.


# Setup the Newspack Dev Environment

## Setup VVV

We use a Vagrant configuration called VVV for our local instances.

If you already have a directory where you like to keep repositories and other code feel free to use it, but for our purposes here we’ll create a directory called `repositories` to contain everything.

Run the following steps (instructions taken from [VVV docs](https://varyingvagrantvagrants.org/docs/en-US/installation/software-requirements/)):

```
mkdir ~/repositories

git clone -b master git://github.com/Varying-Vagrant-Vagrants/VVV.git ~/repositories/VVV_Newspack

cd ~/repositories/VVV_Newspack

vagrant plugin install vagrant-hostsupdater --local

```


## Provision VVV

Run **these exact commands**:

```
cd  ~/repositories/VVV_Newspack

bash <(curl -s https://raw.githubusercontent.com/Automattic/newspack-development-environment/master/setup_newspack.sh)

vagrant up

vagrant provision
```

* note: running shorter versions of the commands instead of exactly these ones (such as _'vagrant up --provision'_) has sometimes shown not to execute the custom installation scripts properly

This process should have downloaded and installed all the necessary Newspack project files, inside the VVV container. In case it has failed for some unpredicted reason, consult the [installation script](https://github.com/Automattic/newspack-development-environment/blob/master/config/homebin/vagrant_provision_custom) for required steps.


## Run Newspack

In a browser go to http://newspack-dev.test/wp-admin and login with username `admin` and password `password`.

