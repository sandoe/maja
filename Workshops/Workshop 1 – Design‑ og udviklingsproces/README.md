# 🚀 Workshop 1 – Docker-netværk og grundlæggende MariaDB

> **Mål:** Efter denne workshop kan du opsætte Docker-container med MariaDB, definere et netværk, og lægge et simpelt database-skema ind med testdata.

---

## 🧩 Trin 1: Opret SQL-mappe og nødvendige filer

1. Opret en mappe til SQL-filer:

```bash
mkdir -p sql
```

2. Opret filen `sql/create_schema.sql` med følgende indhold:

```sql
CREATE DATABASE IF NOT EXISTS jysk_workshop;
USE jysk_workshop;

CREATE TABLE student (
  student_id INT AUTO_INCREMENT PRIMARY KEY,
  full_name VARCHAR(255) NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL
);

CREATE TABLE store (
  store_id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(255),
  location VARCHAR(255)
);

CREATE TABLE application (
  application_id INT AUTO_INCREMENT PRIMARY KEY,
  student_id INT,
  store_id INT,
  applied_date DATE,
  FOREIGN KEY (student_id) REFERENCES student(student_id),
  FOREIGN KEY (store_id) REFERENCES store(store_id)
);
```

3. Opret filen `sql/sample_data.sql` med eksempelrækker:

```sql
USE jysk_workshop;

INSERT INTO student (full_name, email) VALUES
('Maja Madsen', 'maja@example.com'),
('Noah Nielsen', 'noah@example.com');

INSERT INTO store (name, location) VALUES
('JYSK Aarhus', 'Aarhus C'),
('JYSK Viborg', 'Viborg');

INSERT INTO application (student_id, store_id, applied_date) VALUES
(1, 1, CURDATE()),
(2, 2, CURDATE());
```

---

## 🧱 Trin 2: Tilføj netværk i docker-compose.yml

1. Åbn `docker-compose.yml`
2. Tilføj nederst i filen:

```yaml
networks:
  jysk-net:
    driver: bridge
```

3. Under `services -> mariadb`, tilføj:

```yaml
networks:
  - jysk-net
```

---

## 🔄 Trin 3: Genstart og seed databasen

1. Genstart MariaDB-containeren:

```bash
docker compose down
docker compose up -d mariadb
```

2. Kør seed-scripts:

```bash
docker compose exec mariadb sh -c "mysql -u root -p$MYSQL_ROOT_PASSWORD < sql/create_schema.sql"
docker compose exec mariadb sh -c "mysql -u root -p$MYSQL_ROOT_PASSWORD < sql/sample_data.sql"
```

---

## 🔎 Trin 4: Tjek at det virker

1. Log ind i databasen:

```bash
docker compose exec mariadb mariadb -u jysk -p$MYSQL_PASSWORD jysk_workshop
```

2. Prøv kommandoerne:

```sql
SHOW TABLES;
SELECT * FROM student;
SELECT * FROM application;
```

---

## ✅ Klar til Workshop 2!

Du har nu:

* Et Docker-netværk
* En MariaDB-container med normaliseret skema
* Eksempeldata du kan forespørge på

I næste workshop dykker vi ned i normalisering, relationer og optimeret skemadesign.
