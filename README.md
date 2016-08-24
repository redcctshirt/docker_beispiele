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

## Installation

### bei Fedora - per Skript

```
# Install-Skript herunterladen und ausführen
curl -fsSL https://get.docker.com/ | sh

# Dienst aktivieren 
systemctl enable docker.service

# Dienst starten
systemctl start docker

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

# Hallo Welt - Prüfen ob docker läuft
docker run hello-world
```

### docker-Gruppe erstellen

```
# Anwendung von docker ohne su oder sudo wird damit möglich gemacht
# Gruppe docker hinzufügen
groupadd docker

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
```

### Hallo Welt-Dienst starten

```
# -d im Hintergrund laufen lassen (als Daemon)
# im Container (Image: ubuntu) /bin/sh starten und Befehle ausführen
docker run -d ubuntu /bin/sh -c "while true; do echo Hallo Welt; sleep 1; done"

# alle laufenden Container anzeigen
docker ps

# Logging (stdout) vom Container anzeigen, Containername wird bei docker ps angezeigt
docker logs containername

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
```

## Lizenz

Lizenz: https://creativecommons.org/publicdomain/zero/1.0/deed.de
