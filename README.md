# Newspack Development Environment

What follows are steps to guide you through setting up a local instance for [Newspack](https://newspack.blog/) testing.

Most of the steps are one-time setup that you may or may not need to do depending on your existing configuration and what you have installed.

All of this assumes you are working on a Mac or a Linux. All code snippets should be run in the Terminal application.


# Install the Prerequisites

## Install Virtual Box
   
To determine if you have Virtual Box, run:

```
virtualbox
```

This will open the Virtual Box application if you have it already. If not, install from https://www.virtualbox.org/


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

* note: running shorter versions of the commands instead of the exact ones as above (such as _'vagrant up --provision'_) has sometimes shown not to execute the custom installation scripts properly

This process should have downloaded and installed all the necessary Newspack project files, inside the VVV container. In case it has failed for some unpredicted reason, consult the [installation script](https://github.com/Automattic/newspack-development-environment/blob/master/config/homebin/vagrant_provision_custom) for required steps.


## Run Newspack

In a brower go to http://newspack-dev.test/wp-admin and login with username `admin` and password `password`.



## Build Newspack

### Install Build Prerequisites

The build process also requires some preinstalled applications. These are instructions for installing them on OS X.

#### Install Homebrew

You may need Homebrew to install various other software, so it’s recommended to get this sorted out early. To see if you have it already, run:

```
brew --version
```

If not, per instructions on https://brew.sh/ run:

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install) 
```

#### Install Node Version Manager

In the development phase, Newspack repositories use Node.js to build. If your installed version of Node.js doesn’t match ours this can lead to a variety of problems, so we recommend using Node Version Manager (NVM) to easily control your version of Node. Start by checking if you have it already:

```
nvm --version
```

If not, follow the instructions in [this tutorial](https://medium.com/@isaacjoe/best-way-to-install-and-use-nvm-on-mac-e3a3f6bc494d). The steps to edit your bash profile can get a little tricky.


### Build Blocks and Plugin

We recommend using Node.js version 10.10.0 for now. 

```
node --version
```

If it isn’t v10.10.0:

```
nvm install 10.10.0

nvm use 10.10.0

node --version
```

Now it should be v10.10.0. 

```
cd  ~/repositories/VVV_Newspack/www/wordpress-trunk/public_html/src/wp-content/plugins/newspack-plugin	

npm ci && npm run clean && npm run build:webpack

cd  ~/repositories/VVV_Newspack/www/wordpress-trunk/public_html/src/wp-content/plugins/newspack-blocks

npm ci && npm run clean && npm run build:webpack
```

