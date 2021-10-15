# <center>Ansible Media Stack</center>

Ansible playbook to deploy a range of open-source applications for a self-hosted media server. 

*The playbook will only install the applications with configurations like port and baseurl, the rest of the configuration have to done manually or imported throught backups.*

## Ansible

Ansible is a radically simple IT automation system. It handles configuration management, application deployment, network automation, and a lot more. An Ansible playbook is a blueprint of automation tasks which can be executed on one or multiple hosts. Ansible is agentless and can even deploy your playbook by connecting to the host via ssh.

## Supported Applications

Following application can be installed using this playbook:
- [Nginx](https://www.nginx.com) - A web server + reverse proxy + load balancer + HTTP cache
- [Jellyfin](https://github.com/jellyfin/jellyfin) - A media solution to collect, manage and stream your media
- [Jfa-Go](https://github.com/hrfee/jfa-go) - A user management app for Jellyfin that provides invite-based account creation as well as other features
- [Ombi](https://github.com/Ombi-app/Ombi) - A web application that gives your shared users the ability to request content by themselves
- [qBittorrent](https://github.com/qbittorrent/qBittorrent) - A bittorrent client to download media from torrent
- [Sonarr](https://github.com/Sonarr/Sonarr) - A PVR for Usenet and BitTorrent users
- [Radarr](https://github.com/Radarr/Radarr) - A movie collection manager for Usenet and BitTorrent users
- [Prowlarr](https://github.com/Prowlarr/Prowlarr) - An indexer manager to integrate with your various PVR apps
- [OpenVPN](https://github.com/dperson/openvpn-client) + [Socks5 Proxy](https://hub.docker.com/r/serjs/go-socks5-proxy/) - A VPN + Proxy to use with your indexer manager.
- [Firewalld](https://firewalld.org) - A dynamically managed firewall

*Support for more applications might be added in future.*

## Supported OS & Architectures

**Supported OS**
- Debian
- Ubuntu

**Supported CPU Archs**
- amd64
- arm
- armd64

*Support for more platforms might be added in future.*

Ansible can be installed on most operating systems, see [installation docs](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

# Getting Started

1. Install Ansible on your local machine, see [installation docs](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
2. Clone the repo 
```bash
git clone https://github.com/arghyadipchak/ansible-media-stack.git
```
3. For Firewalld, install `ansible.posix` plugin
```bash
ansible-galaxy collection install ansible.posix
```
4. Edit `inventory.yml` to setup your hosts, see [inventory variables](#inventory-variables)
5. Edit `vars.yml` to change configuration for the applications, see [playbook configuration](#playbook-configuration)
6. To deploy the playbook, open a terminal in the folder and use the command:
```bash
ansible-playbook -i inventory.yml main.yml
```
7. To install/configure only specific applications or skip some application(s) use tags, see [tags reference](#tags-reference)

## Inventory Configuration

Variable | Reference
-|-
`ansible_connection` | Keep it to ssh as we want ansible to connect to the host via ssh. *Change only if you know what you are doing.*
`ansible_user` | User ansible should use for ssh connection
`ansible_host` | IP Address of the host
`ansible_ssh_key_file` | Location of the ssh key file
`ansible_ssh_pass` | Password for ssh login (*Can be used inplace of* `ansible_ssh_key_file`)

Given `inventory.yml` has only one host. Multiple hosts can be added similarly with a different unique name.

## Playbook Configuration

Variable | Reference
-|-
`jellyfin.port` | 
`jellyfin.baseurl` | 
`jellyfin.movies_folder` | 
`jellyfin.show_folder` | 
`jfago.user` | 
`jfago.home` | 
`jfago.port` | 
`jfago.baseurl` | 
`nginx.name` | 
`nginx.domain` | 
`ombi.user` | 
`ombi.home` | 
`ombi.port` | 
`ombi.baseurl` | 
`prowlarr.user` | 
`prowlarr.home` | 
`prowlarr.port` | 
`prowlarr.baseurl` | 
`qbittorrent.user` | 
`qbittorrent.home` | 
`qbittorrent.port` | 
`qbittorrent.baseurl` | 
`qbittorrent.downloads_folder` | 
`radarr.user` | 
`radarr.homer` | 
`radarr.port` | 
`radarr.baseurl` | 
`sonarr.port` | 
`sonarr.baseurl` | 
`proxy.ovpn_url` | 
`proxy.port` | 

*`port` variables are only for reference and are not applied at this moment.*

## Tags Reference

`ansible-playbook` supports `--tags` (or just `-t`) to run tasks that contain certain tag(s) and `--skip-tags` to skip tasks that contain certain tag(s). The command will look like
```bash
ansible-playbook -i inventory.yml --tags <tag(s)> --skip-tags <tag(s)> main.yml
```

Tag | Reference
-|-
`docker` | 
`firewalld` | 
`jellyfin` | 
`jfa-go` | 
`nginx` | 
`ombi` | 
`prowlarr` | 
`proxy` | 
`qbittorrent` | 
`radarr` | 
`sonarr` | 
`add_repo` | 
`add_service` | 
`add_site` | 
`config` | 
`create_folder` | 
`create_user` | 
`enable` | 
`gpg_key` | 
`install` | 
`restart` | 
`update_baseurl` | 

*`always` and `never` are special tags that run always and never (respectively) unless specified explicitly.*

*Using tag(s) other than the application specific ones (sonarr, radarr, jellyfin, etc) can have unpredicted results if not used correctly. Please use at your own risk.*

# TODO

- [x] Use `community.docker.docker_compose` instead of `shell: docker-compose`
- [ ] Set application ports from `var.yml`

# Notes

This is a highly personalized playbook, so you might not agree with how I deployed certain applications. This was a personal playbook I used to configure my own media server. I have tried to make it as configurable as possible and will try to improve it in future. More variables will be added so that much more configurations can be directly applied to the applications.

# Contribution

Any contribution to this project is highly appreciated!
