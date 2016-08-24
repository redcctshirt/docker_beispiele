# Docker - Beispiele

https://www.docker.com/

## Installation

### per Skript

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

## Lizenz

Lizenz: https://creativecommons.org/publicdomain/zero/1.0/deed.de
