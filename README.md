# Server Hardening With Ansible
A collection of Ansible roles to harden Linux servers: SSH, firewall, auto updates, fail2ban, auditd, AIDE, kernel hardening, SELinux/AppArmor, and more.

## TLDR / Summary
- Supports Debian/Ubuntu and RedHat family (Rocky/Fedora) via Molecule.
- One playbook to run hardening + optional audit; Lynis report can be saved locally.
- SSH is moved to a custom port (default 2222) and password auth is disabled.
- Firewalls: UFW (Debian/Ubuntu) or firewalld (RedHat).
- Auto updates: unattended-upgrades (Debian/Ubuntu) or dnf-automatic (RedHat).
- Fail2ban, auditd, AIDE, kernel sysctl, module blacklists, systemd overrides.
- Local testing via Molecule (Docker) and optional Vagrant VM.

## Supported Distributions
- Debian 12 / 13
- Ubuntu 24.04
- Rocky Linux 10
- Fedora 43

## Requirements
- Control node: Python 3.10+, Ansible 2.15+
- Targets: SSH access with privilege escalation (become)
- Testing: Docker + Molecule / Vagrant + libvirt

## Installation
- Install Ansible:
```bash
sudo apt-get update && sudo apt-get install -y python3-pip
pip3 install ansible
```
- Optional: install Molecule and plugins:
```bash
pip3 install molecule molecule-plugins[docker] docker
```
- Optional: install Vagrant + libvirt provider (distro-specific)

## Project Structure
- Playbook: playbook.yaml
- Inventory: inventory.ini
- Roles:
  - security: Main hardening tasks across SSH, firewall, updates, fail2ban, auditd, AIDE, kernel, systemd, SELinux/AppArmor
  - audit: Run Lynis and collect report
  - debug: Basic connectivity and facts
- Testing: molecule/default/*
- CI: .github/workflows/*

## How To Use
1. Configure inventory.ini with your host, user, and auth (password or SSH key).
2. Run the playbook:
```bash
ansible-playbook playbook.yaml
```
3. After SSH hardening (port change + password disabled), reconnect using the new port (default 2222):
```bash
ansible-playbook playbook.yaml -e "ansible_port=2222"
```
4. Optional: run only on a group or host:
```bash
ansible-playbook playbook.yaml -l my-host-group
```

## Configuration
- Role defaults:
  - security/defaults/main.yml
  - audit/defaults/main.yml
- Distro-specific vars:
  - security/vars/Debian.yaml
  - security/vars/RedHat.yaml

### Common Variables To Adjust
- ssh_new_port: default 2222

## Roles Overview (High Level)
- security:
  - SSH hardening config, custom port, disable password
  - Firewall: ufw or firewalld rules aligned to ssh port
  - Auto updates: unattended-upgrades or dnf-automatic
  - Fail2ban service and base jails
  - SELinux/AppArmor where applicable
  - auditd rules and service
  - AIDE install and initialize db
  - kernel sysctl + module blacklist
  - systemd hardening (selected services)
- audit:
  - run lynis and store report to result/lynis.audit
- debug:
  - gather facts, connectivity checks

## Testing With Molecule
- Run full test:
```bash
molecule test
```
- Choose distro:
```bash
MOLECULE_DISTRO=rockylinux10 molecule test
```
- Molecule install docs: https://docs.ansible.com/projects/molecule/installation/#pip
- If you add a new role, follow the pattern in molecule/default/converge.yml

## Using Vagrant (Optional)
```bash
vagrant up --provider=libvirt
vagrant destroy -f
```
- Update inventory.ini with the VM ip/user/key or update the password, then run the playbook

## Audit Tools (Optional)
- lynis:
```bash
sudo apt install -y lynis
sudo lynis audit system
```

- Other vps audit:
```bash
curl -O https://raw.githubusercontent.com/vernu/vps-audit/main/vps-audit.sh && chmod +x vps-audit.sh && sudo ./vps-audit.sh
```

## Troubleshooting
- Cannot reconnect after ssh port change:
  - use `-e "ansible_port=<your-port>"` or set new_ssh_port in vars
- Molecule fails:
  - ensure docker is running and python deps are installed
- SElinux changes:
  - reboots may be required after state changes
- VirtualBox Error: VERR_VMX_IN_VMX_ROOT_MODE (Ubuntu/Linux)
  If you encounter this error when running `vagrant up`, it means the KVM kernel module is conflicting with VirtualBox.
  - Temporarily unload the KVM modules before running Vagrant:
```bash
# For Intel Processors
sudo rmmod kvm_intel
sudo rmmod kvm

# For AMD Processors
sudo rmmod kvm_amd
sudo rmmod kvm
```

## Contribution
- Create a branch and open a pull request to main
```bash
git checkout -b your-branch-name
# make changes
git add .
git commit -m "describe changes"
git push origin your-branch-name
```
- Keep branches synced with main to avoid conflicts
- Include molecule tests for new roles or changes
- Use `molecule-idempotence-notest` and `molecule-notest` when needed
