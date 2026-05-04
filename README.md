# SecuriGate : Système de Contrôle d'Accès Intelligent 

## Description du Projet
**SecuriGate** est un système de contrôle d'accès intelligent et performant, optimisé par l'utilisation du microcontrôleur **Raspberry Pi Pico 2W**. Ce projet intègre plusieurs protocoles de communication pour gérer de manière fluide et sécurisée l'accès à un espace dédié.

Le système utilise un lecteur **RFID RC522** via l'interface SPI pour l'authentification des badges. L'interface locale est assurée par un **écran LCD 1602 avec adaptateur I2C**, affichant l'état du système en temps réel. Pour une utilisation nocturne, une **photorésistance** mesure la luminosité ambiante et active progressivement une **LED** via un signal PWM lorsque la luminosité baisse.

Conformément aux recommandations pédagogiques, le projet abandonne les modules Bluetooth externes au profit du **Wi-Fi et du Bluetooth (BLE) intégrés** au Pico 2W. Cette architecture permet d'utiliser un ordinateur local comme serveur, où le téléphone se connecte pour récupérer les données et permettre un déverrouillage à distance sécurisé.

---

## Fonctionnalités Clés
*   **Autentificare RFID :** Lecture sécurisée des badges via le protocole SPI.
*   **Connectivité Sans Fil Native :** Utilisation du Wi-Fi et du BLE intégrés pour la télémétrie et le contrôle sans modules additionnels.
*   **Interface I2C :** Affichage des messages sur écran LCD en utilisant un nombre réduit de broches.
*   **Sortie d'Urgence :** Un bouton-poussoir déclenche une interruption matérielle pour une ouverture immédiate.
*   **Automatisation du Verrouillage :** Un timer referme automatiquement le servomoteur après cinq secondes.
*   **Gestion de la Lumière :** Contrôle adaptatif de la LED de courtoisie basé sur les données du convertisseur analogique-numérique (ADC).

---

## Liste des Composants et Coûts Estimés (Roumanie)

| Composant | Description / Rôle | Prix Estimé (RON) |
| :--- | :--- | :--- |
| **Raspberry Pi Pico 2W** | Microcontrôleur Dual-Core avec Wi-Fi/BLE intégré | 45 - 60 RON |
| **Lecteur RFID RC522** | Module de lecture avec badge et carte (SPI) | 15 - 25 RON |
| **Écran LCD 1602 + I2C** | Affichage des messages (SDA/SCL) | 25 - 35 RON |
| **Micro servomoteur SG90** | Actionneur pour le mécanisme de verrouillage | 12 - 18 RON |
| **Photorésistance (LDR)** | Capteur de luminosité ambiante (ADC) | ~2 RON |
| **LED & Bouton-poussoir** | Indicateur visuel et bouton d'urgence (Interrupt) | ~3 RON |
| **Accessoires** | Breadboard, câbles de liaison, résistances | 15 - 20 RON |

**TOTAL ESTIMÉ : 117 - 163 RON**

> **Note :** L'utilisation du Pico 2W permet une économie d'environ 40 RON par rapport à une solution avec module Bluetooth externe.

---

## Architecture Software et Librairies

### 1. Environnement de Développement
*   **Thonny IDE :** Utilisé pour le développement et l'upload du cod en MicroPython.
*   **Firmware :** MicroPython UF2 pour Raspberry Pi Pico 2 (RP2350).

### 2. Librairies MicroPython (Côté Client - Pico)
*   `mfrc522.py` : Gestion du module RFID via SPI.
*   `pico_i2c_lcd.py` : Pilote pour l'écran LCD 1602 via le protocole I2C.
*   `machine` : Bibliothèque native pour le contrôle des GPIO, PWM, ADC et Timers.
*   `network` & `ubluetooth` : Protocoles pour la communication avec le serveur local.

### 3. Architecture Serveur (Côté PC)
*   **Python 3.x :** Script serveur pentru centralizarea log-urilor de acces.
*   **Bleak :** Bibliothèque Python pour la communication BLE entre le PC et le Pico 2W.

---
<p align="center">Projet développé dans le cadre du Laboratoire de Systèmes Embarqués.</p>
