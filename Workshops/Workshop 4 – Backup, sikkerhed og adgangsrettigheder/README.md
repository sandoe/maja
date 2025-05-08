# 🔐 Workshop 4 – Backup, sikkerhed og adgangsrettigheder

> **Mål:** Du lærer at beskytte dine data ved hjælp af backup, brugerrettigheder og adgangskontrol i MariaDB. Du opretter en læsebruger og gennemfører en komplet backup og gendannelse.

---

## 🧪 Trin 1: Opret en ny databasebruger med begrænsede rettigheder

1. Log ind som root:

```bash
docker compose exec mariadb mariadb -u root -p$MYSQL_ROOT_PASSWORD
```

2. Opret en "analyst"‑bruger med **read-only** adgang:

```sql
CREATE USER 'analyst'@'%' IDENTIFIED BY 'readonlypass';
GRANT SELECT ON jysk_workshop.* TO 'analyst'@'%';
FLUSH PRIVILEGES;
```

3. Test at brugeren virker:

```bash
docker compose exec mariadb mariadb -u analyst -preadonlypass jysk_workshop
```

Prøv en forespørgsel som:

```sql
SELECT * FROM student;
```

Prøv derefter:

```sql
INSERT INTO student (full_name, email) VALUES ('Hack Test', 'hack@example.com');
-- Skal give fejl pga. manglende rettigheder
```

---

## 💾 Trin 2: Backup af databasen

1. Lav backup til fil:

```bash
docker compose exec mariadb sh -c "mysqldump -u root -p$MYSQL_ROOT_PASSWORD jysk_workshop > /backup/jysk_backup.sql"
```

2. For at hente backup ud på din vært (valgfrit):

```bash
docker cp $(docker compose ps -q mariadb):/backup/jysk_backup.sql ./jysk_backup.sql
```

> Du kan også montere en `volumes:`-mappe til `/backup` i `docker-compose.yml` hvis du vil have en fast sti.

---

## 🔁 Trin 3: Gendan fra backup (test)

1. Slet databasen (kun test!):

```sql
DROP DATABASE jysk_workshop;
```

2. Genindlæs backup:

```bash
docker compose exec mariadb sh -c "mysql -u root -p$MYSQL_ROOT_PASSWORD < /backup/jysk_backup.sql"
```

3. Test:

```bash
docker compose exec mariadb mariadb -u jysk -p$MYSQL_PASSWORD jysk_workshop
```

```sql
SHOW TABLES;
SELECT * FROM student;
```

---

## 🛡️ Trin 4: Overvej adgangsstrategier

Skriv en kort refleksion i `docs/security_policy.md` hvor du forklarer:

* Hvilke roller (fx `admin`, `analyst`) du ville oprette
* Hvilke rettigheder de skal have (SELECT, INSERT, DELETE...)
* Hvordan du vil håndtere backup og gendannelse i produktion

---

## ✅ Klar til Workshop 5!

Du har nu:

* En sikker read-only-bruger
* En testet backup og restore-flow
* Begyndende politik for roller og rettigheder
* Klar til at trække data ud via Node.js i næste workshop
