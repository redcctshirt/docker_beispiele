# Docker - Beispiele

https://www.docker.com/ - Website    
https://hub.docker.com/ - Images

* [Installation](#installation)
  * [bei Fedora - per Skript](#bei-fedora---per-skript)
  * [bei Fedora](#bei-fedora)
  * [docker-Gruppe erstellen](#docker-gruppe-erstellen)
* [Infos anzeigen](#infos-anzeigen)
* [Beispiel](#beispiel)
  * [Hallo Welt](#hallo-welt)
  * [Hallo Welt-Dienst starten](#hallo-welt-dienst-starten)
* [Kommandos](#kommandos)
* [Image erstellen](#image-erstellen)
* [Image mit Dockerfile erstellen](#image-mit-dockerfile-erstellen)
* [Beispiel 2](#beispiel-2)
  * [Redis Server und Client](#redis-server-und-client)
* [Container mit Außenwelt verbinden](#container-mit-außenwelt-verbinden)
* [Container mit Volumes](#container-mit-volumes)
* [Beispiel 3](#beispiel-3)
* [Beispiel 4](#beispiel-4)
* [Automation mit docker-compose](#automation-mit-docker-compose)
* [lokale Registry](#lokale-registry)
* [docker-machine](#docker-machine)
* [Logging der Container](#logging-der-container)
* [Vernetzung](#vernetzung)
* [Orchestrieren](#orchestrieren)
* [Verwaltung](#verwaltung)
* [Tools](#tools)
 
* schlanke und portable Variante für Anwendungen
* Container können schnell gestartet und beendet werden
* Container sind portierbar
* sehr viele Container auf einem Host möglich
* Container nutzen Ressourcen vom Betriebssystem
* Anwendungen ohne viel Installation und Konfiguration sind machbar

## Installation

### bei Fedora - per Skript

```
# Install-Skript herunterladen und ausführen
curl -fsSL https://get.docker.com/ | sh

# Dienst aktivieren 
systemctl enable docker.service

# Dienst starten
systemctl start docker
docker daemon - Docker manuell starten

# Hallo Welt - Prüfen ob docker läuft
docker run hello-world
```

### bei Fedora

```
# Docker installieren
dnf install docker

# Dienst aktivieren 
systemctl enable docker.service

# Dienst starten
systemctl start docker
docker daemon - Docker manuell starten

# Hallo Welt - Prüfen ob docker läuft
docker run hello-world
```

### docker-Gruppe erstellen

```
# Anwendung von docker ohne su oder sudo wird damit möglich gemacht
# Benutzer zur Gruppe docker hinzufügen
usermod -aG docker Benutzername

# Hallo Welt - Prüfen ob docker auch ohne su oder sudo läuft
docker run hello-world
```

## Infos anzeigen

```
# Version anzeigen
docker version

# Hilfe anzeigen
docker --help

# Systeminfos anzeigen
docker info

# Hilfe für ein Kommando (Beispiel ps) anzeigen
docker ps --help

# alle Container anzeigen
docker ps -a

# alle laufenden Container anzeigen
docker ps

# alle Images anzeigen
docker images

# Logging (stdout) von einem laufenden Container anzeigen, Containername wird bei docker ps angezeigt
docker logs containername
```

## Beispiel

### Hallo Welt

```
# Hallo Welt
docker run hello-world

# Programm cowsay mit dem Parameter "Hallo Welt" im Container (Image: docker/whalesay) starten
docker run docker/whalesay cowsay Hallo Welt

# Kommando echo 'Hello World' im Container (Image: ubuntu) ausführen
docker run ubuntu /bin/echo 'Hello world'

# interaktiven container starten, -t terminal -i interaktive Verbindung
# im Container (Image: ubuntu) /bin/bash starten, Kommandos können nun ausgeführt werden
# mit exit beenden
docker run -t -i ubuntu /bin/bash

# -h neuen Hostnamen vergeben
docker run -h newhostname -t -i ubuntu /bin/bash
```

### Hallo Welt-Dienst starten

```
# -d im Hintergrund laufen lassen (als Daemon)
# im Container (Image: ubuntu) /bin/sh starten und Befehle ausführen
docker run -d ubuntu /bin/sh -c "while true; do echo Hallo Welt; sleep 1; done"
```

## Kommandos

```
# Version anzeigen
docker version

# Hilfe anzeigen
docker --help

# Systeminfos anzeigen
docker info

# alle Images anzeigen
docker images

# alle laufenden Container anzeigen
docker ps

# alle Container anzeigen
docker ps -a

# Logging (stdout) vom Container anzeigen, Containername wird bei docker ps angezeigt
docker logs containername

# neuen Container mit Image hello-world starten, --name Name für Container festlegen
docker run --name jim hello-world

# Container stoppen
docker stop containername

# Container starten
docker start containername

# Container neu starten
docker restart containername

# laufenden Prozesse im Container anzeigen
docker top containername

# alle Prozesse im Container pausieren
docker pause containername

# alle Prozesse im Container wieder laufen lassen
docker unpause containername

# Containerinfos herausfinden
docker inspect containername

# IP-Adresse des Containers ermitteln
docker inspect --format {{.NetworkSettings.IPAddress}} containername

# Änderungen im Container anzeigen
docker diff containername

# Container löschen
docker rm containername

# alle Container löschen
docker rm $(docker ps -aq)

# Image löschen
docker rmi image

# Container löschen, wenn er beendet wurde
docker run --rm hello-world

# Schichten eines Images anzeigen lassen
docker history imagename

# Container aus Image erzeugen, ohne Start
docker create

# Datein kopieren zwischen Host und Container
docker cp

# Befehl in den Container absetzen
docker exec

# Container abbrechen
docker kill

# Events vom Daemon ausgeben
docker events

# verwendete Ports auflisten
docker port

# Containerdateisystem exportieren, importieren
docker export/import

# Repository (Image) aus Archiv laden/sichern
docker load/save
docker save -o x.tar ubuntu:latest
docker load -i x.tar

# Tag zuweisen
docker tag

# Registry-Kommandos
docker login/logout/pull/push/search

# Image-Update
docker pull image
docker stop container
docker rm container
docker run ... image
```

## Image erstellen

```
# Container mit bash starten (Image: ubuntu)
docker run -it --name containername ubuntu bash

# Programme installieren, System-Änderungen durchführen 
apt-get install terminix

# Container verlassen
exit 

# Image erstellen
docker commit containername repositoryname/imagename

# Container mit neuem Image starten
docker run repositoryname/imagename
```

## Image mit Dockerfile erstellen

```
# Dockerfile erstellen
touch Dockerfile

# Dockerfile-Inhalt ändern (nano Dockerfile)
FROM debian:wheezy # Image bestimmen
RUN apt-get install terminix # Kommando ausführen
# mit jedem Kommando wird eine neue Schicht und ein temp. Container erstellt
# docker history image - Schichten anzeigen lassen

# Image erstellen
docker build -t repositoryname/imagename .
# Build auch mit git, http, oder tar.gz möglich

# Container mit neuem Image starten
docker run repositoryname/imagename 

# .dockerignore - Dateien für build-Context ignorieren
# minimale Images mit Paketmanager: alpine (5MB), debian, ubuntu, phusion
# immer aktuelles Image vor dem build herunterladen, docker pull
# Anweisungen für Dockerfiles: ADD, ARG, CMD, COPY, ENTRYPOINT, ENV, EXPOSE, FROM
# HEALTHCHECK, MAINTAINER, LABEL, ONBUILD, RUN, SHELL, STOPSIGNAL, USER, VOLUME, WORKDIR
```

* Skript nimmt Argumente entgegen

```
# Dockerfile erstellen und Skript erstellen
touch Dockerfile
touch skript.sh

# Skript-Inhalt ändern (nano skript.sh)
#!/bin/bash
if [ $# -eq 0 ]; then
   ...
else
   ...
fi

# Dockerfile-Inhalt ändern (nano Dockerfile)
FROM debian:wheezy # Image bestimmen
RUN apt-get install terminix # Kommando ausführen
COPY skript.sh / # skript.sh nach / kopieren
ENTRYPOINT ["/skript.sh"] # Skript ausführen

# Image erstellen
docker build -t repositoryname/imagename .

# Container mit neuem Image starten
docker run repositoryname/imagename 
```

* Image bei Docker Hub hochladen (Account bei Docker Hub erstellen)

```
# Dockerfile erstellen und Skript erstellen
touch Dockerfile
touch skript.sh

# Skript-Inhalt ändern (nano skript.sh)
#!/bin/bash
if [ $# -eq 0 ]; then
   ...
else
   ...
fi

# Dockerfile-Inhalt ändern (nano Dockerfile)
FROM debian:wheezy # Image bestimmen
MAINTAINER Vorname Name <email@domain.com>
RUN apt-get install terminix # Kommando ausführen
COPY skript.sh / # skript.sh nach / kopieren
ENTRYPOINT ["/skript.sh"] # Skript ausführen

# Image erstellen
docker build -t docker_hub_benutzername/imagename .

# Container mit neuem Image bei Docker Hub hochladen
docker push docker_hub_benutzername/imagename
```

## Beispiel 2

### Redis Server und Client

```
# Redis Server und Client in 2 verschiedenen Containern starten
# redis-Image herunterladen
docker pull redis

# Redis Server starten (-d Daemon), --name Containername
docker run --name redisserver -d redis

# Redis Client starten, mit Container redisserver verbinden und als redisd ansprechen
docker run --rm -it --link redisserver:redisd redis /bin/bash
redis-cli -h redisd -p 6379 # Redis-Client probieren
exit # Redis-Client beenden
exit # Container beenden

# Redis Server starten und Verzeichnis vom Host nutzen (Volumes)
docker run -v /host/verzeichnis:/container/verzeichnis --name redisserver -d redis

# Redis Client starten und Verzeichnis vom Host nutzen, Datei kopieren
docker run --rm --volumes-from redisserver -v /host/verzeichnis:/container/verzeichnisvolumes ubuntu cp /redisservervolumes/datei /container/verzeichnisvolumes
```

## Container mit Außenwelt verbinden

```
# Ports verbinden (lokal 4000 mit Container 80, -P automatisch freien Port wählen)
docker run -d -p 4000:80 nginx
Firefox http://localhost:4000

# belegte Ports auflisten
docker port

# Docker-Container untereinander verlinken
docker run -d --name redisserver redis
docker run --link redisserver:redisd debian env
```

## Container mit Volumes

```
# Volumes nutzen, Verzeichnis auf dem Host das im Container gemounted wird
docker run -it --name containername -h hostname -v /volumes ubuntu /bin/bash
# oder -v Hostdir:Containerdir
# --volumes-from containername - Volumes aus anderen Containern verwenden

# Volumes im Container auflisten
docker inspect -f {{.Mounts}} containername

# Volumes verwalten
docker volume

# nicht mehr genutzte Volumes löschen (gespeichert unter /var/lib/docker/volumes)
docker volume prune
```

## Beispiel 3

* eine Webanwendung im Docker-Container

```
webapp/service.py
from flask import Flask
webapp = Flask(__name__)
@webapp.route('/')
def hallo():
    return 'Hallo\n'
if __name__ == '__main__':
    webapp.run(debug=True, host='0.0.0.0')

Dockerfile
FROM python:3.6
RUN pip install Flask==0.12.0
WORKDIR /webapp
COPY webapp /webapp
CMD ["python","service.py"]

# Image erstellen
docker build -t webapp .
# Container mit Image webapp laufen lassen
docker run -d -p 5000:5000 webapp
# Client
Firefox http://localhost:5000
curl $(docker inspect --format {{.NetworkSettings.IPAddress}} containername):5000
# Container anzeigen, -l latest, -q quiet
docker ps -lq
# Container mit Image webapp laufen lassen, Volume einsetzen
docker run -d -p 5000:5000 -v "$PWD"/webapp:/webapp webapp
```

## Beispiel 4

* eine produktive Webanwendung im Docker-Container
* mit uWSGI Anwendungsserver für das Produkt (https://uwsgi-docs.readthedocs.io/en/latest/index.html)
* Dockerfiles ohne Userangabe - Container läuft immer mit Benutzer root

```
webapp/service.py
from flask import Flask
webapp = Flask(__name__)
@webapp.route('/')
def hallo():
    return 'Hallo\n'
if __name__ == '__main__':
    webapp.run(debug=True, host='0.0.0.0')

Dockerfile
FROM python:3.6
RUN groupadd -r uwsgi && useradd -r -g uwsgi uwsgi
RUN pip install Flask==0.12.0 uWSGI==2.0.10
WORKDIR /webapp
COPY webapp /webapp
# COPY script.sh /
EXPOSE 9090 9191
USER uwsgi
CMD ["uwsgi","--http", "0.0.0.0:9090","--wsgi-file", "/webapp/service.py", "--callable", "webapp", "--stats", "0.0.0.0:9191"]
# CMD ["/script.sh"]

# Image erstellen
docker build -t webapp .
# Container mit Image webapp laufen lassen, -P zufällige Ports wählen
# -e Umgebungsvariablen setzen, die eventuell in einem Skript ausgewertet werden
docker run -d -P webapp
# Ports anzeigen
docker port containername
# Logs anzeigen
docker logs containername
# Client
Firefox http://localhost:9090
curl $(docker inspect --format {{.NetworkSettings.IPAddress}} containername):9090
# Container anzeigen, -l latest, -q quiet
docker ps -lq
# Container mit Image webapp laufen lassen, Volume einsetzen
docker run -d -p 9090:9090 -p 9191:9191 -v "$PWD"/webapp:/webapp webapp
```

## Beispiel 5

```
# Wordpress
docker network create mynet
docker run -d --name wpcontainer --network mynet -v /home/wordpress/wordpress-html:/var/www/html -p 80:80 -e WORDPRESS_DB_PASSWORD=password -e WORDPRESS_DB_HOST=mariadb-container wordpress
```

## Beispiel 6

```
# MariaDB + phpmyadmin
docker network create mynet
docker run -d --name mariadb-container -p 3306:3306 -e MYSQL_ROOT_PASSWORD=password --network mynet -v /home/username/mariadb:/var/lib/mysql mariadb
docker run -d --name pma-container -p 8080:80 --network mynet -e PMA_HOST=mariadb-container phpmyadmin/phpmyadmin
```

## Beispiel 7

```
# Node.js
# Dockerfile
FROM node:10
ENV TZ="Europa/Berlin"
COPY bsp.js /src/
USER node
CMD ["node", "/src/bsp.js"]

# bsp/bsp.js
const http = require("http"), os = require("os");
http.createServer((req,res) => {
 const data = new Date(),
 htmldata = `<!DOCTYPE html>
<html>
 <head>
  <title>Titel</title>
  <meta charset="utf-8" />
 </head>
 <body>
  <h1>Seite</h1>
  ${data}
 </body>
</html>`
res.setHeader('Content-Type', 'text/html');
res.end(htmldata);
}).listen(8080);

docker build -t bsp .
docker run -p 8080:8080 bsp
```

## Beispiel 8

```
# PHP
# Dockerfile
FROM php:7-apache
ENV TZ="Europa/Berlin"
COPY index.php /var/www/html

# bsp/index.html
<!DOCTYPE html>
<html>
 <head>
  <title>Titel</title>
  <meta charset="utf-8" />
 </head>
 <body>
  <h1>Seite</h1>
  <?php echo date("c"); ?>
 </body>
</html>

docker build -t bsp .
docker run -p 80:80 bsp
```

## Automation mit docker-compose

* Automation wird per YAML-Konfigurationsdatei erreicht

```
docker-compose.yml
webapp:
 build: .
 ports:
  - "9090:9090"
 environment:
  ENV: TEST
 volumes:
  - ./webapp:/webapp
 links:
  - containername
containername:
 image: user/image:version

# startet alle Container aus der compose-YAML-Datei
# -f compose-Datei, -d als Daemon
docker-compose up

# Infos
docker-compose ps
# Logs anzeigen
docker-compose logs
# alle Container stoppen
docker-compose stop
# alle Container entfernen, die nicht laufen
docker-compose rm

# nach Änderungen, alle Images neu erstellen
# --force-recreate alle Container stoppen und neu erstellen
docker-compose build
docker-compose up -d
```

## lokale Registry

* eigene Registry für Images starten

```
# Registry starten
docker run -d -p 5000:5000 registry:2

# Image auf lokale Registry hochladen
docker push localhost:5000/image:version

# Image von der lokalen Registry herunterladen
docker pull localhost:5000/image:version
```

## docker-machine

* Hosts für Container verwalten

```
# vorhandene Hosts auflisten
docker-machine ls

# Host erstellen
# z.B. bei Remote-Dienst https://www.digitalocean.com/
docker-machine create

# IP ausgeben
docker-machine ip host

# Host stoppen
docker-machine stop host

# Host löschen
docker-machine rm host

# per ssh mit Host verbinden
docker-machine ssh host

# Datei in Host kopieren
docker-machine scp datei host:/datei

# Weitere Kommandos
start, inspect, status, active, restart, url, env, config
``` 

## Logging der Container

* STDOUT und STDERR wird per Log angezeigt

```
# Logging anzeigen, -t mit Zeitstempel, -f streamen 
docker logs container

# per Remote API
curl -i --cacert ~/.docker/machine/certs/ca.pem --cert ~/.docker/machine/certs/ca.pem --key ~/.docker/machine/certs/key.pem "https://$(docker-machine ip default):2376/containers/$(docker ps -lq)/logs?stderr=1&stdout=1"

# Logging-Methode ändern
docker run -d image --log-driver json-file|syslog|none|fluentd|awslogs usw.

# Logs handhaben mit den Programmen Elasticsearch, logstash, kibana, logspout

# Logging mit syslog
docker run -d --log-driver=syslog image
tail -f /var/log/syslog

# Logging-Datei für einen Container
/var/lib/docker/containers/id/id-json.log

# Docker-Events anzeigen
docker events

# Status-Anzeigen eines Containers (Ressourcenverbrauch)
docker stats container
# alle Container
docker stats $(docker ps -qa)

# cAdvisor - Monitoring-Tool für Docker
docker run -d --name mon -v /:/rootfs:ro -v /var/run:/var/run:rw -v /sys:/sys:ro -v /var/lib/docker:/var/lib/docker:ro -v /cgroups:/cgroups -p 8080:8080 google/cadvisor

# Cluster-Monitoring-Tools: Heapster, Prometheus
# Nachrichten-Dienste: PagerDuty, Pushover
# Metriken können z.B. per Python-Skript mit Prometheus-Client-Bibliothek hinzugefügt werden
```

## Vernetzung

* 2 Container innerhalb eines Hosts können miteinander verlinkt werden
* die Dienste können dann über Host:Port angesprochen werden

```
# Redis Server starten (-d Daemon), --name Containername
docker run --name redisserver -d redis

# Redis Client starten, mit Container redisserver verbinden und als redisd ansprechen
docker run --rm -it --link redisserver:redisd redis /bin/bash
redis-cli -h redisd -p 6379 # Redis-Client probieren
```

* 2 Container auf unterschiedl. Hosts miteinander verbinden
* Host1 (App-Container, Ambassador) <-> Service
* Host1 (App-Container, Ambassador) <-> Host2 (Ambassador, Service-Container) 
* Ambassador sind Proxy-Container (Relays)

```
# 2 Hosts erstellen (Virtualbox muss installiert sein)
docker-machine create -d virtualbox redis-server-host
docker-machine create -d virtualbox redis-client-host

# Umgebung für Docker-Client setzen, Host: redis-server-host setzen
eval $(docker-machine env redis-server-host)
# Service-Container in Host starten
docker run --name redisserver -d redis
# Ambassador-Container in Host starten
docker run -d --name redisserver-ambassador -p 6379:6379 --link redisserver:redisserver amouat/ambassador

# Umgebung für Docker-Client setzen, Host: redis-client-host setzen
eval $(docker-machine env redis-client-host)
# Ambassador-Container in Host starten
docker run -d --name redisclient-ambassador --expose 6379 -e REDIS_PORT_6379_TCP=tcp://$(docker-machine ip redis-server-host):6379 amouat/ambassador
# Client starten
docker run --rm -it --link redisclient-ambassador:redisclient-ambassador redis /bin/bash
redis-cli -h redisclient-ambassador -p 6379 # Redis-Client probieren
```

* Service-Discovery-Tools: etcd, Consul, SkyDNS, ZooKeeper, SmartStack, Eureka, WeaveDNS, docker-discover
* in Docker gibt es folgende Networking-Modi: Bridge, Host, Container, None
* mit --icc=false beim Daemon kann die Networking-Verbindung zwischen den Containern deaktiviert werden
* mit --icc=false, --iptables=true - nur verknüpfte Container haben eine Networking-Verbindung

* Docker-Networking (mit network und service ist link nicht mehr notwendig)
* Netzwerktreiber befinden sich in /usr/share/docker/plugins

```
# Netzwerke auflisten
docker network ls

# Dienste auflisten
docker service ls

# Netzwerk erstellen
docker network create mynet

# Netzwerk mit overlay erstellen
docker network create -d overlay net

# Netzwerkinfos
docker network info net

# Netzwerk erstellen
docker network create --subnet=172.20.0.0/16 mynet

# Container im neuen Netzwerk laufen lassen
docker run --net mynet --ip 172.20.0.10 -it nginx bash
```

* weave (https://www.weave.works/) ist eine einfache Netzwerklösung
* flannel ist eine Cross-Host-Netzwerklösung
* calico ist eine einfache Netzwerklösung

## Orchestrieren

* swarm - Clustering-Tool von docker
* fleet - Clustering-Tool von CoreOS
* Kubernetes - Orchestrierungstool von Google
* Mesos - Cluster Manager von Apache

## Verwaltung

* Rancher - Verwaltungs-Plattform
* Rancher Compose - Cli-Tool für docker-compose

```
# Rancher-Server starten
docker run -d -p 8080:8080 rancher/server
```

* Clocker - Container-Management-Tool
* Tutum - Container-Verwaltungs-Plattform


## Tools

* Distributionen für Container: Proxmox, CoreOS, RancherOS, Atomic Host
* Volume-Plugins: Flocker
* Orchestrieren: Kubernetes, Marathon, Fleet, Swarmkit
* Container verwalten MacOS, Windows: Kitematic
* Docker-Hosts im Gruppen zusammenfassen: Swarm
* Anwendung aus mehereren Docker-Containern bauen: compose
* installiert Docker-Hosts: Machine
* Aufsetzen von Container-Netzwerken: Weave, Calico, Overlay
* Docker-Container soll Services finden: SkyDNS, etcd, Registrator, Consul
* Dateien vertraulich behandeln: Notary
* Docker-Images verwalten: Docker Trusted Registry
* Container-Hosting-Möglichkeiten: Triton, Google Container Engine, Amazon EC2 Container Service, Giant Swarm
* quay.io - Alternative für Docker Hub 
* Key-Value-Stores (verschlüsselt) - KeyWhiz, Vault, Crypt

## Lizenz

Lizenz: https://creativecommons.org/publicdomain/zero/1.0/deed.de
