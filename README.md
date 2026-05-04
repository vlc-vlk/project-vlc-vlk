<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
</head>
<body>

    <h1>SecuriGate : Système de Contrôle d'Accès Intelligent (avec Pico 2W)</h1>

    <h2>Description du Projet</h2>
    <p>
        <strong>SecuriGate</strong> est un système de contrôle d'accès intelligent et performant, optimisé par l'utilisation du microcontrôleur <strong>Raspberry Pi Pico 2W</strong>. Ce projet intègre plusieurs protocoles de communication pour gérer de manière fluide et sécurisée l'accès à un espace dédié.
    </p>
    <p>
        Le système utilise un lecteur <strong>RFID RC522</strong> via l'interface SPI pour l'authentification des badges. L'interface locale est assurée par un <strong>écran LCD 1602 avec adaptateur I2C</strong>, affichant l'état du système en temps réel. Pour une utilisation nocturne, une <strong>photorésistance</strong> mesure la luminosité ambiante et active progressivement une <strong>LED</strong> via un signal PWM lorsque la luminosité baisse.
    </p>
    <p>
        Conformément aux recommandations pédagogiques, le projet abandonne les modules Bluetooth externes au profit du <strong>Wi-Fi et du Bluetooth (BLE) intégrés</strong> au Pico 2W. Cette architecture permet d'utiliser un ordinateur local comme serveur, où le téléphone se connecte pour récupérer les données et permettre un déverrouillage à distance sécurisé.
    </p>

    <hr>

    <h2>Fonctionnalités Clés</h2>
    <ul>
        <li><strong>Authentification RFID :</strong> Lecture sécurisée des badges via le protocole SPI.</li>
        <li><strong>Connectivité Sans Fil Native :</strong> Utilisation du Wi-Fi et du BLE intégrés pour la télémétrie et le contrôle sans modules additionnels.</li>
        <li><strong>Interface I2C :</strong> Affichage des messages sur écran LCD en utilisant un nombre réduit de broches.</li>
        <li><strong>Sortie d'Urgence :</strong> Un bouton-poussoir déclenche une interruption matérielle pour une ouverture immédiate.</li>
        <li><strong>Automatisation du Verrouillage :</strong> Un timer referme automatiquement le servomoteur après cinq secondes.</li>
        <li><strong>Gestion de la Lumière :</strong> Contrôle adaptatif de la LED de courtoisie basé sur les données du convertisseur analogique-numérique (ADC).</li>
    </ul>

    <hr>

    <h2>Liste des Composants et Coûts Estimés (Roumanie)</h2>
    <table border="1" style="width:100%; border-collapse: collapse; text-align: left;">
        <thead>
            <tr style="background-color: #f2f2f2;">
                <th style="padding: 8px;">Composant</th>
                <th style="padding: 8px;">Description / Rôle</th>
                <th style="padding: 8px;">Prix Estimé (RON)</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td style="padding: 8px;"><strong>Raspberry Pi Pico 2W</strong></td>
                <td style="padding: 8px;">Microcontrôleur Dual-Core avec Wi-Fi/BLE intégré</td>
                <td style="padding: 8px;">45 - 60 RON</td>
            </tr>
            <tr>
                <td style="padding: 8px;"><strong>Lecteur RFID RC522</strong></td>
                <td style="padding: 8px;">Module de lecture avec badge et carte (SPI)</td>
                <td style="padding: 8px;">15 - 25 RON</td>
            </tr>
            <tr>
                <td style="padding: 8px;"><strong>Écran LCD 1602 + I2C</strong></td>
                <td style="padding: 8px;">Affichage des messages (SDA/SCL)</td>
                <td style="padding: 8px;">25 - 35 RON</td>
            </tr>
            <tr>
                <td style="padding: 8px;"><strong>Micro servomoteur SG90</strong></td>
                <td style="padding: 8px;">Actionneur pour le mécanisme de verrouillage</td>
                <td style="padding: 8px;">12 - 18 RON</td>
            </tr>
            <tr>
                <td style="padding: 8px;"><strong>Photorésistance (LDR)</strong></td>
                <td style="padding: 8px;">Capteur de luminosité ambiante (ADC)</td>
                <td style="padding: 8px;">~2 RON</td>
            </tr>
            <tr>
                <td style="padding: 8px;"><strong>LED & Bouton-poussoir</strong></td>
                <td style="padding: 8px;">Indicateur visuel et bouton d'urgence (Interrupt)</td>
                <td style="padding: 8px;">~3 RON</td>
            </tr>
            <tr>
                <td style="padding: 8px;"><strong>Accessoires</strong></td>
                <td style="padding: 8px;">Breadboard, câbles de liaison, résistance 10kΩ</td>
                <td style="padding: 8px;">15 - 20 RON</td>
            </tr>
        </tbody>
        <tfoot>
            <tr style="font-weight: bold; background-color: #f9f9f9;">
                <td colspan="2" style="padding: 8px; text-align: right;">TOTAL ESTIMÉ</td>
                <td style="padding: 8px;">117 - 163 RON</td>
            </tr>
        </tfoot>
    </table>
    <p><em>*Note : L'utilisation du Pico 2W permet une économie d'environ 40 RON par rapport à une solution avec module Bluetooth externe.</em></p>

    <hr>

    <h2>Architecture Software et Librairies</h2>

    <h3>1. Environnement de Développement</h3>
    <ul>
        <li><strong>Thonny IDE :</strong> Utilisé pour le développement et l'upload du code en MicroPython.</li>
        <li><strong>Firmware :</strong> MicroPython UF2 pour Raspberry Pi Pico 2 (RP2350).</li>
    </ul>

    <h3>2. Librairies MicroPython (Côté Client - Pico)</h3>
    <ul>
        <li><code>mfrc522.py</code> : Gestion du module RFID via SPI.</li>
        <li><code>pico_i2c_lcd.py</code> : Pilote pour l'écran LCD 1602 via le protocole I2C.</li>
        <li><code>machine</code> : Bibliothèque native pour le contrôle des GPIO, PWM, ADC et Timers.</li>
        <li><code>network</code> & <code>ubluetooth</code> : Protocoles pour la communication avec le serveur local.</li>
    </ul>

    <h3>3. Architecture Serveur (Côté PC)</h3>
    <ul>
        <li><strong>Python 3.x :</strong> Script serveur pour centraliser les logs d'accès.</li>
        <li><strong>Bleak :</strong> Bibliothèque Python pour la communication Bluetooth Low Energy (BLE) entre le PC et le Pico 2W.</li>
    </ul>

    <hr>

    <p align="center">
        <em>Projet développé dans le cadre du Laboratoire de Systèmes Embarqués.</em>
    </p>

</body>
</html>
