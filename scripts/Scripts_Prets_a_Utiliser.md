# 🚀 Scripts Prêts à Copier-Coller

## PowerShell - Vérification Système

```powershell
# === PHASE 0: VÉRIFICATION COMPLÈTE ===

Write-Host "🔍 Vérification de l'environnement..." -ForegroundColor Cyan

# Git
Write-Host "`n1. Git:" -ForegroundColor Yellow
git --version
if ($LASTEXITCODE -ne 0) {
    Write-Host "   ❌ Git non installé" -ForegroundColor Red
    Write-Host "   → Installer avec: winget install Git.Git" -ForegroundColor Green
} else {
    Write-Host "   ✅ Git installé" -ForegroundColor Green
}

# VirtualBox
Write-Host "`n2. VirtualBox:" -ForegroundColor Yellow
vboxmanage --version
if ($LASTEXITCODE -ne 0) {
    Write-Host "   ❌ VirtualBox non installé" -ForegroundColor Red
    Write-Host "   → Télécharger: https://www.virtualbox.org/wiki/Downloads" -ForegroundColor Green
} else {
    Write-Host "   ✅ VirtualBox installé" -ForegroundColor Green
}

# Vagrant
Write-Host "`n3. Vagrant:" -ForegroundColor Yellow
vagrant --version
if ($LASTEXITCODE -ne 0) {
    Write-Host "   ❌ Vagrant non installé" -ForegroundColor Red
    Write-Host "   → Installer avec: winget install Hashicorp.Vagrant" -ForegroundColor Green
} else {
    Write-Host "   ✅ Vagrant installé" -ForegroundColor Green
}

# Docker (optionnel)
Write-Host "`n4. Docker Desktop (optionnel):" -ForegroundColor Yellow
docker --version
if ($LASTEXITCODE -ne 0) {
    Write-Host "   ⚠️  Docker non installé (pas obligatoire)" -ForegroundColor Yellow
} else {
    Write-Host "   ✅ Docker installé" -ForegroundColor Green
}

# Virtualisation
Write-Host "`n5. Virtualisation:" -ForegroundColor Yellow
$hyperv = systeminfo | Select-String "Hyper-V"
if ($hyperv) {
    Write-Host "   ✅ Virtualisation détectée" -ForegroundColor Green
} else {
    Write-Host "   ⚠️  Vérifier dans BIOS que VT-x/AMD-V est activé" -ForegroundColor Yellow
}

Write-Host "`n✅ Vérification terminée!" -ForegroundColor Cyan
```

## PowerShell - Setup Complet du Projet

```powershell
# === PHASE 1: CRÉATION PROJET ===

# Configure Git
Write-Host "🔧 Configuration Git..." -ForegroundColor Cyan
git config --global user.name "Ton Nom"
git config --global user.email "ton.email@student.fr"

# Crée la structure
Write-Host "📁 Création de la structure..." -ForegroundColor Cyan
$projectPath = "C:\Dev\CTF-Platform"
New-Item -ItemType Directory -Force -Path $projectPath
Set-Location $projectPath

# Clone depuis GitHub (APRÈS avoir créé le repo)
Write-Host "📥 Clone du repository..." -ForegroundColor Cyan
# Remplace USERNAME par ton username GitHub
git clone https://github.com/USERNAME/ctf-platform-iaac.git
Set-Location ctf-platform-iaac

Write-Host "✅ Projet initialisé!" -ForegroundColor Green
```

## PowerShell - Création Structure Complète

```powershell
# === STRUCTURE DES DOSSIERS ===

Write-Host "📁 Création de l'arborescence complète..." -ForegroundColor Cyan

# Dossiers principaux
$folders = @(
    "ansible\playbooks",
    "ansible\roles",
    "ansible\templates",
    "ansible\vars",
    "challenges\web",
    "challenges\pwn",
    "challenges\crypto",
    "challenges\reverse",
    "challenges\forensics",
    "challenges\misc",
    "docs",
    "scripts",
    ".github\workflows",
    ".github\ISSUE_TEMPLATE"
)

foreach ($folder in $folders) {
    New-Item -ItemType Directory -Force -Path $folder | Out-Null
    Write-Host "   ✓ $folder" -ForegroundColor Green
}

# Fichiers à créer
$files = @(
    "Vagrantfile",
    "ansible\inventory",
    "ansible\ansible.cfg",
    ".env.example",
    ".gitattributes",
    ".dockerignore"
)

foreach ($file in $files) {
    New-Item -ItemType File -Force -Path $file | Out-Null
    Write-Host "   ✓ $file" -ForegroundColor Green
}

Write-Host "`n✅ Structure créée avec succès!" -ForegroundColor Cyan
```

## PowerShell - Lancement VM

```powershell
# === PHASE 5: DÉMARRAGE VM ===

Write-Host "🚀 Lancement de Vagrant..." -ForegroundColor Cyan

# Vérifie qu'on est dans le bon dossier
if (-Not (Test-Path "Vagrantfile")) {
    Write-Host "❌ Vagrantfile introuvable! Vérifie ton dossier." -ForegroundColor Red
    exit 1
}

# Lance Vagrant
Write-Host "📦 Téléchargement et provisioning (10-15 min)..." -ForegroundColor Yellow
vagrant up

# Vérifie le statut
$status = vagrant status
if ($status -match "running") {
    Write-Host "`n✅ VM démarrée avec succès!" -ForegroundColor Green
    Write-Host "`n📝 Prochaines étapes:" -ForegroundColor Cyan
    Write-Host "   1. vagrant ssh" -ForegroundColor White
    Write-Host "   2. cd /vagrant/ansible" -ForegroundColor White
    Write-Host "   3. ansible-playbook playbooks/main.yml" -ForegroundColor White
} else {
    Write-Host "`n❌ Erreur lors du démarrage" -ForegroundColor Red
    Write-Host "Regarde les logs avec: vagrant up --debug" -ForegroundColor Yellow
}
```

## Bash (dans la VM) - Premier Déploiement

```bash
#!/bin/bash
# === DANS LA VM (après vagrant ssh) ===

echo "🚀 Déploiement CTF Platform..."

# Va dans le dossier
cd /vagrant/ansible

# Vérifie que .env existe
if [ ! -f "/vagrant/.env" ]; then
    echo "⚠️  Fichier .env manquant, création depuis template..."
    cp /vagrant/.env.example /vagrant/.env
    
    # Génère une SECRET_KEY
    SECRET=$(openssl rand -base64 32)
    sed -i "s/generate_with_openssl_rand_base64_32/$SECRET/" /vagrant/.env
    
    echo "✅ .env créé avec SECRET_KEY générée"
    echo "⚠️  N'oublie pas d'éditer les autres valeurs dans /vagrant/.env"
fi

# Vérifie Ansible
ansible --version

# Test de connexion
echo "🔍 Test de connexion Ansible..."
ansible ctf_servers -m ping

# Lance le playbook
echo "📦 Lancement du playbook (5-10 min)..."
ansible-playbook playbooks/main.yml

# Vérifie Docker
echo "🐳 Vérification des containers..."
docker ps

echo ""
echo "✅ Déploiement terminé!"
echo "🌐 CTFd accessible sur: http://192.168.56.10"
```

## Script Bash - Vérification Post-Installation

```bash
#!/bin/bash
# === VÉRIFICATION COMPLÈTE ===

echo "🔍 Vérification de l'installation..."

# Fonction de vérification
check() {
    if [ $? -eq 0 ]; then
        echo "   ✅ $1"
    else
        echo "   ❌ $1"
        return 1
    fi
}

# Docker
echo "1. Docker:"
docker --version > /dev/null 2>&1
check "Docker installé"

docker ps > /dev/null 2>&1
check "Docker daemon actif"

# Containers CTFd
echo ""
echo "2. Containers:"
docker ps | grep -q ctfd
check "CTFd container running"

docker ps | grep -q ctfd_db
check "MariaDB container running"

docker ps | grep -q ctfd_cache
check "Redis container running"

# Réseau
echo ""
echo "3. Réseau:"
curl -s http://localhost:80 > /dev/null
check "CTFd répond sur port 80"

# Ansible
echo ""
echo "4. Ansible:"
ansible --version > /dev/null 2>&1
check "Ansible installé"

# Espace disque
echo ""
echo "5. Ressources:"
df -h / | tail -1 | awk '{print "   Disque: " $5 " utilisé"}'
free -h | grep Mem | awk '{print "   RAM: " $3 " / " $2}'

echo ""
echo "✅ Vérification terminée!"
```

## Git - Workflow pour Ajouter un Challenge

```bash
#!/bin/bash
# === WORKFLOW CHALLENGE ===

CHALLENGE_NAME=$1
CATEGORY=$2

if [ -z "$CHALLENGE_NAME" ] || [ -z "$CATEGORY" ]; then
    echo "Usage: $0 <challenge-name> <category>"
    echo "Catégories: web, pwn, crypto, reverse, forensics, misc"
    exit 1
fi

echo "🚀 Création du challenge: $CHALLENGE_NAME"

# Crée la structure
CHALLENGE_DIR="challenges/$CATEGORY/$CHALLENGE_NAME"
mkdir -p $CHALLENGE_DIR

# Crée Dockerfile template
cat > $CHALLENGE_DIR/Dockerfile << 'EOF'
FROM ubuntu:22.04

# Install dependencies
RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

# Copy challenge files
WORKDIR /app
COPY . /app

# Set flag (will be replaced by Whale)
ENV FLAG="CTF{default_flag}"

# Expose port
EXPOSE 8080

# Run
CMD ["python3", "app.py"]
EOF

# Crée metadata.json
cat > $CHALLENGE_DIR/metadata.json << EOF
{
  "name": "$CHALLENGE_NAME",
  "category": "$CATEGORY",
  "value": 100,
  "description": "Description du challenge",
  "flags": ["CTF{flag_here}"],
  "docker_image": "ctf/$CATEGORY-$CHALLENGE_NAME",
  "port": 8080
}
EOF

# Crée app.py template
cat > $CHALLENGE_DIR/app.py << 'EOF'
from flask import Flask, render_template_string
import os

app = Flask(__name__)
FLAG = os.environ.get('FLAG', 'CTF{test_flag}')

@app.route('/')
def index():
    return render_template_string('''
        <h1>Challenge</h1>
        <p>Trouve le flag!</p>
    ''')

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080)
EOF

# Crée README
cat > $CHALLENGE_DIR/README.md << EOF
# $CHALLENGE_NAME

## Description
TODO: Décris le challenge

## Solution
TODO: Explique la solution (ce fichier ne sera pas public)

## Flag
\`CTF{flag_here}\`

## Difficulté
Easy / Medium / Hard

## Points
100
EOF

echo "✅ Challenge créé dans: $CHALLENGE_DIR"
echo ""
echo "📝 Prochaines étapes:"
echo "   1. Édite $CHALLENGE_DIR/app.py avec ton code"
echo "   2. Test local: docker build -t test $CHALLENGE_DIR"
echo "   3. git add $CHALLENGE_DIR"
echo "   4. git commit -m 'feat: Add $CATEGORY challenge $CHALLENGE_NAME'"
echo "   5. git push origin main"
```

## PowerShell - Update Rapide

```powershell
# === UPDATE & REBUILD RAPIDE ===

Write-Host "🔄 Update de la plateforme..." -ForegroundColor Cyan

# Pull les derniers changements
git pull origin main

# Reload Vagrant
Write-Host "📦 Reload de la VM..." -ForegroundColor Yellow
vagrant reload --provision

# SSH et re-déploie
Write-Host "🚀 Re-déploiement..." -ForegroundColor Yellow
vagrant ssh -c "cd /vagrant/ansible && ansible-playbook playbooks/main.yml"

Write-Host "✅ Update terminée!" -ForegroundColor Green
```

## Commandes Utiles à Connaître

```bash
# === VAGRANT ===
vagrant up              # Démarre la VM
vagrant halt            # Éteint la VM
vagrant reload          # Redémarre la VM
vagrant destroy         # Détruit la VM
vagrant ssh             # Se connecte à la VM
vagrant status          # Statut de la VM
vagrant provision       # Re-exécute le provisioning

# === ANSIBLE (dans la VM) ===
ansible-playbook playbooks/main.yml           # Lance le playbook
ansible-playbook playbooks/main.yml --check   # Dry-run
ansible-playbook playbooks/main.yml -v        # Verbose
ansible ctf_servers -m ping                   # Test connexion

# === DOCKER (dans la VM) ===
docker ps                       # Liste containers actifs
docker ps -a                    # Liste tous les containers
docker logs <container>         # Logs d'un container
docker exec -it <container> bash # Se connecte à un container
docker restart <container>      # Redémarre un container
docker-compose down             # Stoppe tous les services
docker-compose up -d            # Démarre en background

# === GIT ===
git status                      # État du repo
git add .                       # Ajoute tous les fichiers
git commit -m "message"         # Commit
git push origin main            # Push vers GitHub
git pull origin main            # Pull depuis GitHub
git log --oneline               # Historique concis
git branch                      # Liste branches
```

## Troubleshooting Rapide

```bash
# === VM ne démarre pas ===
# 1. Vérifie VirtualBox
vboxmanage list vms

# 2. Détruit et recrée
vagrant destroy -f
vagrant up

# === Containers ne démarrent pas ===
# Dans la VM:
docker system prune -a          # Nettoie tout
systemctl restart docker         # Redémarre Docker
docker-compose up -d --force-recreate

# === Ansible échoue ===
# Vérifie syntaxe
ansible-playbook playbooks/main.yml --syntax-check

# Mode debug
ansible-playbook playbooks/main.yml -vvv

# === Port déjà utilisé ===
# Trouve le process
netstat -ano | findstr :8080    # Windows
sudo lsof -i :8080              # Linux (dans VM)

# Tue le process
taskkill /PID <pid> /F          # Windows
sudo kill -9 <pid>              # Linux
```

---

**💡 Astuce:** Sauvegarde ce fichier dans `scripts/quick-commands.md` dans ton repo!
