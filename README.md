# Système de Scoring Lighthouse Multi-API

Ce système permet d'analyser en masse les performances web de sites en utilisant l'API Google PageSpeed Insights avec plusieurs clés API en parallèle.

## 🎯 Objectif du Projet

Le système est composé de trois scripts principaux qui travaillent ensemble pour analyser efficacement les performances web d'un grand nombre de sites :

1. **Division des données** (`split_csv.js`)
2. **Analyse parallèle** (`master_script.js`)
3. **Analyse détaillée** (`score_lighthouse.js`)
4. **Changement de hook** (`Changement_hook.js`)

## 📋 Description des Scripts

### 1. Script de Division (`split_csv.js`)
1. Placez votre fichier CSV à diviser (nommé `leadsFull.csv`) dans le même dossier que le script
2. Exécutez le script :
```bash
node split_csv.js
```

- Divise un fichier CSV volumineux en plusieurs fichiers plus petits
- Crée des morceaux de 600 lignes avec en-têtes conservés
- Génère des fichiers `leads1.csv`, `leads2.csv`, etc. dans le dossier `output`

### 2. Script Principal (`master_script.js`)
- Orchestre l'analyse parallèle avec 3 clés API
- Divise le fichier d'entrée en 3 parties
- Lance 3 instances en parallèle
- Fusionne les résultats dans `leads_results.csv`

### 3. Script d'Analyse (`score_lighthouse.js`)
- Effectue l'analyse détaillée des performances
- Analyse mobile et SEO via Lighthouse
- Support des métriques CrUX (Core Web Vitals)
- Options configurables via ligne de commande

### 4. Script de Génération de Hooks (`changement_hook.js`)
- Génère des messages personnalisés basés sur les scores de performance
- Analyse les métriques suivantes :
  - Performance mobile (PSI)
  - SEO
  - Core Web Vitals (LCP, CLS, INP)
- Crée deux types de messages :
  - Un hook principal personnalisé selon les problèmes détectés
  - Une phrase d'impact statistique pour renforcer le message
- Seuils de performance configurés :
  - Performance mobile : Poor < 49, NI < 89
  - SEO : Poor < 79, NI < 89
  - LCP : Poor > 4000ms, NI > 2500ms
  - INP : Poor > 500ms, NI > 200ms
  - CLS : Poor > 0.25, NI > 0.1
- Génère un fichier CSV avec les colonnes :
  - `generated_merged_hook` : Message principal personnalisé
  - `generated_stat_impact_sentence` : Statistique d'impact

## 🚀 Fonctionnalités

- Analyse des performances mobiles et SEO via Lighthouse
- Support des métriques CrUX (Core Web Vitals)
- Traitement parallèle avec 3 clés API
- Sauvegardes automatiques tous les 100 traitements
- Gestion robuste des erreurs
- Fusion automatique des résultats

## 📋 Prérequis

- Node.js (v14 ou supérieur)
- Les dépendances npm listées dans `package.json`

## 📦 Installation

1. Cloner le repository
2. Installer les dépendances :
```bash
npm install
```

## 🔑 Configuration des Clés API

Le système utilise 3 clés API Google PageSpeed Insights pour le traitement parallèle.

## 📝 Utilisation

### Format du fichier d'entrée
Le fichier CSV d'entrée doit contenir une colonne `Website` avec les URLs à analyser.

### Lancer l'analyse
```bash
node master_script.js leads.csv
```

### Options du script d'analyse
```bash
node score_lighthouse.js --input <fichier> --output <fichier> --api-key <clé> [options]

Options :
  --input, -i     Fichier CSV d'entrée (obligatoire)
  --output, -o    Fichier CSV de sortie (obligatoire)
  --api-key       Clé API Google (obligatoire)
  --crux          Activer l'analyse CrUX
  --concurrency   Nombre de requêtes simultanées (défaut: 4)
```

## 📊 Métriques Analysées

### Lighthouse
- Performance mobile (score 0-100)
- SEO mobile (score 0-100)

### Core Web Vitals (optionnel)
- LCP (Largest Contentful Paint)
- CLS (Cumulative Layout Shift)
- INP (Interaction to Next Paint)

## 💾 Sauvegardes

- Sauvegardes automatiques tous les 200 traitements
- Format : `results_backup_XXX.csv`
- En cas d'erreur, le script tente de sauvegarder l'état actuel

## ⚠️ Limitations

- Maximum 4 requêtes par seconde par clé API
- Les URLs doivent être valides et accessibles
- Format CSV requis pour l'entrée/sortie

## 🔍 Détails techniques

### Structure des fichiers
- `master_script.js` : Script principal de distribution
- `score_lighthouse.js` : Script d'analyse individuel
- Fichiers temporaires : `temp_input_X.csv`, `temp_output_X.csv`

### Gestion des erreurs
- Logs détaillés pour chaque instance
- Sauvegardes automatiques
- Nettoyage des fichiers temporaires

## 📝 Exemple de sortie

```csv
Website,psi_mobile_score,psi_seo_score,lcp_p75_ms,cls_p75,inp_p75_ms,custom_hook
https://example.com,85,90,1200,0.1,200,""
https://slow-site.com,45,75,3500,0.3,450,"Google évalue votre site mobile à seulement 45/100 de performance..."
```
