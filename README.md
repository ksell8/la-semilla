# La Semilla

La Semilla, or the seed, is a node for hosting content.  It works in tandem with a mobile application that searches for the BLE beacons las semillas emit and uses the information emitted to connect to the hosts.

## Requirements

Debian based OS for now.  Meant to run on a minimal chipset.  

## Setup

### 1. Pull embedded apps

```bash
git submodule update --init --recursive
```

### 2. Configure variables

Edit `vars/local_vars.yml` to override defaults:

```yaml
hotspot_ssid: your-network-name
hotspot_channel: 7
beacon_name: your-beacon-name
hostname: your-hostname
uuid: your-beacon-uuid
beacon_major: 1
beacon_minor: 1
```

### 3. Configure secrets

Edit `vars/vault.yml` with your passwords, then encrypt it:

```bash
ansible-vault encrypt vars/vault.yml
```

```yaml
hotspot_password: your-wifi-password
ansible_become_password: your-sudo-password
```

## Usage

```bash
# Run full setup
ansible-playbook -i inventory.ini site.yml --ask-vault-pass

# Run only networking
ansible-playbook -i inventory.ini site.yml --tags networking --ask-vault-pass

# Run only bluetooth
ansible-playbook -i inventory.ini site.yml --tags bluetooth --ask-vault-pass
```
