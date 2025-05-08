👋 **Velkommen til Elev i JYSK – Netværk & Databaseforløb**

---

## 🌟 Introduktion

Dette forløb fokuserer udelukkende på **netværk** og **relationsdatabaser**. Du lærer at konfigurere Docker‑netværk, opstille en robust **MariaDB**‑backend og arbejde med normaliserede skemaer, SQL‑forespørgsler og dataudtræk gennem et simpelt Node.js‑script.

Alle workshops er hands‑on og bygger oven på hinanden, så du opbygger en fuldt fungerende og sikker database­løsning.

---

## 📆 Workshopstruktur

| Nr. | Titel                                                    | Fokus                                                          |
| --- | -------------------------------------------------------- | -------------------------------------------------------------- |
| 1   | **Workshop 1 – Docker‑netværk & Grund­læggende MariaDB** | Bridge‑netværk, start container, seed med eksempeldata         |
| 2   | **Workshop 2 – Datamodellering & Normalisering**         | Design af 3NF‑skemaer, primær‑/fremmednøgler, ER‑diagram       |
| 3   | **Workshop 3 – SQL‑forespørgsler & Statistik**           | CRUD, JOINs, GROUP BY, aggregering, visninger og indeks        |
| 4   | **Workshop 4 – Backup, Sikkerhed & Rettigheder**         | Adgangsrettigheder, dump/restore, sikkerhed for brugerdata     |
| 5   | **Workshop 5 – Node.js & Datatilgængelighed**            | Tilslutning med Node.js, forespørgsler, dataprint i terminalen |

---

## 🧰 Krav til værktøjer

* Docker + Docker Compose (24+)
* Git (valgfrit)
* MariaDB Client (11+)
* Node.js (18+ anbefales)
* Editor som VS Code

---

## 🔧 Node.js‑integration

I den afsluttende workshop bygger du et **enkelt Node.js‑script**, der forbinder til MariaDB og henter data.

Scriptet skal:

* Koble op til databasen vha. `mysql2` eller `mariadb` npm‑pakke
* Hente fx alle ansøgninger fra `application`‑tabellen
* Udskrive data overskueligt i terminalen

---

## 📚 Kompetencer du opbygger

* Forståelse for Docker‑netværk og container‑opsætning
* Oprettelse og normalisering af relationelle skemaer i MariaDB
* Skrivning af SQL‑forespørgsler og dataanalyse
* Backup og adgangsstyring i databasekontekst
* Praktisk anvendelse af databaseforbindelser i Node.js

---

## 📜 Licens

Udgivet under MIT‑licensen – se `LICENSE` for detaljer.
