# 🟨 Workshop 5 – Node.js og datatilgængelighed

> **Mål:** Du lærer at forbinde til MariaDB fra et Node.js-script, udføre SQL-forespørgsler og vise resultater i terminalen.

---

## 🧩 Trin 1: Opret et projekt og installer afhængigheder

1. Opret ny mappe (hvis du ikke allerede er i et projekt):

```bash
mkdir node-access
cd node-access
npm init -y
```

2. Installer `mysql2` (eller `mariadb`):

```bash
npm install mysql2
```

---

## 📄 Trin 2: Opret en scriptfil

Opret filen `query.js` med følgende indhold:

```js
const mysql = require('mysql2');
require('dotenv').config();

const connection = mysql.createConnection({
  host: 'localhost',
  port: 3306,
  user: 'jysk',
  password: 'jyskpass',
  database: 'jysk_workshop'
});

connection.connect(err => {
  if (err) {
    console.error('Forbindelsesfejl:', err);
    return;
  }

  console.log('Forbundet til databasen!');

  const query = `
    SELECT s.full_name, st.name AS store, a.applied_date
    FROM application a
    JOIN student s ON a.student_id = s.student_id
    JOIN store st ON a.store_id = st.store_id;
  `;

  connection.query(query, (err, results) => {
    if (err) throw err;
    console.table(results);
    connection.end();
  });
});
```

> Husk at ændre credentials, hvis du har brugt en anden bruger eller adgangskode.

---

## 🔧 Trin 3: Test scriptet

1. Kør din MariaDB-container hvis den ikke kører:

```bash
docker compose up -d mariadb
```

2. Kør scriptet:

```bash
node query.js
```

> Du bør se en tabel med elever og deres ansøgninger i terminalen.

---

## 🧾 Trin 4: Refleksion og valgfri udvidelse

Skriv kort i `docs/node_summary.md`:

* Hvilken type data du har hentet
* Hvordan det kunne bruges i et webinterface, rapport eller dashboard

**Bonus**: Udvid `query.js` med:

* Brugerinput via `readline`
* Forespørgsel med `WHERE` fx på butik eller status

---

## ✅ Slut på forløbet!

Du har nu:

* Trukket data ud fra MariaDB med Node.js
* Forstået sammenhængen mellem SQL og JavaScript
* Grundlag for API- eller frontend-integrationer senere
