# Lab 1 : Mise en place de l'environnement d'analyse Android Mobexler

## Introduction
Ce rapport documente la configuration initiale et la validation de l'environnement de laboratoire pour l'analyse de sécurité Android. L'objectif principal est de déployer la machine virtuelle Mobexler, de configurer une architecture réseau isolée mais fonctionnelle, et de garantir la reproductibilité des tests via des mécanismes de restauration.

---

## 1. Acquisition et Vérification de l'Image
La première étape consiste à télécharger l'image OVA officielle de Mobexler via le lien Google Drive fourni. 
*   **Vérification d'intégrité** : Afin d'éviter toute corruption de fichier ou altération malveillante, le calcul du hash SHA256 est préconisé avant l'importation.
*   **Commandes de contrôle** : L'utilisation de `Get-FileHash` sous Windows ou `sha256sum` sous Linux permet de garantir que l'image utilisée est identique à celle attendue par les mainteneurs du projet.

---

## 2. Importation et Configuration de l'Architecture Réseau
L'importation a été réalisée sous VMware Workstation, en respectant les spécifications matérielles requises pour une fluidité optimale de l'analyse.

### Spécifications Matérielles de la VM
Selon le fichier **Screenshot 2026-05-01 223419_2.png**, les ressources suivantes ont été allouées :
*   **Mémoire (RAM)** : 4 GB.
*   **Processeurs** : 2 cœurs.
*   **Disque Dur** : 70 GB (SCSI).

### Segmentation Réseau
L'environnement repose sur une double interface réseau pour isoler le trafic de laboratoire tout en maintenant un accès aux outils externes :
1.  **Adaptateur 1 (NAT)** : Fournit une connexion Internet stable pour les mises à jour et le téléchargement d'outils.
2.  **Adaptateur 2 (Host-Only)** : Crée un réseau privé dédié à la communication sécurisée entre la VM Mobexler et la cible Android.

---

## 3. Initialisation du Système et Authentification
Une fois la machine démarrée, l'accès au bureau s'effectue via l'interface de connexion. 
*   **Identifiants** : Comme illustré dans le fichier **Screenshot 2026-05-02 143728_2.jpg**, l'utilisateur `mobexler` est utilisé pour l'ouverture de session.
*   **Environnement** : Le système charge un environnement basé sur Linux optimisé pour le pentest mobile.

---

## 4. Tests de Santé et Validation Réseau
Une phase de diagnostic est cruciale pour confirmer que la segmentation réseau définie à l'étape 2 est opérationnelle. Les résultats observés dans le fichier **Screenshot 2026-05-01 231937_2.png** confirment les points suivants :

*   **Attribution IP** : L'interface `ens33` a reçu l'adresse `192.168.18.137` via DHCP sur le segment NAT.
*   **Connectivité Internet** : Un test de connectivité vers les DNS de Google (`ping -c 2 8.8.8.8`) a été validé avec un temps de réponse moyen de 28.3 ms.
*   **Résolution de noms (DNS)** : La commande `ping -c 2 google.com` a réussi, confirmant que le fichier `resolv.conf` est correctement configuré.
*   **Routage** : La table de routage (`ip route`) définit correctement la passerelle `192.168.18.2` pour le trafic externe.

---

## 5. Création du Snapshot de Référence (Baseline)
Pour garantir la possibilité de revenir à un état "propre" après des manipulations intrusives, un snapshot a été créé immédiatement après la validation des tests de santé.

Détails du snapshot (visibles dans le fichier **Screenshot 2026-05-01 233216.png**) :
*   **Nom** : `CLEAN_BASELINE_TP1`.
*   **Description** : Documente la réussite de l'import, de la configuration réseau (NAT + Host-Only), du boot et de la disponibilité d'ADB.

---

## 6. Configuration de l'Interface ADB (Android Debug Bridge)
Le pont de débogage Android est l'outil central pour interagir avec la cible (émulateur ou appareil physique). 

D'après le fichier **Screenshot 2026-05-01 234156_2.png**, l'état de l'outil est le suivant :
*   **Version** : ADB version 1.0.39.
*   **Emplacement** : `/usr/lib/android-sdk/platform-tools/adb`.
*   **Statut du démon** : Le serveur ADB a été démarré avec succès sur le port TCP 5037.

---

## Conclusion
Le Laboratoire 1 est désormais pleinement opérationnel. L'infrastructure Mobexler est correctement segmentée, la connectivité réseau est validée, et un point de restauration système a été établi. L'environnement est prêt pour l'intégration d'une cible Android et le début des analyses de sécurité applicative.
