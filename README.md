# WalterWhiteBTC Umbrel App Store

Magasin communautaire Umbrel contenant **WalterWhiteBTC Monitoring**.

## Contenu de l'application

- Prometheus 3.14.0
- Node Exporter 1.12.1
- collecte toutes les 15 secondes
- retention maximale de 30 jours ou 2 Go
- interface Umbrel sur le port 9091
- acces interne securise pour Grafana
- limites de 512 Mo pour Prometheus et 128 Mo pour Node Exporter
- stockage persistant gere par Umbrel
- identification du site `theoule` et du serveur `theoule-umbrel`

## Source de donnees Grafana

L'URL interne recommandee est :

```text
http://walterwhitebtc-monitoring_prometheus_1:9090
```

Le tableau de bord Grafana `Node Exporter Full`, identifiant `1860`, reste compatible.
Le port Prometheus n'est plus expose directement sur le reseau local. L'interface
reste accessible depuis l'icone Umbrel, tandis que Grafana communique avec
Prometheus sur le reseau Docker interne d'Umbrel.

Avant la mise a jour depuis la version 1.0.0, la source de donnees Grafana doit
utiliser cette URL interne et afficher `Successfully queried the Prometheus API`.

## Installation

Une fois ce dossier publie dans le depot GitHub public
`WalterWhiteBTC/umbrel-app-store`, l'adresse suivante devient le magasin a
ajouter dans Umbrel :

```text
https://github.com/WalterWhiteBTC/umbrel-app-store
```

## Migration des donnees

Lors de la premiere mise a jour vers la version 1.0.1, le service temporaire
`migrate-data` copie automatiquement les donnees depuis le volume de la version
1.0.0 vers le repertoire persistant gere par Umbrel :

```text
walterwhitebtc-monitoring_prometheus-data
```

Le volume source reste intact afin de permettre un retour arriere. L'ancien
volume manuel `monitoring_prometheus-data` reste egalement intact, mais il n'est
pas fusionne automatiquement avec la nouvelle base.

Apres la mise a jour, le tableau de bord utilise l'instance
`theoule-umbrel`. Les anciennes series restent consultables dans la meme base
sous l'instance `node-exporter:9100`.

Les anciennes unites systemd sont :

```text
walterwhitebtc-monitoring.service
walterwhitebtc-monitoring.timer
```

Elles restent desactivees apres l'installation de l'application Umbrel.

## Securite

L'application ne contient aucune cle privee, seed phrase, adresse de paiement,
identifiant Bitcoin RPC ou autre secret. Node Exporter accede au systeme de
fichiers hote en lecture seule et toutes ses capacites Linux sont retirees.
Prometheus fonctionne egalement sans capacite Linux, avec l'option
`no-new-privileges`. Les images multi-architectures sont verrouillees par leur
empreinte SHA-256 et incluent ARM64 pour le Raspberry Pi 5.
