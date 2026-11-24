# Sammelregel

Quellen: Spec 01 von `skill-improve` 0.1.0, *COLLECT*, *Fundorte*. MAA-Anja-AIBOS-Expert, Memos 154, 155, 162; Spec 05 von Anja, *Sammelregel*.

Diese Regel legt fest, wie `skill-improve` erfasste Wünsche in ein Konzeptprojekt mit Anja überträgt (COLLECT). Sie gilt für jedes Paket des Konzeptprojekts, nicht nur für Anja selbst. `improve` findet sie als Standardregel im Wissen des zugeordneten Assistenten; eine Regel `@/@Rule/rule-collect.md` im Konzeptprojekt geht ihr vor.

- **S1 — Ein Memo je Wunsch** im Memo-Ordner des Pakets: `@Memo/<paket>/`, wenn das Projekt mehrere Pakete hat, sonst `@Memo/`. Nummer: höchste im Ordner plus 1. Dateiname: `NN Wunsch — <Kurztitel>.md`.
- **S2 — Form nach dem Skill `memo`:** kein Frontmatter, Titel ohne Nummer, Kopf in `**Feld:**`-Zeilen: `**Datum:**` (Tag des Sammelns), `**Status:** Wunsch`, `**Autor:**` (`author` des Wunsches, leer: „unbekannt“), `**Betrifft:**` (`<package>` `<version>`), `**Quelle:**` (Eingang/Dateiname, erfasst am `<date>`, aus Projekt `<project>`), `**Verwandt:**` (Memos aus den Hinweisen, sonst —). Abschnitte: Wunsch, Beispiel, Abgrenzung, Einordnung.
- **S3 — Wörtlich übernehmen:** Wunsch, Beispiel und Abgrenzung unverändert.
- **S4 — Einordnung:** die Hinweise von `improve`, dazu, falls erkennbar, der betroffene Teil des Pakets. Entschieden wird beim Aufgreifen (Anjas Skill `anja-adopt`).
- **S5 — Ein Commit je Sammellauf:** „Sammeln: <n> Wünsche zu <paket> (Memos NN–MM)“, `<n>` gezählt.
- **S6 — Plan:** Gibt es `@Plan/Plan.md`, bekommt es im selben Commit unter *Offen* den Punkt „<n> gesammelte Wünsche aufgreifen (Memos NN–MM)“.
- **S7 — Melden:** Zahl, Memos, Hinweise, Commit.
