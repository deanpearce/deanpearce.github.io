---
title: "Building DBD::Oracle on MacOS"
date: "2017-07-31"
categories: 
  - "development"
tags: 
  - "dbdoracle"
  - "macos"
  - "oracle"
  - "perl"
---

As you may know, DBD::Oracle is one of the most challenging DB drivers to build and install. I recently switched to MacOS Sierra and found myself needing to install DBD::Oracle within my Perlbrew installed local Perl. I opted to use the latest Instant Client from Oracle (12.1) and the latest stable DBD::Oracle build (1.74) for my system.

### Install Oracle Instant Client 12.1 Packages

The easiest way to get DBD::Oracle built is against the Instant Client. Download the following 3 packages (or the corresponding 3 for the Client version you desire) to your system, and extract them to a folder on your system. I opted to keep my install local within my /Users directory, but you can opt to install it to /Library/Oracle as well. Simply extract all 3 zips to the same path in your system.

- [instantclient-basic-macos.x64-12.1.0.2.0.zip](http://download.oracle.com/otn/mac/instantclient/121020/instantclient-basic-macos.x64-12.1.0.2.0.zip)
- [instantclient-sqlplus-macos.x64-12.1.0.2.0.zip](http://download.oracle.com/otn/mac/instantclient/121020/instantclient-sqlplus-macos.x64-12.1.0.2.0.zip)
- [instantclient-sdk-macos.x64-12.1.0.2.0.zip](http://download.oracle.com/otn/mac/instantclient/121020/instantclient-sdk-macos.x64-12.1.0.2.0.zip)

### Build DBD::Oracle 1.74 Module

On MacOS Sierra I had to prepare the following steps to ensure a successful build. You may need to adjust the steps based on the location of your Instant Client. The primary issue is the fact that MacOS Sierra does not like the dynamic path linking using @rpath. A fix is to just set your full path into the binary.

```
# setup the Oracle Instant Client environment
export ORACLE_HOME=/Library/Oracle/instantclient_12_1
export DYLD_LIBRARY_PATH=$ORACLE_HOME

# download and unpack DBD::Oracle
cd /tmp
wget http://search.cpan.org/CPAN/authors/id/P/PY/PYTHIAN/DBD-Oracle-1.74.tar.gz
tar -xzf DBD-Oracle-1.74.tar.gz
cd ./DBD-Oracle-1.74/

# build the module against 12.1
perl Makefile.PL
make

# fix for problem with dynamic linking on MacOS Sierra
install_name_tool -change @rpath/libclntsh.dylib.12.1 \
    /Library/Oracle/instant_client_12_1/libclntsh.dylib.12.1 \
    ./blib/arch/auto/DBD/Oracle/Oracle.bundle

# complete the install
make install
```

### Post-Install Tips

After installing, you may want to do a few things to make your experience easier.

1. Add permanent paths to your .bash\_profile
    - export ORACLE\_HOME=/Library/Oracle/instantclient\_12\_1
    - export DYLD\_LIBRARY\_PATH=$ORACLE\_HOME
    - export PATH=$ORACLE\_HOME:$PATH
2. Add a tnsnames.ora for easier configuration
    - cd $ORACLE\_HOME
    - mkdir -p network/ADMIN && cd network/ADMIN
    - touch tnsnames.ora
