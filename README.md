# 🔐 Ansible CIS Benchmark Modules

Ce projet propose une collection de **modules Ansible** dédiés à l’**audit** et à la **remédiation** des systèmes Linux conformément aux recommandations du **CIS Benchmark**, en particulier pour Debian 12.

---

## 🎯 Objectif

Automatiser, valider et sécuriser la configuration d’un système en s’appuyant sur les standards **CIS (Center for Internet Security)** à l’aide de **rôles modulaires Ansible**.

---

## 📦 Fonctionnalités

- ✅ **Audit par rôle** : chaque contrôle CIS est représenté par un rôle Ansible réutilisable.
- 🔁 **Remédiation conditionnelle** : certaines tâches appliquent automatiquement les correctifs si l’audit échoue.
- 📊 **Rapports clairs** : utilisation de `debug:` pour afficher l’état de conformité par tâche.
- ⚙️ **Support multi-partitions** : audit des partitions `/tmp`, `/var`, `/home`, etc., avec validation des options `fstab`.
- 📎 **Systemd-aware** : vérifie l’état des unités `*.mount` pour les partitions critiques.
- 🧩 **Personnalisable** : chaque rôle accepte des variables comme `required_mount_options`, `partition_path`, `systemd_unit`, etc.

---

## 📁 Structure du projet


---

## 🚀 Exemples d’utilisation

roles/
  ├── Filesystem_Kernel_Modules/ # Audit/remédiation de modules noyau (ex: cramfs, freevxfs)
  ├── Filesystem_Partitions/ # Audit des partitions, montages et options fstab

### 🔍 Audit de partitions système

```yaml
- name: Audit de plusieurs partitions
  hosts: all
  become: true
  roles:
    - role: Filesystem_Partitions
      vars:
        partition_config:
          - path: /tmp # 1.1.2.1
            systemd_unit: tmp.mount
            required_mount_options: ["noexec", "nosuid", "nodev"]
          - path: /dev/shm # 1.1.2.2
            systemd_unit: none # Aucun systemd pour shm, on laisse a none
            required_mount_options: ["noexec", "nosuid", "nodev"]
          - path: /home # 1.1.2.3
            systemd_unit: home.mount
            required_mount_options: ["nosuid", "nodev"]
          - path: /var # 1.1.2.4
            systemd_unit: var.mount
            required_mount_options: ["nosuid", "nodev"]
          - path: /var/tmp # 1.1.2.5
            systemd_unit: var-tmp.mount
            required_mount_options: ["noexec", "nosuid", "nodev"]
          - path: /var/log # 1.1.2.6
            systemd_unit: var-log.mount
            required_mount_options: ["noexec", "nosuid", "nodev"]
          - path: /var/log/audit # 1.1.2.7
            systemd_unit: var-log-audit.mount
            required_mount_options: ["noexec", "nosuid", "nodev"]


