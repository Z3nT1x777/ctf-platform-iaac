# 🚩 CTF Platform - Infrastructure as Code

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Vagrant](https://img.shields.io/badge/Vagrant-2.4%2B-blue)](https://www.vagrantup.com/)
[![Ansible](https://img.shields.io/badge/Ansible-2.14%2B-red)](https://www.ansible.com/)

## 📋 Projet
Plateforme CTF self-hosted complètement automatisée avec isolation Docker par challenge.  
**Projet M2 Cybersécurité** - Infrastructure as Code & DevSecOps.

### ✨ Caractéristiques
- 🏗️ **100% Infrastructure as Code** : Vagrant + Ansible
- 🐳 **Isolation Docker** : Chaque challenge dans son container
- 🔄 **CI/CD Automatisé** : Pipeline GitLab intégré
- 📊 **Monitoring** : Prometheus + Grafana
- 🔐 **Sécurisé** : Isolation réseau, secrets management

### 🛠️ Stack Technique
| Composant     | Technologie          |
|---------------|----------------------|
| Provisioning  | Vagrant + VirtualBox |
| Configuration | Ansible              |
| Runtime       | Docker + Compose     |
| CTF Platform  | CTFd + Whale plugin  |
| CI/CD         | GitLab CE            |
| Monitoring    | Prometheus + Grafana |

## 🚀 Quick Start
```bash
# Clone le repo
git clone https://github.com/USERNAME/ctf-platform-iaac.git
cd ctf-platform-iaac

# Configure l'environnement
cp .env.example .env
# Édite .env avec tes valeurs

# Lance la VM
vagrant up

# Attends 10-15 min, puis accède à :
# CTFd: http://192.168.56.10
# GitLab: http://192.168.56.10:8080
# Grafana: http://192.168.56.10:3000