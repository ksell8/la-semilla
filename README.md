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
hotspot_password: the-password-people-will-connect-to-your-network-with
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

## AI Agent

By default ollama is installed with the gemma:2b model.  You can turn this off by setting `enable_ai: false`.

There's a [web ui](https://github.com/open-webui)

### Known Bugs

1) OLLAMA_BASE_URL override isn't working.  According to the docs, when running in a docker it should default to using the docker host interface instead of localhost, but it's not.  Go to Settings > Connection and change localhost to host.docker.internal.
2) Go to Settings > Model and turn off built in tools for gemma:2b otherwise your prompts will error with `Error processing chat payload: registry.ollama.ai/library/gemma:2b does not support tools`.

