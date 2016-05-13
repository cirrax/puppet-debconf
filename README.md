# debconf

[![Build Status](https://travis-ci.org/smoeding/puppet-debconf.svg?branch=master)](https://travis-ci.org/smoeding/puppet-debconf)
[![Puppet Forge](http://img.shields.io/puppetforge/v/stm/debconf.svg)](https://forge.puppetlabs.com/stm/debconf)
[![License](https://img.shields.io/github/license/smoeding/puppet-debconf.svg)](https://raw.githubusercontent.com/smoeding/puppet-debconf/master/LICENSE)

#### Table of Contents

1. [Overview](#overview)
2. [Module Description - What does the module do?](#module-description)
3. [Setup - The basics of getting started with debconf](#setup)
	* [What debconf affects](#what-debconf-affects)
	* [Setup requirements](#setup-requirements)
4. [Usage - Configuration options and additional functionality](#usage)
5. [Reference - An under-the-hood peek at what the module is doing and how](#reference)
5. [Limitations - OS compatibility, etc.](#limitations)
6. [Development - Guide for contributing to the module](#development)

## Overview

Manage entries in the Debian debconf database.

## Module Description

Debian based systems use the debconf database to record configuration choices the user made during the installation of a package. The system uses the stored answers and does not need to query the user again when the package is upgraded or reinstalled at a later time.

The debconf type allows preseeding the database with given answers to allow an unattended package installation or modification of package defaults.

The standard `package` type uses the responsefile parameter to provide a file with preseeded answers during installation. The `debconf` type is a more general solution as it allows to update settings without the need to install the package at the same time.

## Setup

### What debconf affects

The debconf type modifies entries in the Debian debconf database.

### Setup Requirements

This module uses programs provided by the Debian/Ubuntu `debconf` package. Debian assigns the `required` priority to this package so it should already be installed everywhere.

## Usage

### Use dash as replacement for the bourne shell

```puppet
debconf { 'dash/sh':
  type  => 'boolean',
  value => 'true',
}
```

### Automatically set a root password for MySQL during installation

```puppet
debconf { 'mysql-root-passwd':
  package => 'mysql-server-5.5',
  item    => 'mysql-server/root_password',
  type    => 'password',
  value   => 'secret',
}

debconf { 'mysql-root-passwd-again':
  package => 'mysql-server-5.5',
  item    => 'mysql-server/root_password_again',
  type    => 'password',
  value   => 'secret',
}
```

## Reference

- [**Types**](#types)
  - [Type: debconf](#type-debconf)

### Types

#### Type: `debconf`

Ensures presence or absence of a debconf database enty on Debian based systems.

**Parameters for the `debconf` type:**

##### `ensure`

Ensures whether the resource is present. Valid options: `present`, `absent`. Default value: `present`

##### `item`

The configuration item to manage. This is normally a string with at least one slash character. Valid options: String. Default: the title of the debconf resource.

##### `package`

The name of the package that owns the item. Valid options: String. Default: the prefix of the item parameter up to the first slash character.

##### `type`

The data type of the item. Valid options: string, boolean, select, multiselect, note, text, password, title. Default: Undefined.

##### `value`

The value to set. Valid options: String. Default: Undefined.

## Limitations

This module is only useful on Debian based systems where the debconf database is used.

## Development

Feel free to send pull requests for new features.
