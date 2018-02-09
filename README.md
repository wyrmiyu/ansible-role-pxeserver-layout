This an Ansible role for setting up mounted ISO image layout for a PXE server that serves kickstart files for RHEL/CentOS installations over HTTPD. The role asumes that PXE infrastructure server runs the full stack:

- HTTPD
- TFTPD
- DHCPD
- BIND


This playbook has been created originally to be used with RHV-H iso images. It will:

- Copy over iso-images
- Create directory structures under DocumentRoot
- Loop mount those images
- Extract RHV squashfs installtion image and put that in the directory tree
- Create and put available kickstart configuration file

## Requirements

This module is intended to be run with: https://github.com/wyrmiyu/ansible-role-pxeserver

## Role Variables

| Variable              | Default                        | Comment                                                          |
| :---                  | :---                           | :---                                                             |
| `domain`              | `example.com`                  | Needed to make sure the local bind server is in place            |
| `iso_source_path`     | `.`                            | Local path where ISO files will be copied over from.             |
| `ks_root_pass`        | `password`                     | Password for root set in the kickstart template.                 |
| `pxe_env_uri`         | `pxe`                          | The top-level URI/path element for the environment.              |
| `ks_uri`              | `<pxe_env_uri>/ks`             | Where the kickstart configurations are served from.              |
| `iso_uri`             | `<pxe_env_uri>/isos`           | Where the iso images are server from.                            |
| `mounted_iso_uri`     | `<pxe_env_uri>/mnt`            | Where the loop mounted images are served from.                   |
| `pxeserver_images`    | []                             | List of dicts specifying PXEboot images to be served. See below. |
| `pxeserver_ip`        | `ansible_default_ipv4.address` | IP address of the PXE server                                     |

You must specify the boot images to be served with the variable `pxeserver_images`, a dict containing the keys listed below. Keys are *mandatory* unless specified.

| Key              | Value                                                             |
| :---             | :---                                                              |
| `name`           | A unique identifier for the image.                                |
| `default`        | When `true`, this image is chosen as the default. May be omitted. |
| `kernel_url`     | URL where to download the kernel image.                           |
| `initrd_url`     | URL where to download the initrd image.                           |
| `kickstart_url`  | URL where to download the kickstart file.                         |
| `kickstart_path` | Location where to copy the kickstart file from.                   |
| `label`          | Label for the PXE boot menu entry of this image.                  |
| `pxe_url`        | URL where the PXE environment can be reached with HTTP.           |
| `isofile`        | Filename of the ISO image.                                        |


## Dependencies

This role depends on:

- [bertvv.bind](https://galaxy.ansible.com/bertvv/bind/)
- [bertvv.httpd](https://galaxy.ansible.com/bertvv/httpd/)
- [bertvv.tftp](https://galaxy.ansible.com/bertvv/tftp/)
- [bertvv.dhcp](https://galaxy.ansible.com/bertvv/dhcp/)
