<img align="right" src="html/images/PSICK-logo-200x200.png" />

## PSICK
## Puppet Systems Infrastructure Construction Kit

[![Build Status](https://travis-ci.org/example42/psick.png?branch=production)](https://travis-ci.org/example42/psick)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/503831d4ea6a470e864f1a3969449b78)](https://www.codacy.com/app/example42/psick?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=example42/psick&amp;utm_campaign=Badge_Grade)


A **Puppet control-repo** [generator] on steroids, featuring:

  - A modern, opinionated, general purpose, full featured, reusable control-repo
  - Multiple ways to test local Puppet code (on Docker, Vagrant or directly remote hosts)
  - Gitlab CI pipeline to control Puppet code deployment
  - Usable in any Puppet setup, based on Puppet OSS, PE, Foreman...
  - Ready to use profiles for system baselines and common applications
  - Toolset to create and maintain a new control-repo based on PSICK (WIP)

PSICK is a Puppet control-repo itself, you can use this repository directly in a Puppet environment,
and basically have a full PSICK setup, or run the ```psick``` command to generate a new Puppet
control-repo based on the components you need.

Components can be:

  - Profiles (and relevant tools and hiera data) for different applications
  - Integrations with Vagrant, Docket, GitLab, Fabric...
  - Scripts, tools or additional control-repo files

### Setup of a new control-repo

Download this repository:

    git clone https://github.com/example42/psick
    cd psick
    ./psick create

The psick command currently it just allows you to create a new control-repo and populate it either with a bare
minimal skeleton, or with the full PSICK contents.
In the future it will provide the possibility to pick single components (integrations, profiles...),
see how they diff compared to your own control-repo and eventually update them on your local contro-repo.

Once you have created your control-repo you can start to work with it.
If you have chosen to copy the full PSICK contents in your control repo, you can run the following commands
from your own control-repo directory, otherwise run them from the PSICK directory.
This applies to all the scripts and paths referenced in the docs, just be aware that some of the scripts in bin/
and other integrations might not work correctly in a not full PSICK setup.


### Setup of a Puppet environment

This control-repo requires Puppet 4, if it's not already installed, you can install it with this cross OS Puppet 4 install script (it uses the official Puppet repos):

    sudo bin/puppet_install.sh # Only if you don't have Puppet 4 installed

Before starting to use it, you have to populate the ```modules/``` directory of the control-repo.

You need to do this both on your **development** workstation, and on your **Puppet server** (after having placed your control-repo into the ```/etc/puppetlabs/code/environments/``` directory).

To install the prequequisite gems (hiera-eyaml, deep_merge, r10k) and populate the external modules directory via r10k, you can run: 

    bin/puppet_setup.sh        # Only if you don't have the prerequisites gems

If you have already r10k and the prerequisite gems, just run:

    r10k puppetfile install -v

If you also want to install the recommended (Fabric, Vagrant, Docker) tools that can be used with the repo, run:

    bin/setup.sh               # Only if you want to install Fabric, Vagrant and Docker

The script, installs and runs r10k and then uses Puppet to install the other software.

Notes:

  - You will be always asked to confirm or skip each step.

  - The script will use ```sudo``` for the operations that need root privileges.

  - Scripts are mostly tested on Mac and Linux environments. On Mac some packages installations don't work.

  - You can safely interrupt the scripts with CTRL+C at any time

  - For unattended setups (typically in CI pipelines) you can skip confirmation requests passing the argument auto:

        bin/puppet_setup.sh auto
        bin/setup.sh auto


### Directory structure

PSICK has the common set of files and directories of a Puppet control-repo:

  - ```environment.conf``` - The Puppet environment configuration file

  - ```manifests/``` - Directory with the main manifests. Here we have just ```site.pp```

  - ```Puppetfile``` - File that defines the external modules to add via r10k

  - ```modules/``` - Directory where modules defined in Puppetfile are placed (it's .gitignored)

  - ```hiera.yaml``` - Hiera 5 environment configuration file. An equivalent Hiera 3 file is ```hiera3.yaml``` (was linked to ```/etc/puppetlabs/puppet/hiera.yaml```)   

  - ```hieradata/``` - Directory where Hiera data is stored, in Yaml files.

Some extra directories are added in PSICK for integrations and tools:

  - ```bin/``` - Directory containing tools and scripts for various Puppet related operations

  - ```docs/``` - Directory with extra docs

  - ```site/``` - An additional modules directory, with local profiles and tools.

  - ```docker/``` - Files used for building Docker images for multiple OS

  - ```vagrant/``` - Various Vagrant environments where is possible to test local Puppet code

  - ```fabfile/``` - Fabric integration and scripts

  - ```skeleton/``` - A skeleton of a Puppet 4 module, to use with ```puppet module generate```

  - ```.gitlab-ci.yaml``` - (Sample) GitLab Continuous Integration pipeline for code testing and deployment


### Compatibility

PSICK uses cutting edge Puppet technology and all its components are expected to work on these versions:

  - Puppet OSS 4.9 or later.
  - Puppet Enterprise 2017.1.0 or later

In particular the ```profile``` class uses data in modules and requires a relatively modern Puppet.

For most of the other parts of the control repo you can use, at least:

  - Puppet OSS >= 4.4 < 4.9
  - Puppet Enterprise >= 2016.1.1 < 2017.1.0

In order to fix the ```profile``` class for compatibility with these versions, add default values to the profiles params as curretly defined in the ```data``` directory.


### Documentation

PSICK is full of more or less hidden stuff, which ease a lot Puppet code development, testing and deployment.  Here is where you can find more info:

General Puppet documentation:

  - [Introduction to Puppet](docs/puppet.md) - A very basic introduction to Puppet

  - [Hiera essentials](docs/hiera.md) - Basic Hiera concepts

  - [Hiera eyaml](docs/hiera_eyaml.md) - An overview on how to use hiera-eyaml

  - [Trusted Facts](docs/trusted_facts.md) - How to set and use trusted facts

  - [External Facts](docs/external_facts.md) - How to set and use external facts

  - [Puppet Enterprise Console](docs/pe_console.md) - An overview on the Puppet Enterprise console


About this control-repo:

  - [Control-repo structure](docs/structure.md) - A description of the control-repo structure and most important paths

  - [Control-repo logic](docs/use.md) - An overview of the design choices and the logic of this control repo.

  - [Prerequisites](docs/prerequisites.md) - A more detailed view of the prerequisites needed to fully use the control-repo

  - [Noop Mode](docs/noop_mode.md) - An overview on how to enforce noop mode server side with this repo

  - [Vagrant Integration](docs/vagrant.md) - How to use Vagrant to test the control-repo during development

  - [Docker Integration](docs/docker.md) - How to use Docker to test Puppet code and to build images based on the existing Puppet code

  - [Fabric](docs/fabric.md) - A review of Puppet tasks available with Fabric


Managing changes:

  - [Git tasks](docs/git.md) - An overview on how to use Git

  - [Change Process](docs/change_process.md) - A step by step guide on how to manage changes in Puppet code

For the complete documentation of the local classes and defines check the [puppet strings generated docs](http://puppet.pages.lab.psick.io/psick/), automatically generated in PSICK CI pipeline.
