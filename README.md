java role
=========
[![License](https://img.shields.io/badge/license-Apache-green.svg?style=flat)](https://raw.githubusercontent.com/lean-delivery/ansible-role-java/master/LICENSE)
[![Build Status](https://travis-ci.org/lean-delivery/ansible-role-java.svg?branch=master)](https://travis-ci.org/lean-delivery/ansible-role-java)

## Summary

This Ansible role has the following features for Oracle Java:

 - Install JRE, JDK, Server-JRE
 - Additional opportunity to install from s3, web, oracle OTN, local source.

DISCLAIMER: usage of any version of this role implies you have accepted the
[Oracle Binary Code License Agreement for Java SE](http://www.oracle.com/technetwork/java/javase/terms/license/index.html).


Requirements
------------

 - Version of the ansible for installation: 2.4
 - **Supported java version**:
   - 7
   - 8
 - **Supported OS**: 
   - CentOS
     - 6
     - 7
   - Windows:
     - 10

## Role Variables

- required
  - `java_package` Java package type.
    Available:
      - `jdk`
      - `jre`
      - `server-jre`

  - `transport` Artifact source transport. Use `local` or `web` for more predictable result. OTN is not enough stable. 
    Available:
      - `oracle-fallback` Downloading artifact from pre-defined oracle otn known artifacts `fallback_oracle_artifacts` with specified:
          - `java_package`
          - `java_major_version`
          - `java_minor_version`
          - `java_arch`
      - `web` Fetching artifact from custom web uri (not supporting idempotent operation)
      - `win-chocolatey` Windows specific package manager
      - `local` Local artifact

- defaults
  - `java_major_version` 8
  - `java_minor_version` 181
  - `java_arch` Package architecture.
    Available:
      - `x64` for x86_64
      - `i586` for x86

  - `java_path` Where java package will be installed
    default: `/opt/{{ java_package }}/`

  - `download_path`: Local folder for downloading artifacts
    default: `/tmp/`

  - `transport_web` URI for http/https artifact  e.g. "http://my-storage.com/jdk-8u172-linux-x64.tar.gz"
  - `transport_local` Path for local artifact e.g. "/tmp/jdk-8u172-linux-x64.tar.gz"


## Some examples of the installing current role

Example Playbook
----------------
```yaml
- name: "Install java"
  hosts: all

  roles:
    - role: "lean-delivery.ansible-role-java"
      java_major_version: 8
      java_minor_version: 181
      java_arch: "x64"
      java_package: "jdk"
```

### Installing java from local file:
```yaml
- name: "Install java"
  hosts: all

  roles:
    - role: "lean-delivery.ansible-role-java"
      transport: "local"
      transport_local: "/tmp/jdk-8u181-linux-x64.tar.gz"
```

License
-------

Apache2

Author Information
------------------

authors:
  - Ivan Bogomazov <ivan_bogomazov@lean-delivery.com>
