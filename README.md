atg-platform-patching
=========
[![License](https://img.shields.io/badge/license-Apache-green.svg?style=flat)](https://raw.githubusercontent.com/lean-delivery/ansible-role-atg-platform-patching/master/LICENSE)
[![Build Status](https://travis-ci.org/lean-delivery/ansible-role-atg-platform-patching.svg?branch=master)](https://travis-ci.org/lean-delivery/ansible-role-atg-platform-patching)
[![Build Status](https://gitlab.com/lean-delivery/ansible-role-atg-platform-patching/badges/master/build.svg)](https://gitlab.com/lean-delivery/ansible-role-atg-platform-patching)

## Summary
--------------

This role installs patches and fix packs on Oracle ATG Web Commerce platform.


Requirements
--------------

 - Minimal Version of the ansible for installation: 2.5
 - **Supported OS**:
   - CentOS
     - 6
     - 7

For more information regarding support matrix please visit <https://support.oracle.com>

ATG platform should be installed preliminarily:
  - lean_delivery.atg-platform


```
For test scenarios atg-platform-patching/requirements.yml is used  
If another roles/versions are required, put requirements.yml to molecule/<scenario_name> and remove in molecule.yml lines  
  options:  
    role-file: requirements.yml
```


Role Variables
--------------

  - `transport` - artifact source transport  
     Available:
      - `web` - fetch artifact from custom web uri
      - `local` - local artifact

      - `atg_patches` - list of atg patches
        - `id` - patch ID
        - `transport_web` - URI for http/https artifact  e.g. "http://my-storage.example.com/p24950065_112000_Generic.zip"
        - `transport_local` - path for local artifact e.g. "/tmp/p24950065_112000_Generic.zip"

```
Set  patches ID as they are defined in official Oracle Documentation
```


  - `download_path` - local folder for downloading artifacts  
    default: `/tmp`

  - `dynamo_root` - path to installed ATG Platform  
    default: `/opt/atg/ATG`


Example Playbook
----------------

### Installing ATG patches for 11.2 from local:
```yaml
- name: "Install ATG 11.2p2 patch and 1.2p2.1 fix pack from local"
  hosts: all

  roles:
    - role: lean_delivery.java
      java_major_version: 7
      java_minor_version: 80
      transport: "local"
      transport_local: "/tmp/jdk-7u80-linux-x64.tar.gz"
    - role: lean_delivery.jboss
      transport: "local"
      transport_local: "/tmp/jboss-eap-6.1.0.zip"
    - role: lean_delivery.atg-platform
      transport: "local"
      transport_local: "/tmp/V78217-01.zip"
    - role: lean_delivery.atg-platform-patching
      transport: "local"
      atg_patches:
        - id: "11.2p2"
          transport_local: "/tmp/p24950065_112000_Generic.zip"
        - id: "11.2p2.1"
          transport_local: "/tmp/p25404313_112020_Generic.zip"
  vars:
    atg_version: "11.2"
    dynamo_root: "/opt/atg/ATG{{ atg_version }}"

```

## License

Apache2

## Authors

team@lean-delivery.com
