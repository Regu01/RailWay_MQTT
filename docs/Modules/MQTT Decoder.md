# MQTT Decoder

Le MQTT Decoder 2 est une carte électronique destinée à la commande d’un réseau ferroviaire miniature via le protocole MQTT.
Elle permet de piloter les signaux, aiguillages, et détecteurs d’occupation en s’intégrant à un système domotique ou à une plateforme comme JMRI. Les fonctionnalités principales sont les suivantes :

* Détection des blocs d’occupation
* Commande des signaux et aiguillages
* Logique auxiliaire configurable
* Intégration MQTT pour contrôle via JMRI ou tout broker MQTT


## ⚙️ Spécifications matérielles principales

| Composant / Fonction           | Quantité | Description                                              |
| ------------------------------ | -------- | -------------------------------------------------------- |
| **Entrées de détection**       | 12       | Capteurs de présence / occupation (opto-isolés)          |
| **Sorties de signal**          | 42       | Commande jusqu’à 14 signaux 3-aspects (rouge/jaune/vert) |
| **Sorties logiques**           | 14       | Commande de relais ou d’équipements auxiliaires          |
| **Entrées logiques**           | 8        | Entrées digitales pour boutons, interrupteurs, etc.      |
| **Sorties servo**              | 14       | Commande de servomoteurs (aiguillages ou barrières)      |
| **Interface de communication** | 1        | Port série / UART + module réseau (ESP32 ou ESP8266)     |
| **Alimentation**               | 1        | Entrée 5 V / 12 V selon la configuration                 |
| **Connecteurs**                | -        | Borniers à vis pour signaux, capteurs et servos          |


## 🧠 Microcontrôleur et communication

Le cœur du système repose sur un **ESP8266**, permettant :

* la connexion Wi-Fi au réseau local,
* la communication via **MQTT** (avec un broker local ou distant),
* la gestion en temps réel des entrées/sorties (signaux, servos, capteurs).

Le firmware écoute et publie sur des **topics MQTT** définis selon le plan du réseau ferroviaire.

## 🧱 Architecture matérielle

### 🔹 1. Module principal (MCU)

* **ESP32 / ESP8266** : cœur du système, gère la logique MQTT.
* **UART / USB** : pour la programmation et le débogage.
* **Alimentation** : 5 V via USB ou bornier externe.

### 🔹 2. Extension d’entrées/sorties

* Basée sur **PCA9685** (PWM) ou **MCP23017** (I2C) pour étendre les E/S.
* Les signaux sont multiplexés via des registres à décalage ou des drivers ULN2803.

### 🔹 3. Détection d’occupation

* Entrées isolées par optocoupleurs pour protéger le microcontrôleur.
* Permet la détection de courant sur les rails (ou capteurs IR).

### 🔹 4. Commande de servos

* Connecteurs 3 broches (signal + 5 V + GND).
* Pilotage par PWM via un contrôleur dédié (ex. PCA9685).

### 🔹 5. Alimentation

* 5 V pour la logique et les signaux LED.
* 12 V optionnel pour servos puissants ou relais.
* Régulateur intégré pour stabiliser les tensions internes.


## 🧭 Installation & mise en route

1. [Télécharger ou cloner le dépôt du projet](https://github.com/Regu01/RailWay_MQTT_Decoder.git)
2. Vérifier la configuration matérielle : connecter les capteurs, sorties signaux, sorties servo.
3. Configurer le broker MQTT (hôte, port, topics) et intégrer dans JMRI ou autre interface.
4. Charger le firmware sur l’unité de contrôle.
5. Tester via MQTT : publication/souscription sur les topics prévus, vérifier que les capteurs réagissent et que les sorties commandent les signaux/servos.


## 🔧 Configuration

* Broker MQTT : `mqtt://<adresse>`
* Topics typiques : `railway/decoder/#`
* Variables d’environnement ou fichier de config à adapter selon installation (ex. : nombre de blocs, adresses des sorties, logiques auxiliaires).
* Adapter le firmware ou la logique aux besoins spécifiques de ton réseau ferroviaire miniature.