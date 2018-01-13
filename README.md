# Docker - Beispiele

https://www.docker.com/ - Website    
https://hub.docker.com/ - Images

* [Installation](#installation)
  * [bei Fedora - per Skript](#bei-fedora---per-skript)
  * [bei Fedora](#bei-fedora)
  * [docker-Gruppe erstellen](#docker-gruppe-erstellen)
* [Infos anzeigen](#infos-anzeigen)
* [Beispiele](#beispiele)
  * [Hallo Welt](#hallo-welt)
  * [Hallo Welt-Dienst starten](#hallo-welt-dienst-starten)
* [Kommandos](#kommandos)
* [Image erstellen](#image-erstellen)
* [Image mit Dockerfile erstellen](#image-mit-dockerfile-erstellen)
* [Beispiele 2](#beispiele-2)
  * [Redis Server und Client](#redis-server-und-client)
 
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

## Beispiele

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

# Container löschen, wenn er beendet wurde
docker run --rm hello-world
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

# Image erstellen
docker build -t repositoryname/imagename .

# Container mit neuem Image starten
docker run repositoryname/imagename 
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

## Beispiele 2

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


## Lizenz

Lizenz: https://creativecommons.org/publicdomain/zero/1.0/deed.de
