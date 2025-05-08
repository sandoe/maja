# 🛠️ Installation – uden brug af Git

Denne vejledning hjælper dig med at installere og konfigurere **Elev i JYSK – Netværk og Database** uden brug af Git. Du lærer at sætte systemet op med Docker Compose, oprette en MariaDB-container og strukturere filerne korrekt. Perfekt til lokal undervisning, testmiljøer eller zip-overførsel.

---

## 1. Fælles forudsætninger

| Værktøj                             | Minimum‑version | Download / installér                                                                                                |
| ----------------------------------- | --------------- | ------------------------------------------------------------------------------------------------------------------- |
| **Docker Engine / Docker Desktop**  | 24.x            | [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop) eller distro‑specifikke pakker |
| **Docker Compose v2 CLI**           | 2.20+           | [docs.docker.com/compose/install](https://docs.docker.com/compose/install/)                                         |
| **MariaDB‑klient**                  | 11.x            | [mariadb.org/download](https://mariadb.org/download/)                                                               |
| **Node.js**                         | 18.x anbefales  | [nodejs.org](https://nodejs.org/en/download)                                                                        |
| **Kode‑editor** (VS Code anbefales) | —               | [code.visualstudio.com](https://code.visualstudio.com)                                                              |

> Du skal også bruge projektmappen, som du kan hente som .zip eller få via USB/netværksdrev.

---

## 2. Udpak og opbyg projektstrukturen

1. Hent eller modtag mappen `elev-i-jysk-workshop.zip`
2. Udpak til en valgfri placering, fx:

```bash
~/elev-i-jysk-workshop
```

3. Sørg for, at mappen indeholder:

   * `docker-compose.yml`
   * `.env.example` (eller lav en ny selv – se næste trin)
   * `sql/create_schema.sql` og `sql/sample_data.sql`
   * Eventuelt `query.js` og `package.json`

---

## 3. Opret `docker-compose.yml`

Opret filen i roden af projektmappen hvis den ikke findes:

```yaml
version: '3.9'

services:
  mariadb:
    image: mariadb:11
    container_name: jysk-mariadb
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      MYSQL_DATABASE: ${MYSQL_DATABASE}
      MYSQL_USER: ${MYSQL_USER}
      MYSQL_PASSWORD: ${MYSQL_PASSWORD}
    ports:
      - "3306:3306"
    volumes:
      - ./sql:/docker-entrypoint-initdb.d
    networks:
      - jysk-net

networks:
  jysk-net:
    driver: bridge
```

---

## 4. Opret .env-fil manuelt

Hvis `.env.example` **ikke findes**, så opret filen selv:

```bash
touch .env
nano .env
```

Indsæt følgende i filen:

```env
MYSQL_ROOT_PASSWORD=hemmelig
MYSQL_DATABASE=jysk_workshop
MYSQL_USER=jysk
MYSQL_PASSWORD=jyskpass
```

Gem og luk filen (i nano: CTRL + O, ENTER, CTRL + X).

---

## 5. Start databasen og seed schema

1. Start container:

```bash
docker-compose up -d mariadb
```

2. Containeren vil automatisk importere `sql/create_schema.sql` og `sql/sample_data.sql` ved første opstart.

3. Bekræft forbindelsen:

```bash
mariadb -h 127.0.0.1 -P 3306 -u jysk -p jysk_workshop
```

---

## 6. Kør Node.js-script

1. Gå til projektmappe med `query.js`:

```bash
cd node-access  # eller ./ hvis det ligger i roden
```

2. Installer afhængigheder:

```bash
npm install
```

3. Kør scriptet:

```bash
node query.js
```

---

## 7. Stop og oprydning

```bash
docker compose down -v
```

Dette stopper og fjerner databasen, netværket og mapper.

---

> Nu er du klar til at bruge og udvikle videre – helt uden Git!
