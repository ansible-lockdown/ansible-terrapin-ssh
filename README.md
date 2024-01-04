# Terrapin-SSH Playbook Documentation

## Overview

The `terrapin-ssh` playbook is designed to test and remediate SSH server and client configurations on remote hosts using the Terrapin-Scanner tool. It provides flexibility in configuring the scan and remediation options according to specific requirements.

### Testing

- ansible 2.11.12+
- ansible 2.16.1
- EL7,8,9
- Ubuntu18,20,22

## Warning

This can make changes to your system you are responsible for testing that this has no adverse effect on your system.

## Playbook Defaults

The playbook includes default settings that can be adjusted based on your needs. These defaults are defined in the playbook configuration file.

```yaml
test_ssh_server: true
test_ssh_client: true
fix_the_terrapin: false
copy_scanner: false
get_scanner: true
scanner_version: 'v1.1.0'
scanner_platform: amd64
scanner_hash:
  v1.1.0:
    amd64: sha256:b4f326f0817e8c7abb7abfa121331ccfe7969c04fe80fded7c8e7a5283782b35
    arm: sha256:32cd233cb71f77893050ed23e44fc84af757eafb9fc25215bfd4b53591099419
    aarch64: sha256:05acfdaeb079280bedf31c1b87ea7f97f294f2c213d4edda9f4ac5a213c05312
    i386: sha256:8a2a04cd5c7d34f69b81f3871d844520100c1f181087f051223bf5d1a3e88d06
scanner_url: "https://github.com/RUB-NDS/Terrapin-Scanner/releases/download/{{ scanner_version }}/Terrapin_Scanner_Linux_amd64"
terrapin_run_remote_scan: true
terrapin_remote_path: '~/'
terrapin_localhost_path: '~/go/bin/'
terrapin_server_scan_command: "Terrapin-Scanner --json --connect"
terrapin_server_port: 22
el7_based_ciphers: aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes192-ctr,aes128-ctr
terrapin_client_test_port: 2222
terrapin_client_scan_command: "{{ terrapin_remote_path }}Terrapin-Scanner --json --listen localhost:{{ terrapin_client_test_port }}"
```

## Configuration Options

### SSH Server Testing and Remediation

- `test_ssh_server`: If set to `true`, the playbook will test the SSH server configurations. If issues are found, remediation can be applied if `fix_the_terrapin` is also set to `true`.
- `fix_the_terrapin`: If set to `true`, remediation actions will be applied to fix identified SSH server issues.

### Terrapin Scanner

- `copy_scanner`: If set to `true`, the playbook will copy the supplied Terrapin-Scanner to the host.
- `get_scanner`: If set to `true`, the playbook will download Terrapin-Scanner from the specified URL.
- `scanner_version`: Version of Terrapin-Scanner to be used.
- `scanner_platform`: Platform architecture for Terrapin-Scanner.
- `scanner_url`: URL to download Terrapin-Scanner.

### SSH Server Remote Scan

- `terrapin_run_remote_scan`: If set to `true`, the playbook will run the Terrapin-Scanner against a remote server to test SSH server connections.
- `terrapin_remote_path`: Path to store the Terrapin binary on the host.

### Localhost Path

- `terrapin_localhost_path`: Path to the correct binary if running from the delegated localhost.

### Terrapin Server Scan Configuration

- `terrapin_server_scan_command`: Terrapin server scan command.
- `terrapin_server_port`: Port for Terrapin server scan.
- `el7_based_ciphers`: List of ciphers for SSH server on EL7-based systems.

### Terrapin Client Testing

- `test_ssh_client`: If set to `true`, the playbook will perform Terrapin-Scanner tests on the client.
- `terrapin_client_test_port`: Port for Terrapin client testing.
- `terrapin_client_scan_command`: Terrapin client scan command.

## Usage

1. Adjust the playbook defaults based on your requirements.
2. Run the playbook to test and remediate SSH server and client configurations.

**Note:** Ensure that Terrapin-Scanner dependencies are met and the specified paths and URLs are accessible.

For more information about Terrapin-Scanner, refer to the official documentation: [Terrapin-Scanner GitHub Repository](https://github.com/RUB-NDS/Terrapin-Scanner)
