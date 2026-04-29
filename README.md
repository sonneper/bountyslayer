<h1 align="center">💰 Bounty Slayer AI</h1>
<p align="center">
  <img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="Licence MIT">
  <img src="https://img.shields.io/badge/language-bash-4EAA25.svg" alt="Bash">
  <img src="https://img.shields.io/badge/platform-linux%20%7C%20macOS-lightgrey" alt="Plateforme">
  <img src="https://img.shields.io/badge/AI-Reinforcement%20Learning-brightgreen" alt="IA">
</p>

<h3 align="center">Autonomous Bug Bounty & Pentesting Engine with Reinforcement Learning</h3>
---
## 🧠 Vision
**Bounty Slayer AI** est le couteau suisse des chasseurs de primes et des équipes Red Team.  
Il automatise l’intégralité d’un test d’intrusion moderne – de la découverte des sous-domaines à la génération de **Proof-of-Concepts (PoC) exploitables** – en s’appuyant sur un **apprentissage par renforcement** qui augmente vos gains HackerOne à chaque scan.

Inspiré par :
- La méthodologie **TBHM** de Jason Haddix  
- Les labs **TryHackMe / HackTheBox**  
- Les matrices **MITRE ATT&CK** et le **Top 10 OWASP**  
- La stack **ProjectDiscovery** et les meilleurs outils open-source GitHub

Le script s’améliore **automatiquement** après chaque trouvaille : plus vous l’utilisez, plus il devient rentable.

---

## 📐 Architecture


Un moteur de **Reinforcement Learning** (Q‑table) ajuste les priorités en fonction du succès des PoC, garantissant que les vulnérabilités les plus lucratives sont testées en premier.

---

## ⚡ Fonctionnalités clés

- 🔍 **Recon automatique** : collecte de sous‑domaines (passif + actif), résolution DNS, crawl profond avec `katana`
- 📊 **Extraction de features** : statut HTTP, technologies, titres, CDN, IPs
- 🧠 **Modèle pré‑entraîné** : `Nuclei` avec ses milliers de templates (CVEs, OWASP, MITRE)
- 🎲 **Probabilités** : score composite CVSS × poids RL × exploration ε‑greedy (découvre de nouveaux vecteurs)
- 🎯 **Priorisation intelligente** : les vulnérabilités critiques et bien rémunérées sont attaquées en premier
- 💣 **Exploitation avec PoC** :
  - `sqlmap` pour SQLi
  - `dalfox` pour XSS
  - `subjack` pour les subdomain takeovers
  - `curl` + tests temporels pour RCE et command injection
  - Callback pour SSRF
- 📝 **Rapport Markdown complet** avec commandes PoC et extraits de résultats
- 🔁 **Apprentissage continu** : les Q‑values sont sauvegardées dans `~/.bountyslayer_rl/q_model.csv` et réutilisées à chaque scan
- 🛡️ **Tolérance aux pannes** : gestionnaire d’erreurs intégré (`trap`), poursuite du scan même en cas de segfault
- 📦 **Installation automatique** des dépendances manquantes (Go, apt, brew)

---

## 🧰 Outils intégrés

| Phase               | Outils                                                                                             |
|---------------------|----------------------------------------------------------------------------------------------------|
| Reconnaissance      | `subfinder`, `amass`, `assetfinder`, `crt.sh` (curl), `httpx`, `katana`                           |
| Scan de vulnérabilités | `nuclei` (ProjectDiscovery) + templates communautaires                                          |
| Fuzzing / Exploit   | `sqlmap`, `dalfox`, `commix`, `subjack`, `curl`, `whatweb`, `testssl.sh`                          |
| Utilitaires         | `jq`, `python3`, `git`, `go`                                                                      |

Tous les outils sont **libres** et **open source**.

---

## 🧠 Reinforcement Learning (RL)

Le fichier `~/.bountyslayer_rl/q_model.csv` stocke une table Q simplifiée :

| vuln_type | total_attempts | successes | weight |
|-----------|----------------|-----------|--------|
| sqli      | 12             | 8         | 1.4    |
| rce       | 5              | 4         | 2.0    |
| xss       | 7              | 1         | 0.8    |

- **Récompense** : chaque PoC confirmé augmente le poids du type de vulnérabilité  
- **Pénalité** : un faux positif diminue légèrement le poids  
- **Exploration** (ε = 10%) : une part de hasard évite de tomber dans un optimum local

Ainsi, l’outil apprend **quelles failles rapportent le plus** sur vos programmes HackerOne et concentre ses efforts dessus.

---

## 🚀 Installation

### Prérequis

- **Linux** (recommandé) ou **macOS**
- `bash` >= 4.0
- `curl` et `git` déjà présents
- Go (si vous voulez installer les outils ProjectDiscovery automatiquement)

### Quick start

```bash
git clone https://github.com/sonneper/bounty-slayer-ai.git
cd bounty-slayer-ai
chmod +x bountyslayer.sh
./bountyslayer.sh example.com
