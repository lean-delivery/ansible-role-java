java role
=========
[![License](https://img.shields.io/badge/license-Apache-green.svg?style=flat)](https://raw.githubusercontent.com/lean-delivery/ansible-role-java/master/LICENSE)
[![Build Status](https://travis-ci.org/lean-delivery/ansible-role-java.svg?branch=master)](https://travis-ci.org/lean-delivery/ansible-role-java)
[![Build Status](https://gitlab.com/lean-delivery/ansible-role-java/badges/master/build.svg)](https://gitlab.com/lean-delivery/ansible-role-java/pipelines)
[![Galaxy](https://img.shields.io/badge/galaxy-lean__delivery.java-blue.svg)](https://galaxy.ansible.com/lean_delivery/java)
![Ansible](https://img.shields.io/ansible/role/d/27687.svg)
![Ansible](https://img.shields.io/badge/dynamic/json.svg?label=min_ansible_version&url=https%3A%2F%2Fgalaxy.ansible.com%2Fapi%2Fv1%2Froles%2F27687%2F&query=$.min_ansible_version)
## Summary

This Ansible role has the following features for Oracle Java:

 - Install JRE, JDK, Server-JRE
 - Additional opportunity to install from s3, web, oracle OTN, local source.

DISCLAIMER: usage of any version of this role implies you have accepted the
[Oracle Binary Code License Agreement for Java SE](http://www.oracle.com/technetwork/java/javase/terms/license/index.html).


Requirements
------------

 - Version of the ansible for installation: 2.7
 - **Supported java version**:
   - 7
   - 8
   - 11
   - 12
 - **Supported OS**:
   - Ubuntu
     - xenial
     - trusty
   - Debian
     - stretch
     - jessie
   - EL
     - 6
     - 7
   - Windows
     - all

## Role Variables

  - `java_package` Java package type.

    Available:
      - `jdk` (default)
      - `jre`
      - `server-jre`

  - `transport` Artifact source transport. Use `local`, `web` or `s3` for more predictable result. OTN is not enough stable.

    Available:
      - `oracle-fallback` Downloading artifact from pre-defined oracle otn known artifacts `fallback_oracle_artifacts` with specified:
          - `java_package`
          - `java_major_version`
          - `java_minor_version`
          - `java_arch`
        `oracle-fallback` is default value for `transport` variable.
      - `web` Fetching artifact from custom web url
      - `chocolatey` Windows specific package manager
      - `local` Local artifact stored on ansible master
      - `s3` artifact in s3 bucket   
        **Notice** using `s3` transport requires specific packages to be installed on target host:
          - 'botocore'
          - 'boto'
          - 'boto3'
        These packages are not included in given role. You should install them preliminary.

  - `java_tarball_install` - boolean parameter to choose between tarball and package installation. Default is `True`.
  - `java_major_version` - major version of oracle-java (6,7,8, 11 etc.) Default is 8.
  - `java_minor_version` - minor version of oracle-java. For version `8.202` minor will be `202` (default). 
  - `java_arch` Package architecture.

    Available:
      - `x64` for x86_64 (default)
      - `i586` for x86

  - `java_path` Where java package will be installed

    default values depend on OS distribution: 
      - RedHat: `/usr/java`
      - Debian: `/usr/lib/jvm`
      - Windows: `C:\Program Files\Java`

  - `download_path`: Local folder for downloading artifacts

    Linux default: `/tmp`

    Windows default: `TEMP environment variable`

  - `transport_web` URI for http/https artifact  e.g. "http://my-storage.com/jdk-8u172-linux-x64.tar.gz"

  - `transport_local` Path for local artifact e.g. "/tmp/jdk-8u172-linux-x64.tar.gz"

  - `transport_s3_bucket` - s3 bucket name

    default: `s3_bucket`
  - `transport_s3_path` - path to patch folder in bucket

    default: `/folder`
  - `transport_s3_aws_access_key` - aws key. Need to set in role or set as parameter or set env variables according https://docs.ansible.com/ansible/latest/modules/aws_s3_module.html

    default: `{{ lookup('env','AWS_ACCESS_KEY') }}`
  - `transport_s3_aws_secret_key` - aws secret key. Need to set in role or set as parameter or set env variables according https://docs.ansible.com/ansible/latest/modules/aws_s3_module.html

    default: `{{ lookup('env','AWS_SECRET_KEY') }}`

# Configure unlimited policy
  - `java_unlimited_policy_enabled` - to apply unlimited policy

    default: `False`
  - `java_unlimited_policy_transport` Artifact source transport. Use `local`, `web` or `s3` for more predictable result.

    Available:
      - `oracle-fallback` Downloading artifact from pre-defined oracle otn known artifacts `fallback_oracle_security_policy_artifacts` with specified:
      - `web` Fetching artifact from custom web url
      - `local` Local artifact stored on ansible master
      - `s3` artifact in s3 bucket
  - `java_unlimited_policy_transport_web` URI for http/https artifact  e.g. "http://my-storage.com/jce_policy-8.zip"
  - `java_unlimited_policy_transport_local` Path for local artifact e.g. "/tmp/jce_policy-8.zip"
  - `java_unlimited_policy_transport_s3_bucket` - s3 bucket name

    default: `s3_bucket`
  - `java_unlimited_policy_transport_s3_path` - path to patch folder in bucket

    default: `/folder`

## Some examples of the installing current role

ansible-galaxy install lean_delivery.java

Example Playbook
----------------
```yaml
- name: Install java
  hosts: all

  roles:
    - role: lean_delivery.java
      java_major_version: 8
      java_minor_version: 202
      java_arch: x64
      java_package: jdk
```

### Installing java from local file:
```yaml
- name: Install java
  hosts: all

  roles:
    - role: lean_delivery.java
      transport: local
      transport_local: /tmp/jdk-8u181-linux-x64.tar.gz
```
### Installing java from S3 bucket:
Before install you should prepare host to use aws_s3 module
https://docs.ansible.com/ansible/latest/modules/aws_s3_module.html#requirements
```yaml
- name: Install java
  hosts: all


  roles:
    - role: lean_delivery.java
        java_package: jre
        java_major_version: 8
        transport: s3
        transport_s3_bucket: java-s3-bucket
        transport_s3_path: /java/jre-8u181-linux-x64.tar.gz

```
### Installing java on Windows host with win_chocolatey:
```yaml
- name: Install java
  hosts: windows

  roles:
    - role: lean_delivery.java
      java_major_version: 8
      java_minor_version: 201
      transport: chocolatey
```

### Installing java 11 on Windows host with oracle-fallback:
```yaml
- name: Install java
  hosts: windows

  roles:
    - role: lean_delivery.java
      java_major_version: 11
```

License
-------

Apache

Author Information
------------------

authors:
  - Lean Delivery Team <team@lean-delivery.com>
