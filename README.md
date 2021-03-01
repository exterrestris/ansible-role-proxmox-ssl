# Ansible role `exterrestris.proxmox_ssl`

An Ansible role for setting up a cron job to fetch an SSL certificate via SSH and install it into Proxmox using `pvenode`.

## Requirements

- SSH access to certificate source using password-less public key authentication
- Full certificate chain in PEM format
- Private key in PEM format
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

## Example - Installing Let's Encrypt wildcard certificate issued to pfSense

pfSense can automatically request and install a Let's Encrypt certificate using the [ACME](https://docs.netgate.com/pfsense/en/latest/packages/acme/index.html) package. If you request a wildcard certificate, you can reuse this certificate on other devices instead of dealing with managing additional certificates on a per-device basis

The ACME package saves the issued certificate in a variety of formats, including the full chain required by Proxmox

### Requirements

- ACME package configured within pfSense to issue and automatically renew a wildcard certificate
- The `Write Certificates` option within `Services / Acme / Settings / General Settings` enabled to copy the issued certificates to the `/conf/acme/` directory
- SSH access to pfSense using public-key authentication for a user with read access to `/conf/acme/`

### Configuration

```Yaml
proxmox_ssl_ssh_host: "pfsense-host"
proxmox_ssl_ssh_user: admin
proxmox_ssl_ssh_cert_path: "/conf/acme/certificate-name.fullchain"
proxmox_ssl_ssh_key_path: "/conf/acme/certificate-name.key"
proxmox_ssl_run_now: yes
```

Replace `pfsense-host` with the hostname or IP of your pfSense router, and `certificate-name` with the name you gave your certficate when configuring it within the ACME module