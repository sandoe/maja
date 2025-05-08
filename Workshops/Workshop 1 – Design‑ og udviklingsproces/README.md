# Workshop 1 – Design‑ og udviklingsproces (Solo‑opgave)

> **Mål:** At sparke udviklingen af **Elev i JYSK‑appen** i gang med en iterativ, brugercentreret metode (Design Thinking + Agile). Når du er færdig, har du et klart problemstatement, konceptskitser og en prioriteret backlog til dit første sprint.

---

## 📋 Forudsætninger

* Projektet er klonet, og installationen er gennemført jf. `installation.md`.
* Du arbejder **alene** på denne opgave.
* Du kender den overordnede idé: rekruttering & fastholdelse af elever til JYSK.

---

## 🛠️ Trin‑for‑trin

### 0 — Opsæt arbejdsområde (10 min)

1. Opret en ny **branch**:

   ```bash
   git checkout -b workshop1
   ```
2. Opret mappen `docs/` hvis den ikke findes.
3. Åbn dit foretrukne whiteboard‑værktøj (Miro/FigJam) eller brug papir & post‑its.

---

### 1 — Definér en målgruppe‑persona (20 min)

1. Brainstorm mulige elevtyper (fx 1.g‑studerende, FGU, sabbatår).
2. Vælg **én** persona der repræsenterer kernemålgruppen.
3. Udfyld en kort persona‑skabelon: navn, alder, mål, frustrationer, teknologivaner.
4. Gem som `docs/persona.pdf` eller billede.

---

### 2 — Empathise: Indsamling af brugerindsigt (30 min)

1. Forbered 3‑4 spørgsmål om motivation, hindringer og bekymringer ved elevpladser.
2. Interview **mindst én** medstuderende/ven (5‑10 min). Indhent samtykke til notetagning.
3. Notér **ordrette citater** og observationer (ingen tolkning endnu).
4. Gem noterne i `docs/empathise_notes.md`.

---

### 3 — Define: Problemstatement (15 min)

1. Kategorisér observationerne (motiver, pain points, behov).
2. Formuler en **HMW‑sætning** (How Might We) – fx:

   > *HMW hjælpe Jens (17) med hurtigt at finde en elevplads tæt på sig, så han sparer tid og føler sig tryg ved ansøgningsprocessen?*
3. Gem sætningen i `docs/problem_statement.md`.

---

### 4 — Ideate: Idégenerering (25 min)

1. **Crazy‑8s (solo‑version):** Fold et A4‑ark i otte felter og tegn 8 idéer på 8 min.
2. Vælg dine **top 3** idéer ved at markere dem med en stjerne.
3. Gem fotos eller skærmbilleder i `docs/ideation/`.

---

### 5 — Prototype: Low‑fi skitser (30 min)

1. Skitser flowet for din favoritidé (fx landing → søg butik → ansøg).
2. Brug papir, Figma wireframes eller Balsamiq.
3. Lav mindst **3 sammenhængende skærme**.
4. Eksportér til `docs/prototype_v1.pdf`.

---

### 6 — Test: Hurtig brugertest (20 min)

1. Lad en neutral peer klikke igennem (papir eller Figma prototype).
2. Bed dem **tænke højt** – ikke giv hints.
3. Notér hvad de forstår / misforstår.
4. Gem resultater i `docs/test_notes.md`.

---

### 7 — Refleksion & backlog (15 min)

1. Skriv de **tre vigtigste indsigter** fra testen.
2. Opret `backlog.md` med **User Stories** i følgende skabelon:

   ```
   Som <persona> vil jeg <mål>, så jeg <gevinst>.
   ```
3. Prioritér stories med MoSCoW (Must, Should, Could, Won’t).

---

## 📤 Aflevering

1. Commit alle filer i `docs/` og push til din fork eller central repo.
2. Opret et **Pull Request** til `main` med titel `Workshop 1 – Solo`.
3. Beskriv kort din HMW‑sætning, prototype og vigtigste indsigter.

---

## ✔️ Tjekliste før aflevering

* [ ] `docs/persona.pdf`
* [ ] `docs/problem_statement.md`
* [ ] `docs/ideation/` med fotos af idéer
* [ ] `docs/prototype_v1.pdf` (3+ skærme)
* [ ] `docs/test_notes.md`
* [ ] `backlog.md` med prioriterede User Stories

God arbejdslyst – næste gang dykker vi ned i GDPR og databeskyttelse! 🎉
