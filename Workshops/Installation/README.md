# 🛠️ Installation

Denne vejledning dækker **Windows 10/11**, **Linux** (Debian/Ubuntu & Fedora‑familien), en **stand‑alone Docker Compose‑opsætning** samt **macOS** (Intel & Apple Silicon). Følg de trin, som passer til dit miljø.

---

## 1. Fælles forudsætninger

| Værktøj                             | Minimum‑version | Download / installér                                                                                             |
| ----------------------------------- | --------------- | ---------------------------------------------------------------------------------------------------------------- |
| **Git**                             | 2.x             | [https://git‑scm.com/downloads](https://git‑scm.com/downloads)                                                   |
| **Docker Engine / Docker Desktop**  | 24.x            | [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop) / distro‑pakker |
| **Docker Compose v2 CLI**           | 2.20+           | [https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)                             |
| **MariaDB‑klient**                  | 11.x            | [https://mariadb.org/download/](https://mariadb.org/download/)                                                   |
| **Kode‑editor** (VS Code anbefales) | —               | [https://code.visualstudio.com](https://code.visualstudio.com)                                                   |

> Hvis du ikke kan anvende Docker, kan du installere MariaDB lokalt – se afsnit 7.

---

## 2. Installation på **Windows 10/11**

### 2.1 Installér afhængigheder

1. **Git for Windows** – vælg *“Git from the command line and also from 3rd‑party software”*.
2. **Docker Desktop for Windows** – inkluderer både Docker Engine og Docker Compose v2; kræver WSL 2. Genstart efter installation.
3. **MariaDB Client** – vælg MSI‑pakken (kun klientdelen er nødvendig).

### 2.2 Klon projektet

```powershell
cd ~\Documents
git clone https://github.com/<DIN‑ORG>/elev‑i‑jysk‑workshop.git
cd elev‑i‑jysk‑workshop
```

### 2.3 Miljøvariabler

```powershell
copy .env.example .env
notepad .env  # rediger om ønsket
```

### 2.4 Start container og seed database

```powershell
docker compose up -d mariadb
docker compose exec mariadb sh -c "mysql -u root -p$Env:MYSQL_ROOT_PASSWORD < sql/create_schema.sql"
```

---

## 3. Installation på **Linux** (Debian/Ubuntu & Fedora/RHEL)

### 3.1 Pakker

```bash
# Debian/Ubuntu
sudo apt update
sudo apt install -y git docker.io docker-compose-plugin mariadb-client

# Fedora / RHEL
sudo dnf install -y git docker docker-compose-plugin mariadb
```

Tilføj din bruger til docker‑gruppen og start dæmonen:

```bash
sudo usermod -aG docker "$USER"
newgrp docker
sudo systemctl enable --now docker
```

### 3.2 Klon og kør

```bash
git clone https://github.com/<DIN-ORG>/elev-i-jysk-workshop.git
cd elev-i-jysk-workshop
cp .env.example .env
docker compose up -d mariadb
docker compose exec mariadb sh -c "mysql -u root -p$MYSQL_ROOT_PASSWORD < sql/create_schema.sql"
```

---

## 4. Stand‑alone **Docker Compose v2 CLI**

Brug denne metode, hvis du har Docker Engine uden Docker Desktop (fx headless Linux‑server):

```bash
mkdir -p ~/.docker/cli-plugins
curl -SL https://github.com/docker/compose/releases/download/v2.24.2/docker-compose-$(uname -s)-$(uname -m) -o ~/.docker/cli-plugins/docker-compose
chmod +x ~/.docker/cli-plugins/docker-compose

# Test
docker compose version
```

På nogle distributioner erstattes dette af pakken `docker-compose-plugin` (se afsnit 3).

---

## 5. Installation på **macOS**

### 5.1 Installér afhængigheder

* **Homebrew**: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
* `brew install git mariadb-client`
* **Docker Desktop for Mac** – henter både Engine og Compose.

### 5.2 Klon og kør

```bash
cd ~/Projects
git clone https://github.com/<DIN-ORG>/elev-i-jysk-workshop.git
cd elev-i-jysk-workshop
cp .env.example .env
docker compose up -d mariadb
docker compose exec mariadb sh -c "mysql -u root -p$MYSQL_ROOT_PASSWORD < sql/create_schema.sql"
```

---

## 6. Test forbindelsen

```bash
mariadb -h 127.0.0.1 -P 3306 -u jysk -p jysk_workshop
SHOW TABLES;  -- bør vise student, store, application
```

---

## 7. Alternativ: Lokal MariaDB (uden Docker)

1. Installér **MariaDB‑server** og **klient** via dit pakkesystem.
2. Opret database og bruger, der matcher `.env`.
3. Importér schema: `mariadb -u root -p jysk_workshop < sql/create_schema.sql`.

---

## 8. Oprydning

```bash
docker compose down -v
```

---

## 9. Fejlfinding

| Problem                                   | Løsning                                                                 |                 |
| ----------------------------------------- | ----------------------------------------------------------------------- | --------------- |
| **Port 3306 optaget**                     | Luk anden MariaDB/MySQL instans eller ændr port i `docker-compose.yml`. |                 |
| **docker compose: command not found**     | Installer Compose v2 CLI (afsnit 4) eller aktiver plugin‑pakken.        |                 |
| **Access denied ved seed**                | Kontrollér adgangskoder i `.env` og \`docker compose exec mariadb env   | grep MYSQL\_\`. |
| **Docker Desktop starter ikke (Windows)** | Aktiver **WSL 2** og sørg for virtualisering i BIOS.                    |                 |

> Opret et *Issue* i repoet, hvis du støder på andre problemer.
