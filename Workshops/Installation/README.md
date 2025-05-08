# 🛠️ Installation

Denne vejledning hjælper dig med at installere alle nødvendige værktøjer til at arbejde med projektet **Elev i JYSK – Netværk og Database**. Installationen dækker de mest brugte platforme: **Windows 10/11**, **Linux** (Debian/Ubuntu & Fedora‑familien), **macOS** (Intel og Apple Silicon), samt en selvstændig installation af **Docker Compose v2 CLI** til servermiljøer. Du vil også få vejledning i at teste og rydde op i din installation.

---

## 1. Fælles forudsætninger

Før du kan komme i gang med at bruge og udvikle på projektet, skal følgende værktøjer være installeret på din maskine:

| Værktøj                             | Minimum‑version | Download / installér                                                                                                |
| ----------------------------------- | --------------- | ------------------------------------------------------------------------------------------------------------------- |
| **Git**                             | 2.x             | [git-scm.com/downloads](https://git-scm.com/downloads)                                                              |
| **Docker Engine / Docker Desktop**  | 24.x            | [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop) eller distro‑specifikke pakker |
| **Docker Compose v2 CLI**           | 2.20+           | [docs.docker.com/compose/install](https://docs.docker.com/compose/install/)                                         |
| **MariaDB‑klient**                  | 11.x            | [mariadb.org/download](https://mariadb.org/download/)                                                               |
| **Node.js**                         | 18.x anbefales  | [nodejs.org](https://nodejs.org/en/download)                                                                        |
| **Kode‑editor** (VS Code anbefales) | —               | [code.visualstudio.com](https://code.visualstudio.com)                                                              |

> Hvis du ikke kan anvende Docker, kan du installere MariaDB lokalt – se afsnit 7.

---

## 2. Installation på **Windows 10/11**

### 2.1 Installér afhængigheder

1. Installer **Git for Windows** og vælg under installationen "Git from the command line and also from 3rd‑party software".
2. Installer **Docker Desktop for Windows**. Programmet installerer både Docker Engine og Compose v2. Det kræver WSL 2 og genstart af systemet.
3. Installer **MariaDB Client** – vælg MSI‑pakken og kun klientdelen.
4. Installer **Node.js** fra den officielle hjemmeside.
5. Installer **Visual Studio Code** hvis du ønsker en anbefalet editor.

### 2.2 Klon projektet og konfigurer miljøvariabler

```powershell
cd ~\Documents
git clone https://github.com/<DIN‑ORG>/elev‑i‑jysk‑workshop.git
cd elev‑i‑jysk‑workshop
copy .env.example .env
notepad .env
```

### 2.3 Start Docker-container og seed database

```powershell
docker compose up -d mariadb
docker compose exec mariadb sh -c "mysql -u root -p$Env:MYSQL_ROOT_PASSWORD < sql/create_schema.sql"
```

---

## 3. Installation på **Linux** (Debian/Ubuntu & Fedora/RHEL)

### 3.1 Installér nødvendige pakker

```bash
# Debian/Ubuntu
sudo apt update
sudo apt install -y git docker.io docker-compose-plugin mariadb-client nodejs npm

# Fedora/RHEL
sudo dnf install -y git docker docker-compose-plugin mariadb nodejs npm
```

### 3.2 Giv Docker-rettigheder og start

```bash
sudo usermod -aG docker "$USER"
newgrp docker
sudo systemctl enable --now docker
```

### 3.3 Klon projektet og kør det

```bash
git clone https://github.com/<DIN-ORG>/elev-i-jysk-workshop.git
cd elev-i-jysk-workshop
cp .env.example .env
docker compose up -d mariadb
docker compose exec mariadb sh -c "mysql -u root -p$MYSQL_ROOT_PASSWORD < sql/create_schema.sql"
```

---

## 4. Stand‑alone **Docker Compose v2 CLI** (headless server)

Hvis du ikke anvender Docker Desktop, skal du installere CLI direkte:

```bash
mkdir -p ~/.docker/cli-plugins
curl -SL https://github.com/docker/compose/releases/download/v2.24.2/docker-compose-$(uname -s)-$(uname -m) -o ~/.docker/cli-plugins/docker-compose
chmod +x ~/.docker/cli-plugins/docker-compose
docker compose version
```

På nogle systemer er dette allerede dækket af pakken `docker-compose-plugin`.

---

## 5. Installation på **macOS**

### 5.1 Installér afhængigheder via Homebrew

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install git mariadb-client node
```

Installer **Docker Desktop for Mac**, som inkluderer både Docker Engine og Docker Compose.

### 5.2 Klon projektet og start det

```bash
cd ~/Projects
git clone https://github.com/<DIN-ORG>/elev-i-jysk-workshop.git
cd elev-i-jysk-workshop
cp .env.example .env
docker compose up -d mariadb
docker compose exec mariadb sh -c "mysql -u root -p$MYSQL_ROOT_PASSWORD < sql/create_schema.sql"
```

---

## 6. Test forbindelsen til databasen

```bash
mariadb -h 127.0.0.1 -P 3306 -u jysk -p jysk_workshop
SHOW TABLES;  -- du bør se tabellerne: student, store, application
```

---

## 7. Alternativ: Lokal MariaDB (uden Docker)

1. Installér MariaDB-server og -klient.
2. Opret database og bruger, der matcher værdierne i `.env`.
3. Importér skema:

```bash
mariadb -u root -p jysk_workshop < sql/create_schema.sql
```

---

## 8. Oprydning og stop

Når du vil lukke ned og rydde op efter dig:

```bash
docker compose down -v
```

Dette stopper og fjerner containeren, netværket og tilknyttede volumes.

---

## 9. Fejlfinding

| Problem                                   | Løsning                                                                               |                |
| ----------------------------------------- | ------------------------------------------------------------------------------------- | -------------- |
| **Port 3306 er optaget**                  | Luk anden MySQL/MariaDB instans eller brug en alternativ port i `docker-compose.yml`. |                |
| **docker compose: command not found**     | Installer Docker Compose CLI manuelt (se afsnit 4).                                   |                |
| **Access denied ved import af database**  | Kontrollér dine `.env`-oplysninger og brug \`docker compose exec mariadb env          | grep MYSQL\_\` |
| **Docker Desktop starter ikke (Windows)** | Kontrollér om WSL2 er installeret og at virtualisering er aktiveret i BIOS.           |                |

> Er du i tvivl om noget? Opret et *Issue* i GitHub-repoet eller kontakt din underviser for support.
