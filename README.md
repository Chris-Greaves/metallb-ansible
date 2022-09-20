# MetalLB Ansible Playbook

This repo contains an Ansible Playbook able to deploy MetalLB to a cluster using the same commands listed in the [MetalLB Docs](https://metallb.universe.tf/installation/)

## Prerequisite

- [Ansible](https://www.ansible.com/) - install guide [here](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
    - Note: for windows users, you will need to install Ansible in [WSL](https://learn.microsoft.com/en-us/windows/wsl/about).
- Machines must have passwordless SSH access (only needs to be run on one machine per cluster)

## Usage

1. Clone and enter the github repo.

```bash
git clone https://github.com/Chris-Greaves/metallb-ansible
cd metallb-ansible
```

2. Copy the sample inventory into one for your own machines.

```bash
cp -R inventory/sample inventory/my-clusters
```

3. Edit `inventory/my-clusters/hosts.ini` and add your own list of machine IPs or FQDNs.  
(Remember that you only need one machine per cluster, so for most people looking to add MetalLB to their cluster you should only need one IP or FQDN)

```ini
[master]
192.168.1.99
192.168.1.199
192.168.1.225
```

4. Check and possibly update `inventory/my-clusters/group_vars/all.yml` with the correct version and configuration of MetalLB you are trying to deploy.

```yaml
---
metallb_version: v0.13.5
ip_address_pool: 192.168.1.240-192.168.1.250
address_pool_name: default
advertisement_name: default
```

5. Run the playbook from the root of the repository

```bash
ansible-playbook deploy.yml -i inventory/my-clusters/hosts.ini
```

if your machines use a diffent username than the host machine then you can pass that in using `-u`.

```bash
ansible-playbook deploy.yml -i inventory/my-clusters/hosts.ini -u pi
```