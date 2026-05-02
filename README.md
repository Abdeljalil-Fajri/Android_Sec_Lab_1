# Rapport de Laboratoire 1 : Mise en place de l'environnement d'audit Android (Mobexler)

Ce document détaille la procédure de configuration initiale de l'environnement de laboratoire pour l'audit d'applications Android, basée sur l'image Mobexler.

---

## 1. Objectifs du Laboratoire
L'objectif de ce premier laboratoire est d'établir une base de travail saine et reproductible pour les analyses futures. Les points clés incluent :
*   Le déploiement de la machine virtuelle Mobexler.
*   La configuration d'un double adressage réseau (NAT et Host-Only).
*   La vérification de la connectivité et des outils de débogage (ADB).
*   La création d'un point de restauration système (Snapshot).

---

## 2. Étape 1 : Acquisition et Importation de l'image
L'image au format Open Virtualization Format (OVA) a été téléchargée via le lien officiel. Comme illustré dans le fichier **"Screenshot 2026-05-01 223419_2.png"**, le fichier source est reconnu comme une appliance virtuelle prête à l'import.

### Configuration Matérielle
L'importation dans VMware Workstation a été effectuée avec les spécifications suivantes, visibles dans le fichier **"Screenshot 2026-05-01 231937_2.png"** :
*   **Mémoire vive (RAM) :** 4 Go.
*   **Processeurs :** 2 cœurs.
*   **Stockage :** Disque dur de 70 Go.

---

## 3. Étape 2 : Architecture Réseau
Pour répondre aux exigences de sécurité et de fonctionnalité, deux cartes réseau ont été configurées :
1.  **Adaptateur 1 (NAT) :** Fournit un accès stable à Internet pour la mise à jour des outils et le téléchargement de dépendances.
2.  **Adaptateur 2 (Host-Only) :** Crée un réseau isolé permettant une communication sécurisée entre la VM Mobexler et la cible Android (physique ou émulée) sans exposition au réseau local physique.

---

## 4. Étape 3 : Authentification et Premier Démarrage
Le système démarre sur un environnement Linux personnalisé. L'accès au bureau s'effectue via les identifiants par défaut :
*   **Utilisateur :** mobexler.
*   **Mot de passe :** mobexler.

L'interface de connexion est documentée dans le fichier **"Screenshot 2026-05-02 143728_2.jpg"**.

---

## 5. Étape 4 : Tests de Santé et Connectivité
Une fois la session ouverte, une série de tests a été réalisée dans le terminal pour valider l'environnement (réf. **"Screenshot 2026-05-01 232939_2.png"**) :

*   **Vérification des interfaces :** La commande `ip a` confirme la présence des interfaces `ens33` (NAT) et `ens34` (Host-Only) avec leurs adresses respectives.
*   **Test de résolution DNS et connectivité :** Un `ping -c 2 google.com` a été exécuté avec succès (0% de perte de paquets), confirmant que la route par défaut via le NAT est opérationnelle.

---

## 6. Étape 5 : Création de la Baseline (Snapshot)
La pérennité de l'environnement est assurée par la création d'un snapshot nommé **CLEAN_BASELINE_TP1**. Comme montré dans les fichiers **"Screenshot 2026-05-01 233059.png"** et **"Screenshot 2026-05-01 233216.png"**, ce point de restauration fige l'état du système après l'importation et la validation du réseau.

Cette étape est critique pour permettre un retour rapide à un état "propre" après toute manipulation invasive lors des futurs audits.

---

## 7. Étape 6 : Préparation de la Cible Android
L'outil **Android Debug Bridge (ADB)** est le composant central pour interagir avec la cible. Le fichier **"Screenshot 2026-05-01 234156_2.png"** confirme :
*   La version d'ADB installée (1.0.39).
*   Le démarrage réussi du démon ADB sur le port TCP 5037.

L'environnement est désormais prêt pour la connexion d'un périphérique via USB (Passthrough) ou via un émulateur sur le réseau Host-Only.
