# 📊 Workshop 3 – SQL-forespørgsler og statistik

> **Mål:** Du lærer at skrive effektive SQL-forespørgsler med JOINs, GROUP BY og filtrering. Du analyserer data i din MariaDB-database og opbygger grundlag for simple rapporter og statistik.

---

## 🧠 Trin 1: Lav en ny SQL-fil til forespørgsler

Opret filen `sql/queries.sql` og tilføj følgende:

### A) Alle ansøgninger med elevnavn og butik

```sql
SELECT a.application_id, s.full_name, st.name AS store_name, a.applied_date
FROM application a
JOIN student s ON a.student_id = s.student_id
JOIN store st ON a.store_id = st.store_id;
```

### B) Antal ansøgninger pr. butik

```sql
SELECT st.name, COUNT(*) AS application_count
FROM application a
JOIN store st ON a.store_id = st.store_id
GROUP BY st.name;
```

### C) Ansøgninger fordelt på status

```sql
SELECT st.label AS status, COUNT(*) AS total
FROM application a
JOIN status st ON a.status_id = st.status_id
GROUP BY st.label;
```

### D) Elever uden ansøgninger

```sql
SELECT s.full_name
FROM student s
LEFT JOIN application a ON s.student_id = a.student_id
WHERE a.application_id IS NULL;
```

### E) Valgfri bonusforespørgsel (skriv selv)

```sql
-- Fx: antal ansøgninger pr. uddannelsestype
SELECT e.name AS education, COUNT(*) AS total
FROM application a
JOIN student s ON a.student_id = s.student_id
JOIN education e ON s.education_id = e.education_id
GROUP BY e.name;
```

---

## 🧪 Trin 2: Kør forespørgslerne

1. Log ind på databasen:

```bash
docker compose exec mariadb mariadb -u jysk -p$MYSQL_PASSWORD jysk_workshop
```

2. Kør forespørgslerne direkte eller brug:

```bash
docker compose exec mariadb mariadb -u jysk -p$MYSQL_PASSWORD jysk_workshop < sql/queries.sql
```

---

## 🧾 Trin 3: Dokumentér resultater (valgfrit)

Opret en ny markdown-fil `docs/query_insights.md` hvor du:

* Forklarer kort hvad hver forespørgsel gør
* Noterer mindst én indsigt du kan drage fra dataene
* Fx: "Viborg har modtaget flest ansøgninger" eller "2 elever har ingen ansøgninger"

---

## 🧰 Klar til Workshop 4!

Du har nu:

* Skrevet og testet 5+ SQL-forespørgsler
* Fået styr på GROUP BY og LEFT JOIN
* Bygget et fundament til videre statistik, adgangsbegrænsning og sikkerhed
