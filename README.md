# WalterWhiteBTC Umbrel App Store

Magasin communautaire Umbrel contenant **WalterWhiteBTC Monitoring**.

## Contenu de l'application

- Prometheus 3.14.0
- Node Exporter 1.12.1
- collecte toutes les 15 secondes
- retention maximale de 30 jours ou 2 Go
- interface Umbrel sur le port 9091
- acces direct pour Grafana sur le port 9090
- limites de 512 Mo pour Prometheus et 128 Mo pour Node Exporter

## Source de donnees Grafana

L'URL reste :

```text
http://192.168.1.51:9090
```

Le tableau de bord Grafana `Node Exporter Full`, identifiant `1860`, reste compatible.

## Installation

Une fois ce dossier publie dans le depot GitHub public
`WalterWhiteBTC/umbrel-app-store`, l'adresse suivante devient le magasin a
ajouter dans Umbrel :

```text
https://github.com/WalterWhiteBTC/umbrel-app-store
```

## Migration depuis l'installation Docker actuelle

La migration sera effectuee seulement apres apparition de l'application dans
Umbrel. Le volume actuel `monitoring_prometheus-data` restera intact pendant le
basculement, ce qui permettra de copier l'historique vers le volume gere par
l'application.

Les anciennes unites systemd sont :

```text
walterwhitebtc-monitoring.service
walterwhitebtc-monitoring.timer
```

Elles ne seront desactivees qu'au moment de l'installation de l'application,
afin d'eviter une interruption prematuree de la supervision.

## Securite

L'application ne contient aucune cle privee, seed phrase, adresse de paiement,
identifiant Bitcoin RPC ou autre secret. Node Exporter accede au systeme de
fichiers hote en lecture seule et toutes ses capacites Linux sont retirees.
