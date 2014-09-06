# Introduction

A useful distribute package util for producing embeded devices.

# Features

* Simple package distribution and installation.
* Simple version management. Versions of stubs in any package is memorized.
* NO compiler needed for target system.
* Distribute package directly generated from source repo URL.
* Simple source distribution configuration.

# Semantics

* package (or PKG for short)
  
  A tar ball which can be delivered and to be installed on a target system.

* source / stub

  A source is a code tree hosted on a reachable SVC server.  
  A stub is the name mapped to a source in the package building environment.

* plat

  Short for platform. Indicates a percific target system.

* branch

  For some reason, we want to split a package into two or more packages, and each of them contains some less platfrom binaries.

# Install
# Usage

## Get instruction

* To get help

  ```
  dist help [COMMAND]
  ```

* To get information

  ```
  dist show [all] [config] [source] [output] [PACKAGE]
  ```

  * `all` is same to given all other options except `PACKAGE`.

## Create environment

* To set current folder to be distribution package build folder, and the package name is start with `PACKAGE`.

  ```
  dist init PACKAGE
  ```

## Config environment

* To (re)set package name to `PACKAGE`.

  ```
  dist config name PACKAGE
  ```

  * `PACKAGE` regex: `[a-zA-Z][a-zA-Z0-9_-]*`

* To set/update package full discription name to `NAME`. (*TODO*)

  ```
  dist config fullname NAME
  ```

  **NOTE:** NOT sure where to use this name.

* To set/update package version generation rule to `RULE`.

  ```
  dist config versionrule RULE
  ```

* To set/update a branch with name `BRANCH`.

  ```
  dist branch BRANCH [-p PACKAGE] [-t TAG] [[--] STUB [...]]
  ```

  * `BRANCH`, `PACKAGE`, `TAG` and `STUB` regex: `[a-zA-Z][a-zA-Z0-9_-]*`

* To set/update a source with name `STUB` and link to `URL`.

  ```
  dist source STUB URL [-b BUILDMETHOD] [-c COLLECTMETHOD] [-f]
  ```

  * `STUB` regex: `[a-zA-Z][a-zA-Z0-9_-]*`
  * `URL` can be `svn://SERVER/.*`, `git@SERVER/.*`
  * `BUILDMETHOD` can be `make`, `cmake`, `autotools`
  * `COLLECTMETHOD` can be `copy`, `cpack`, `autotools`

* To remove a source from envionment.

  ```
  dist source STUB [-r]
  ```

  * `STUB` regex: `[a-zA-Z][a-zA-Z0-9_-]*`

## Source version fetch/update

* To fetch given `STUB`s or all stubs we used in package version `VERSION`.

  ```
  dist fetch VERSION [STUB] [...]
  ```

* To get the newest verison of given `STUB`s or all stubs.

  ```
  dist update [STUB] [...]
  ```

## Build package

* To do compile all sources.

  ```
  dist build
  ```

* To create package.

  ```
  dist pack
  ```

## Install package

Prepare the package, assume to be `PACKAGE.VERSION.tar`.

Upload the packege to a HTTP/FTP server, assume the URL to be `http://SERVER/PACKAGE.VERSION.tar`.

Determine if there is any version of this package has been installed on the target system.

* If there is NOT any version of this package has been installed on the target system:

  0. Download package onto the target system. Usually we can do this using `wget` util.  
Assume the downloaded package is `/tmp/PACKAGE.VERSION.tar`.

  0. Extract `install.sh` from the package. assume it is `/tmp/PACKAGE/install.sh`.

  0. Use that script to install the package with given sub-platform.
  
     ```
     /tmp/PACKAGE/install.sh /tmp/PACKAGE.VERSION.tar SUBPLAT
     ```

* Otherwise:

  0. use installed `update.sh` directly.

     ```
     /usr/bin/update.sh http://SERVER/PACKAGE.VERSION.tar
     ```
