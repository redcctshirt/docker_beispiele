# Docker - Beispiele

https://www.docker.com/ - Website    
https://hub.docker.com/ - Images

* [Installation](#Installation)
* [Hallo Welt](#Hallo Welt)
* [Infos anzeigen](#Infos anzeigen)

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

## Hallo Welt

```
# Hallo Welt
docker run hello-world

# Programm cowsay mit dem Parameter "Hallo Welt" im Container docker/whalesay starten
docker run docker/whalesay cowsay Hallo Welt
```

## Infos anzeigen

```
# alle Container anzeigen
docker ps -a

# alle Images anzeigen
docker images

```


## Lizenz

Lizenz: https://creativecommons.org/publicdomain/zero/1.0/deed.de
