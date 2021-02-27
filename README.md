# Ansible role `exterrestris.proxmox_ssl`

An Ansible role for setting up a cron job to fetch an SSL certificate via SSH and install it into Proxmox.

## Requirements

SSH access to certificate source using public key authentication

## Role Variables

### Required

| Variable                    | Comments                                                |
| :---                        | :---                                                    |
| `proxmox_ssl_ssh_host`      | Hostname/IP address from where to fetch the certificate |
| `proxmox_ssl_ssh_user`      | Username to use on `proxmox_ssl_ssh_host`               |
| `proxmox_ssl_ssh_cert_path` | Path to the full certificate chain in PEM format        |
| `proxmox_ssl_ssh_key_path`  | Path to the certificate private key in PEM format       |

### Optional

| Variable                     | Default                        | Comments                        |
| :---                         | :---                           | :---                            |
| `proxmox_ssl_script_dir`     | /usr/local/sbin/               | Install script into directory   |
| `proxmox_ssl_script_name`    | install-ssh-cert.sh            | File name for script            |
| `proxmox_ssl_script_owner`   | root                           | Owner for script                |
| `proxmox_ssl_script_group`   | root                           | Group for script                |
| `proxmox_ssl_script_mode`    | 0700                           | Mode/permissions for script     |
| `proxmox_ssl_tmp_cert`       | /tmp/proxmox_ssl_tmp_cert      | Temp file for certificate chain |
| `proxmox_ssl_tmp_key`        | /tmp/proxmox_ssl_tmp_key       | Temp file for private key       |
| `proxmox_ssl_cron_name`      | 'install pveproxy certificate' | Name of cron job                |
| `proxmox_ssl_cron_file`      | 'install-pveproxy-certificate' | Filename for cron job           |
| `proxmox_ssl_cron_frequency` | daily                          | Cron frequency                  |
| `proxmox_ssl_run_now`        | no                             | Run script immediately          |
