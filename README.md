java role
=========
[![License](https://img.shields.io/badge/license-Apache-green.svg?style=flat)](https://raw.githubusercontent.com/lean-delivery/ansible-role-java/master/LICENSE)
[![Build Status](https://travis-ci.org/lean-delivery/ansible-role-java.svg?branch=master)](https://travis-ci.org/lean-delivery/ansible-role-java)
[![Build Status](https://gitlab.com/lean-delivery/ansible-role-java/badges/master/build.svg)](https://gitlab.com/lean-delivery/ansible-role-java/pipelines)
[![Galaxy](https://img.shields.io/badge/galaxy-lean__delivery.java-blue.svg)](https://galaxy.ansible.com/lean_delivery/java)
![Ansible](https://img.shields.io/ansible/role/d/27687.svg)
![Ansible](https://img.shields.io/badge/dynamic/json.svg?label=min_ansible_version&url=https%3A%2F%2Fgalaxy.ansible.com%2Fapi%2Fv1%2Froles%2F27687%2F&query=$.min_ansible_version)
## Summary

This Ansible role has the following features for:

**OpenJDK**

- Install JRE, JDK
- Additional opportunity to install from openjdk-fallback, repositories, s3, web, chocolatey, local source.

**Oracle Java:**

 - Install JRE, JDK, Server-JRE
 - Additional opportunity to install from s3, web, local source.

**SAPJVM**

- Install JDK
- Additional opportunity to install from sapjvm-fallback, s3, web, local source.

**ZULU**

- Install JDK
- Additional opportunity to install from zulu-fallback, s3, web, local source, chocolatey.

**AdoptOpenJDK**

- Install JDK, JRE
- Additional opportunity to install from adoptopenjdk-fallback, repositories, web, local source, s3, chocolatey.


**SapMachine**

- Install JDK, JRE
- Additional opportunity to install from sapmachine-fallback, web, local source, chocolatey (only latest version), s3.

**Alibaba Dragonwell 8 JDK**

- Install JDK
- Alibaba Dragonwell 8 corresponds to OpenJDK 8 and is compatible with the Java SE Standard
- Linux/x86_64 platform only
- Additional opportunity to install from dragonwell8-fallback, web, local source, s3.

DISCLAIMER: usage of any version of this role implies you have accepted the
[Oracle Binary Code License Agreement for Java SE](http://www.oracle.com/technetwork/java/javase/terms/license/index.html).


Requirements
------------

 - Version of the ansible for installation: 2.7
- **Supported OpenJDK version**:
   - 8
      - EL 6: repositories, tarball
      - EL 7: repositories, tarball
      - EL 8: repositories, tarball
      - Ubuntu bionic: repositories, tarball
      - Debian stretch: repositories, tarball
      - Windows: tarball
   - 11
      - EL 6: tarball
      - EL 7: repositories, tarball
      - Ubuntu bionic: repositories, tarball
      - Debian stretch: tarball
      - Windows: tarball
   - 12
      - EL 6: tarball
      - EL 7: tarball
      - EL 8: tarball
      - Ubuntu bionic: tarball
      - Debian stretch: tarball
      - Windows: tarball
   - 13
      - EL 6: tarball, openjdk-fallback
      - EL 7: tarball, openjdk-fallback
      - EL 8: tarball, openjdk-fallback
      - Ubuntu bionic: tarball, openjdk-fallback
      - Debian stretch: tarball, openjdk-fallback
      - Windows: tarball, openjdk-fallback
 - **Supported oracle java version**:
   - 7
   - 8
   - 11
   - 12
 - **Supported sapjvm version**:
   - 7
   - 8
 - **Supported zulu version**:
   - 7
   - 8
   - 11
   - 12
 - **Supported AdoptOpenJDK version**:
   - 8
   - 11
   - 12
   - 13
- **Supported SapMachine version**:
    - 11
      - EL 7: sapmachine-fallback
      - EL 8: sapmachine-fallback
      - Ubuntu bionic: sapmachine-fallback
      - Debian stretch: sapmachine-fallback
      - Windows: chocolatey (only latest version, don't support java_minor_version variables), sapmachine-fallback
   - 12
      - EL 7: tarball
      - EL 8: tarball
      - Ubuntu bionic: tarball
      - Debian stretch: tarball
      - Windows: tarball
   - 13
      - EL 7: sapmachine-fallback
      - EL 8: sapmachine-fallback
      - Ubuntu bionic: sapmachine-fallback
      - Debian stretch: sapmachine-fallback
      - Windows: chocolatey (only latest version, don't support java_minor_version variables), sapmachine-fallback
 - **Supported Alibaba Dragonwell version**:
   - 8.0.0
   - 8.1.1
 - **Supported OS**:
   - Ubuntu
     - bionic
     - xenial
     - trusty
   - Debian
     - stretch
   - Amazon Linux
   - Amazon Linux 2
   - EL
     - 6
     - 7
     - 8
   - Windows
     - all

## Role Variables
  - `java_distribution` Java distribution type, one of:
     - `openjdk` (default)
     - `oracle_java`
     - `sapjvm`
     - `zulu`
     - `adoptopenjdk`
     - `sapmachine`
     - `dragonwell8`

        **Notice**: this variable is mandatory in case of installing other distribution than 'openjdk'.

  - `java_package` Java package type.

    Available:
      - `jdk` (default)
      - `jre`

  - `transport` Artifact source transport. Use `openjdk-fallback`(OpenJDK only), `repositories`(OpenJDK only), `sapjvm-fallback`(SAPJVM only), `adoptopenjdk-fallback`(AdoptOpenJDK only),`sapmachine-fallback`(SapMachine only), `zulu-fallback`(ZULU only), `dragonwell8-fallback`(Alibaba Dragonwell only), `local`, `web` or `s3` for more predictable result.

    Available:
      - `repositories` Installing OpenJDK java from system repositories (yum or apt, Linux only)
      - `web` Fetching artifact from custom web url
      - `chocolatey` Windows specific package manager (Supported OpenJDK: JDK 11,12 or JRE 8)
      - `local` Local artifact stored on ansible master (can be used as cache for other transport)
      - `s3` Download artifact from s3 bucket (Linux clients only, for Windows please use other transports)
      - `sapjvm-fallback` fetching artifact from SAP site.
      - `zulu-fallback` fetching artifact from AZUL site.
      - `adoptopenjdk-fallback` fetching artifact from adoptopenjdk site.
      - `sapmachine-fallback` fetching artifact from SapMachine site.
      - `dragonwell8-fallback` fetching artifact from GitHub.
      - `openjdk-fallback` fetching artifact from jdk.java.net.
         This is default value for `transport` variable

        **Notice** using `s3` transport requires specific packages to be installed on target host:
          - 'botocore'
          - 'boto'
          - 'boto3'
        These packages are not included in given role. You should install them preliminary.

  - `java_tarball_install` - boolean parameter to choose between tarball and package installation. Default is `true` if `transport` is not `repositories`.
  - `java_major_version` - major version of OpenJDK (8,11,12) or oracle-java (6,7,8, 11 etc.) Default is 12.
  - `java_minor_version` - minor version of oracle-java. For version `8.202` minor will be `202` (default). For OpenJDK this variable not needed setup manually.
  - `java_arch` Package architecture. (With installing OpenJDK from repositories its variable you may use only for RHEL )

    Available:
      - `x64` for x86_64 (default)
      - `i586` for x86

  - `java_path` Where java package will be installed.
    **Notice** Not use this variable if transport = repositories selected

    default values depend on OS distribution:
      - RedHat: `/usr/java` (`/usr/lib/jvm` from repositories)
      - Debian: `/usr/lib/jvm`
      - Windows: `C:\Program Files\Java`

  - `java_download_path`: Local folder for downloading artifacts

    Linux default: `/tmp`

    Windows default: `TEMP environment variable`

  - `transport_web` URI for http/https artifact  e.g. "http://my-storage.com/jdk-8u172-linux-x64.tar.gz"
  - `transport_web: "https://download.java.net/java/GA/jdk11/9/GPL/openjdk-11.0.2_linux-x64_bin.tar.gz"` (OpenJDK 11 for example)
  - `transport_local` Path for local artifact e.g. "/tmp/jdk-8u172-linux-x64.tar.gz"

  - `transport_s3_bucket` - s3 bucket name

    default: `s3_bucket`
  - `transport_s3_path` - path to patch folder in bucket

    default: `/folder`
  - `transport_s3_aws_access_key` - aws key. Need to be set as parameter or env variables according to https://docs.ansible.com/ansible/latest/modules/aws_s3_module.html

    default: `{{ lookup('env','AWS_ACCESS_KEY') }}`
  - `transport_s3_aws_secret_key` - aws secret key. Need to be set as parameter or env variables according to https://docs.ansible.com/ansible/latest/modules/aws_s3_module.html

    default: `{{ lookup('env','AWS_SECRET_KEY') }}`

#  Configure AdoptOpenJDK
  - `adoptopenjdk_impl` AdoptOpenJDK Implementation
     - `hotspot` (default)
     - `openj9`

# Configure unlimited policy
  - `java_unlimited_policy_enabled` - to apply unlimited policy

    default: `false`
  - `java_unlimited_policy_transport` Artifact source transport. Use `local`, `web` or `s3` for more predictable result.

    Available:
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
### Installing OpenJDK 13 from openjdk-fallback (default role behaviour):
```yaml
- name: Install openjdk java
  hosts: all

  roles:
    - role: lean_delivery.java
```

### Installing OpenJDK 8 from repositories:
```yaml
- name: Install openjdk java
  hosts: all

  roles:
    - role: lean_delivery.java
      transport: repositories
      java_major_version: 8
```

### Installing OpenJDK 11 from web:
```yaml
- name: Install openjdk java
  hosts: all

  roles:
    - role: lean_delivery.java
      java_major_version: 11
      java_tarball_install: true
      transport: web
      transport_web: https://download.java.net/java/GA/jdk11/9/GPL/openjdk-11.0.2_linux-x64_bin.tar.gz
```

### Installing Oracle java 8 from local file:
```yaml
- name: Install oracle java
  hosts: all

  roles:
    - role: lean_delivery.java
      java_distribution: oracle_java
      transport: local
      transport_local: /tmp/jdk-8u181-linux-x64.tar.gz
```
### Installing Oracle java 8 from S3 bucket:
Before install you should prepare host to use aws_s3 module
https://docs.ansible.com/ansible/latest/modules/aws_s3_module.html#requirements
```yaml
- name: Install java
  hosts: all
  
  roles:
    - role: lean_delivery.java
        java_distribution: oracle_java
        java_package: jre
        java_major_version: 8
        transport: s3
        transport_s3_bucket: java-s3-bucket
        transport_s3_path: /java/jre-8u181-linux-x64.tar.gz

```
### Installing OpenJDK 11.0.2 on Windows host with win_chocolatey:
```yaml
- name: Install java
  hosts: windows

  roles:
    - role: lean_delivery.java
      java_package: jdk
      transport: chocolatey
      java_major_version: 11
      java_minor_version: 0.2
```
### Installing SAPJVM 8 from sapjvm-fallback:
```yaml
- name: Install sapjvm
  hosts: all

  roles:
    - role: lean_delivery.java
      java_distribution: sapjvm
      transport: sapjvm-fallback
      java_major_version: 8
```
### Installing ZULU 12 from zulu-fallback:
```yaml
- name: Install zulu
  hosts: all

  roles:
    - role: lean_delivery.java
      java_distribution: zulu
      transport: zulu-fallback
```
### Installing AdoptOpenJDK 8-openj9-jre from adoptopenjdk-fallback:
```yaml
- name: Install AdoptOpenJDK
  hosts: all

  roles:
    - role: lean_delivery.java
      java_distribution: adoptopenjdk
      transport: adoptopenjdk-fallback
      java_package: jre
      adoptopenjdk_impl: openj9
      java_major_version: 8
```

### Installing SapMachine sapmachine-jre-10 from sapmachine-fallback:
```yaml
- name: Install SapMachine
  hosts: all

  roles:
    - role: lean_delivery.java
      java_distribution: sapmachine
      transport: sapmachine-fallback
      java_package: jre
      java_major_version: 10
```
### Installing Alibaba Dragonwell 8 from dragonwell8-fallback:
```yaml
- name: Install Alibaba Dragonwell8
  hosts: all

  roles:
    - role: lean_delivery.java
      java_distribution: dragonwell8
      transport: dragonwell8-fallback
      java_major_version: 8
```
License
-------

Apache

Author Information
------------------

authors:
  - Lean Delivery Team <team@lean-delivery.com>
