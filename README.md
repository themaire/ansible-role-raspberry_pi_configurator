
# ansible-role-raspberry_pi_configurator

## For Raspberry Pi Bookworm

**Ce fichier est bilingue : français puis anglais.**  
**This file is bilingual: French then English.**

---

# 🇫🇷 Français

Rôle Ansible pour configurer rapidement et facilement un Raspberry Pi sous Raspberry Pi OS.

## Description

Ce rôle permet d’automatiser la configuration initiale d’un Raspberry Pi : installation de paquets, configuration système, sécurité SSH, gestion des utilisateurs, montage de disques, etc.

## Prérequis

- Raspberry Pi OS installé et accessible en SSH
- Ansible ≥ 2.9 sur la machine de contrôle
- Clé SSH pour l’utilisateur cible

## Astuces et bonnes pratiques

### 1. Préparation de la carte SD

Avant de flasher, définissez un mot de passe personnalisé pour le Pi, configurez le Wi-Fi (si besoin) et activez SSH via Raspberry Pi Imager.

### 2. Génération de la clé SSH

Sur la machine de contrôle, créez une clé SSH pour l’utilisateur cible :

```bash
ssh-keygen -f ~/.ssh/my-user.key
```

Remplacez le fichier `files/my-user.key.pub` par votre propre clé publique, nommée selon l’utilisateur cible.

### 3. Utilisation du Vault Ansible

Le fichier vault est à placer dans `group_vars/new/vault.yml` (ou selon votre organisation). Il permet de stocker les mots de passe chiffrés, notamment celui de l’utilisateur admin créé par le rôle.

**Mot de passe par défaut** : Le mot de passe par défaut de l’utilisateur admin est `raspberry` (à modifier dans le vault !).

Commandes utiles :

```bash
ansible-vault rekey ./group_vars/new/vault.yml
ansible-vault edit ./group_vars/new/vault.yml
```

Il est recommandé de déclarer d’abord un seul utilisateur admin, puis d’ajouter d’autres utilisateurs si besoin.

### 4. Adaptation de l’inventaire

Modifiez le fichier d’inventaire pour lister vos Raspberry Pi. Plusieurs exemples sont fournis.

### 5. Exécution du playbook

1. Connectez-vous une première fois en SSH sur le Pi fraîchement installé, puis déconnectez-vous.
2. Lancez le playbook comme indiqué ci-dessous.
3. Profitez !

## Variables du rôle

Voir le fichier `defaults/main.yml` pour la liste complète et les valeurs par défaut. Exemples :

```yaml
locale: 'fr_FR.UTF-8'
timezone: 'Europe/Paris'
packages:
  - git
  - tmux
  - vim
  - nmon
  - neofetch
log2ram_size: '128'
gpu_mem: '16'
disable_hdmi: true
disable_ipv6_interfaces:
  - wlan0
  - eth0
```

## Exemple d’utilisation

```yaml
- name: Configuration d’un Raspberry Pi
  hosts: all
  become: true
  roles:
    - role: raspberry_pi_configurator
```

## Fonctionnalités principales

- Mise à jour et upgrade du système
- Installation de paquets et de Log2Ram
- Création d’utilisateurs/admins
- Changement du hostname
- Configuration mémoire GPU
- Changement de timezone et locale
- Désactivation HDMI et IPv6
- Sécurisation SSH (port, root, mot de passe, clé)
- Configuration Samba (optionnel)

## Structure du projet

```text
defaults/
files/
handlers/
meta/
tasks/
templates/
tests/
```

## Crédits


Inspiré par [zjael/raspberry_pi](https://galaxy.ansible.com/zjael/raspberry_pi).

---

# 🇬🇧 English

Ansible role to quickly and easily configure a Raspberry Pi running Raspberry Pi OS.

## Description

This role automates the initial configuration of a Raspberry Pi: package installation, system configuration, SSH security, user management, disk mounting, and more.

## Requirements

- Raspberry Pi OS installed and accessible via SSH
- Ansible ≥ 2.9 on the control machine
- SSH key for the target user

## Tips and Best Practices

### 1. SD Card Preparation

Before flashing, set a custom password for the Pi, configure Wi-Fi (if needed), and enable SSH using Raspberry Pi Imager.

### 2. SSH Key Generation

On your control machine, create an SSH key for the target user:

```bash
ssh-keygen -f ~/.ssh/my-user.key
```

Replace the file `files/my-user.key.pub` with your own public key, named after the target user.

### 3. Using Ansible Vault

The vault file should be placed in `group_vars/new/vault.yml` (or as you prefer). It stores encrypted passwords, especially for the admin user created by the role.

**Default password**: The default password for the admin user is `raspberry` (change it in the vault!).

Useful commands:

```bash
ansible-vault rekey ./group_vars/new/vault.yml
ansible-vault edit ./group_vars/new/vault.yml
```

It is recommended to declare only one admin user at first, then add more users if needed.

### 4. Inventory Adaptation

Edit your inventory file to list your Raspberry Pis. Several examples are provided.

### 5. Playbook Execution

1. Connect once via SSH to the freshly installed Pi, then disconnect.
2. Run the playbook as shown below.
3. Enjoy!

## Role Variables

See `defaults/main.yml` for the full list and default values. Examples:

```yaml
locale: 'fr_FR.UTF-8'
timezone: 'Europe/Paris'
packages:
  - git
  - tmux
  - vim
  - nmon
  - neofetch
log2ram_size: '128'
gpu_mem: '16'
disable_hdmi: true
disable_ipv6_interfaces:
  - wlan0
  - eth0
```

## Example Usage

```yaml
- name: Raspberry Pi configuration
  hosts: all
  become: true
  roles:
    - role: raspberry_pi_configurator
```

## Main Features

- System update and upgrade
- Package and Log2Ram installation
- Admin/user creation
- Hostname change
- GPU memory configuration
- Timezone and locale change
- HDMI and IPv6 disabling
- SSH hardening (port, root, password, key)
- Samba configuration (optional)

## Project Structure

```text
defaults/
files/
handlers/
meta/
tasks/
templates/
tests/
```

## Credits

Inspired by [zjael/raspberry_pi](https://galaxy.ansible.com/zjael/raspberry_pi).
