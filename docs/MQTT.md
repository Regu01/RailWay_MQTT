# MQTT

## 🧭 Introduction

Le **MQTT Decoder** communique avec le reste du système via le protocole **MQTT** (Message Queuing Telemetry Transport).
Ce protocole léger permet d’échanger des **commandes** et **informations de capteurs** entre le décodeur matériel et une **application de contrôle** (comme **JMRI**, **Home Assistant**, ou un serveur MQTT local).


## ⚙️ Architecture de communication - Schéma simplifié
```
          +----------------------+
          |     JMRI / Client    |
          |----------------------|
          |  Publie : commandes  |
          |  Souscrit : capteurs |
          +----------+-----------+
                     |
              (Broker MQTT)
                     |
          +----------+-----------+
          |    MQTT Decoder 2    |
          |----------------------|
          | Publie : états       |
          | Souscrit : actions   |
          +----------------------+
```


## 🧠 Exemple d’utilisation avec **JMRI**

JMRI permet de relier directement ses capteurs et accessoires à MQTT.

**Configuration type :**

* **Serveur MQTT** : `192.168.1.50:1883`
* **Topic de base** : `railway/decoder1/`
* **Type de message** : `JSON` ou texte brut

**Exemple :**

* Occupation → JMRI écoute `railway/decoder1/occupancy/+`
* Signal → JMRI publie sur `railway/decoder1/signal/5`


## 🔧 Format des messages MQTT

Le firmware accepte plusieurs formats, selon le besoin.

### Format simple

```
railway/decoder1/signal/5 → GREEN
```

### Format JSON

```
railway/decoder1/signal/5 → {"state":"GREEN"}
```

### Format pour servo avec paramètre

```
railway/decoder1/servo/1 → {"angle":90}
```

## 📊 Exemple complet de configuration MQTT

Fichier `config.json` (côté firmware) :

```json
{
  "mqtt": {
    "host": "192.168.1.50",
    "port": 1883,
    "topic_root": "railway/decoder1/",
    "user": "train",
    "password": "secret"
  },
  "features": {
    "signals": 14,
    "servos": 14,
    "inputs": 8,
    "outputs": 14
  }
}
```

## 🔒 Sécurité

* Utiliser un **compte dédié** au module dans le broker MQTT.
* Si possible, activer **TLS (port 8883)** pour chiffrer les communications.
* Restreindre les topics autorisés à ce module.
* Toujours changer les identifiants par défaut.


## 📚 Références

* [Protocole MQTT officiel](https://mqtt.org/)
* [JMRI – Documentation MQTT](https://www.jmri.org/help/en/html/doc/Technical/MQTT.shtml)
* [Mosquitto broker](https://mosquitto.org/)