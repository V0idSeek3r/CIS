# 🔐 Ansible CIS Benchmark Modules

Ce projet propose une collection de **modules Ansible** dédiés à l’**audit** et à la **remédiation** des systèmes Linux conformément aux recommandations du **CIS Benchmark**, en particulier pour Debian 12.

---

## 🎯 Objectif

Automatiser, valider et sécuriser la configuration d’un système en s’appuyant sur les standards **CIS (Center for Internet Security)** à l’aide de **rôles modulaires Ansible**.

---

## 📁 Structure du projet
```
Ansible/
├── playbook.yml
└── roles/
    ├── Filesystem_Kernel_Modules/
    │   ├── tasks/
    │   │   ├── audit.yml
    │   │   ├── fix.yml
    │   │   └── main.yml
    │   └── vars/
    │       └── main.yml
    └── Filesystem_Partitions/
        ├── tasks/
        │   ├── check_partition.yml
        │   └── main.yml
        └── vars/
            └── main.yml
```

---
## 🚀 Fonctionnalités

### 🔍 Audit des modules noyau (`Filesystem_Kernel_Modules`) CIS 1.1.1 Configure Filesystem Kernel Modules
- Vérifie si des modules vulnérables ou inutiles sont :
  - chargés (`lsmod`)
  - blacklistés (`modprobe`)
  - désactivés via `install /bin/false`
- Génère un rapport d’audit et applique les remédiations nécessaires (déchargement, blacklist, verrouillage via modprobe).

### 🧱 Vérification des partitions (`Filesystem_Partitions`) CIS 1.1.2 Configure Filesystem Partitions
- Vérifie que les partitions sensibles comme `/tmp`, `/dev/shm`, `/home`, etc. sont montées avec les options `nodev`, `nosuid`, `noexec` selon la configuration CIS.
- Signale les manques via une logique conditionnelle basée sur `/etc/fstab`.

## ⚙️ Prérequis

- Ansible 2.10+
- Système Linux supportant `lsmod`, `modprobe`, `grep`, `systemctl`
- Accès `root` (ou `become: true`) pour les remédiations

## 📦 Installation

Clonez ce dépôt :

```bash
git clone https://github.com/votre-utilisateur/ansible-filesystem-hardening.git
cd ansible-filesystem-hardening
```

## 🛠️ Utilisation

Exécutez le playbook principal :

```bash
ansible-playbook -i inventory.yml Ansible/playbook.yml
```

Vous pouvez aussi surcharger certaines variables :

```bash
ansible-playbook Ansible/playbook.yml -e "Filesystem_Kernel_Modules_On=true Filesystem_Partitions_On=false"
```

## 🔧 Variables personnalisables

Ces variables sont définies dans `vars/main.yml` pour chaque rôle.

### `Filesystem_List`

Liste des modules à auditer :

```yaml
Filesystem_List:
  - cramfs
  - udf
  - usb-storage
```

### `partition_config`

Liste des points de montage à valider et leurs options requises :

```yaml
partition_config:
  - path: /tmp
    required_mount_options: ["noexec", "nosuid", "nodev"]
```

## 📄 Sortie attendue

Exemples de résumé :

```
=== 🔍 Audit Modules ===
✅ Le module cramfs est conforme : non chargé, non chargeable et blacklisté.
❌ Le module udf est chargé ou mal configuré.

=== 🛠️ Remédiations ===
🛡️ Le module udf a été déchargé, rendu non chargeable et blacklisté.
```

## 📝 Licence

Ce projet est distribué sous licence MIT.

## 🤝 Contributions

Les PRs et suggestions sont les bienvenues ! Pour contribuer :
1. Forkez le dépôt
2. Créez une branche `feature/xxx`
3. Envoyez une Pull Request

## 👤 Auteur

Développé par mra7
📧 :o
