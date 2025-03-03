# Ansible Role: certs - CA and Certificates Management

## Overview

This Ansible role manages Certificate Authority (CA) and certificate generation for etcd clusters. It includes tasks for generating CA root certificates, node certificates, and client certificates.

---

## Default Variables

The following variables are defined in `defaults/main.yaml`:

### CA Certificate Paths

- `certs_ca_certificate_path`: Path to store CA certificates. Default: `/etc/etcd-ca`
- `certs_etcd_certificate_path`: Path to store etcd certificates. Default: `/etc/etcd/tls`
- `certs_ca_bundle_path`: Path to etcd CA bundle. Default: `{{ certs_etcd_certificate_path }}/etcd-ca.pem`
- `certs_output_path`: Path to store the generated etcd CA bundle. Default: `{{ playbook_dir }}/output/{{ certs_etcd_env }}/etcd-ca-bundle.pem`

### CA Root Certificates

- `certs_ca_certificate_root_cname`: Common name for CA root certificate. Default: `etcd`
- `certs_ca_certificate_root_key_file`: Path to the CA root key file. Default: `{{ certs_ca_certificate_path }}/ca-root.key`
- `certs_ca_certificate_root_crt_file`: Path to the CA root certificate file. Default: `{{ certs_ca_certificate_path }}/ca-root.pem`
- `certs_ca_certificate_root_csr_file`: Path to the CA root CSR file. Default: `{{ certs_ca_certificate_path }}/ca-root.csr`
- `certs_ca_root_key_passkey`: Passkey for CA root key. Default: `{{ vault_certs_ca_root_key_passkey }}`
- `certs_ca_root_ownca_not_after`: Expiration time for root CA. Default: `+3652d`
- `certs_ca_certificate_set_immutable`: Set immutable attribute on CA files. Default: `false`

### etcd Node Certificates

- `certs_etcd_key_file`: Path to etcd node key file. Default: `{{ certs_etcd_certificate_path }}/etcd-node.key`
- `certs_etcd_crt_file`: Path to etcd node certificate file. Default: `{{ certs_etcd_certificate_path }}/etcd-node.pem`
- `certs_etcd_csr_file`: Path to etcd node CSR file. Default: `{{ certs_etcd_certificate_path }}/etcd-node.csr`
- `certs_etcd_csr_forced`: Force re-creation of CSR. Default: `false`
- `certs_etcd_common_name`: Common name for etcd node certificate. Default: `{{ inventory_hostname }}`
- `certs_etcd_ownca_not_after`: Expiration time for etcd node certificate. Default: `+365d`
- `certs_etcd_sans`: Subject Alternative Names for etcd node certificates:
  ```yaml
  - DNS:localhost
  - DNS:etcd-node-a
  - DNS:etcd-node-b
  - DNS:etcd-node-c
  - IP:127.0.0.1
  - IP:10.0.2.237
  - IP:10.0.2.188
  - IP:10.0.2.237
  ```

### etcd Client Certificates

- `certs_etcd_client_certificate_path`: Path for storing etcd client certificates. Default: `{{ certs_etcd_certificate_path }}`
- `certs_etcd_client_certs`: List of client certificates to generate:
  ```yaml
  - name: etcd-tenant-a
    cname: etcd-tenant-a
  - name: etcd-tenant-b
    cname: etcd-tenant-b
  ```
- `certs_etcd_client_csr_forced`: Force re-creation of client CSRs. Default: `false`
- `certs_etcd_client_ownca_not_after`: Expiration time for client certificates. Default: `+365d`

---

## Tasks

### ca.yaml

Manages the creation of CA certificates:

- Creates a directory for CA certificates.
- Generates CA root key.
- Creates CSR for CA certificate.
- Generates self-signed CA certificate.
- Optionally sets immutable attributes on CA files.

### etcd-crt.yaml

Manages the creation of etcd node certificates:

- Creates directory for etcd certificates.
- Generates private keys.
- Creates CSRs for etcd nodes.
- Signs node certificates using the CA.
- Writes signed certificates to the etcd node path.

### client-crt.yaml

Manages the creation of etcd client certificates:

- Creates a directory for client certificates.
- Generates private keys for client certificates.
- Creates CSRs for each client.
- Signs client certificates using the CA.
- Writes signed client certificates to the client certificate path.

---

## Handlers

- `Store ca csr`: Stores the generated CA CSR.
- `Store etcd csr`: Stores the etcd node CSR.
- `Store etcd client csr`: Stores the client CSR.

---

## Usage

Include this role in your playbook and customize the variables as needed:

```yaml
- name: Deploy etcd certificates
  hosts: etcd_nodes
  roles:
    - role: ca_and_certificates_management
      vars:
        certs_etcd_env: 'production'
        certs_etcd_cname: 'etcd-node-a'
```

---

## License

MIT

## Author

andris zbitkovskis
