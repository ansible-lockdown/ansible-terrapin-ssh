# Ansible playbook to mitigate where possible Terrapin SSH bug

## Overview

The playbook uses the scanner listed above to discovers is a system is vulnerable.

## Warning

This can make changes to your system you are responsible for testing that this has no adverse effect on your system.

## Associated Links

- [Terrapin Attack](https://terrapin-attack.com)
- [NIST CVE](https://nvd.nist.gov/vuln/detail/CVE-2023-48795)
- [Terrapin Scanner](https://github.com/RUB-NDS/Terrapin-Scanner)

## Tested with

- ansible 11.12+
- EL7,8,9
- Ubuntu18,20,22

## Steps

It is able to check in the following ways using the [variables](#variables) listed below found in default/main.yml and easily overriden.

copies/downloads binary to host to test and runs local test or it is expected to exist.

## SSH server vulnerability

- running from ansible control node to host to test

```sh
terrapin_run_remote_scan: true
```

or

- copies/downloads binary to host to test and runs test on the host.

## SSH Client vulnerability

- Runs on the host setups up a listener and then test connection

## Variables

All variable are set here and can be easily overriden for your environment

```txt
---

## Defaults for running the terrapin-ssh playbook
test_ssh_server: true
test_ssh_client: true
# To actually apply remediation if issues found
# This will change ssh either by removing weak/bad ciphers or upgrading the packages
fix_the_terrapin: false

# To copy supplied Terrapin-Scanner to host
copy_scanner: false
get_scanner: true
# url
scanner_version: 'v1.1.0'
# Can override this in inventory or setup
scanner_platform: amd64
scanner_hash:
  v1.1.0:
    amd64: sha256:b4f326f0817e8c7abb7abfa121331ccfe7969c04fe80fded7c8e7a5283782b35
    arm: sha256:32cd233cb71f77893050ed23e44fc84af757eafb9fc25215bfd4b53591099419
    aarch64: sha256:05acfdaeb079280bedf31c1b87ea7f97f294f2c213d4edda9f4ac5a213c05312
    i386: sha256:8a2a04cd5c7d34f69b81f3871d844520100c1f181087f051223bf5d1a3e88d06
scanner_url: "https://github.com/RUB-NDS/Terrapin-Scanner/releases/download/{{ scanner_version }}/Terrapin_Scanner_Linux_amd64"

# Run against remote server to test ssh server connections
# If false will add Terrapin-Scanner to system
terrapin_run_remote_scan: true
# Path with which to store the terrapin Binary on the host
terrapin_remote_path: '~/'

# If running from delegated localhost path to correct binary
terrapin_localhost_path: '~/go/bin/'

# Terrapin server scan command
terrapin_server_scan_command: "Terrapin-Scanner --json --connect"
terrapin_server_port: 22
el7_based_ciphers: aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes192-ctr,aes128-ctr

# Terrapin Client tests
terrapin_test_client: true
terrapin_client_test_port: 2222
terrapin_client_scan_command: "{{ terrapin_remote_path }}Terrapin-Scanner --json --listen localhost:{{ terrapin_client_test_port }}"
```
