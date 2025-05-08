# 🧠 Workshop 2 – Datamodellering og normalisering

> **Mål:** Efter denne workshop kan du designe en relationsdatabase i 3. normalform (3NF), analysere eksisterende tabeller for redundans og optimere dine datastrukturer med relationer og nøgler.

---

## 🧩 Trin 1: Analyser det eksisterende skema

Åbn filen `sql/create_schema.sql` fra Workshop 1.

1. Kig på tabellerne `student`, `store` og `application`:

   * Indeholder nogen kolonner **gentagne oplysninger** (redundans)?
   * Findes der værdier, der kunne deles op i egne tabeller (f.eks. `location` eller `email-domain`)?

2. Skriv en kort analyse i filen `docs/normalisation_analysis.md`:

   * Er skemaet i 1NF, 2NF, 3NF?
   * Begrunde hvorfor / hvorfor ikke.

---

## 📐 Trin 2: Udvid skemaet med flere relationer

Tilføj nye tabeller i `sql/create_schema.sql`:

### A) Uddannelsesretning

```sql
CREATE TABLE education (
  education_id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100)
);
```

Tilføj kolonne til `student`:

```sql
ALTER TABLE student ADD education_id INT;
ALTER TABLE student ADD FOREIGN KEY (education_id) REFERENCES education(education_id);
```

### B) Ansøgningsstatus (lookup‑tabel)

```sql
CREATE TABLE status (
  status_id INT AUTO_INCREMENT PRIMARY KEY,
  label VARCHAR(50)
);
```

Tilføj kolonne til `application`:

```sql
ALTER TABLE application ADD status_id INT;
ALTER TABLE application ADD FOREIGN KEY (status_id) REFERENCES status(status_id);
```

> Disse ændringer reducerer redundans og gør systemet lettere at vedligeholde.

---

## 🧪 Trin 3: Tilføj testdata til nye tabeller

Rediger `sql/sample_data.sql`:

```sql
INSERT INTO education (name) VALUES ('HTX'), ('EUD'), ('STX');
INSERT INTO status (label) VALUES ('Sendt'), ('Afventer'), ('Afslået'), ('Optaget');

UPDATE student SET education_id = 1 WHERE student_id = 1;
UPDATE student SET education_id = 2 WHERE student_id = 2;

UPDATE application SET status_id = 2 WHERE application_id = 1;
UPDATE application SET status_id = 4 WHERE application_id = 2;
```

---

## 🔄 Trin 4: Genindlæs database og test

```bash
# Stop og start container
docker compose down
docker compose up -d mariadb

# Kør opdateret schema og data
docker compose exec mariadb sh -c "mysql -u root -p$MYSQL_ROOT_PASSWORD < sql/create_schema.sql"
docker compose exec mariadb sh -c "mysql -u root -p$MYSQL_ROOT_PASSWORD < sql/sample_data.sql"
```

Log ind og test:

```bash
docker compose exec mariadb mariadb -u jysk -p$MYSQL_PASSWORD jysk_workshop
```

```sql
SELECT s.full_name, e.name AS education FROM student s JOIN education e ON s.education_id = e.education_id;
SELECT a.application_id, st.label FROM application a JOIN status st ON a.status_id = st.status_id;
```

---

## ✅ Klar til Workshop 3!

Nu har du:

* Normaliseret skema i 3NF
* Lookup-tabeller og relationer
* Et solidt grundlag til at arbejde videre med forespørgsler og statistik i næste workshop
