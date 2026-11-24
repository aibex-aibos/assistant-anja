---
name: anja-anlegen
version: 0.6.0
description: >
  Legt ein neues Paket-Repo für AIbex AIBOS an, für jeden Typ (app, assistant, skill, knowledge,
  tutorial, distribution, worker, kernel): Typ und Name klären, Ort nach den Vorgaben des Users,
  Vorlage aus einem bestehenden Paket, Manifest, Lizenz, lokaler Commit. Verwenden bei
  "neues Paket", "Paket anlegen", "Modul anlegen", "Repo für … anlegen".
---

# Paket anlegen

Ein Skill der Assistentin Anja. Er legt ein Paket-Repo **lokal** an; nichts geht nach außen.

**Arbeitsweise:** Arbeite mit absoluten Pfaden und `git -C <repo>`, wechsle nie mit `cd`. Rufe `aibos` über den Weiterleiter auf: mit dem absoluten Pfad aus dem Block `<aibos-kernel-skills>` deines Master-Kontexts, als einzelnen Befehl ohne `cd`, `;`, `&&`, `|` oder Umleitung.

**Fragen:** Jede Frage mit Optionen stellst du mit dem Auswahlwerkzeug, nie als Liste im Text. In Claude Code lädst du es zu Beginn mit `ToolSearch` und `select:AskUserQuestion`, noch bevor du die erste Frage stellst, und rufst es dann für jede Frage mit Optionen auf. Eine nummerierte Liste derselben Form gibt es nur, nachdem ein Aufruf abgelehnt wurde (etwa headless) (`@@Praxis/rueckfragen.md`, R10).

## 1. Klären

Frage, was noch offen ist, in einem kleinen Fragepaket mit Empfehlung:

| Punkt | Hilfe |
|---|---|
| Typ | `app`, `assistant`, `skill`, `knowledge`, `tutorial`, `distribution`, `worker`, `kernel`; Begriffe im Leitfaden. Braucht ein Paket ein Programm (z. B. `yt-dlp`), ist das ein eigenes Paket `worker-<name>` (`@@Praxis/arbeitspferde.md`) |
| Name | `<typ>-<name>`, Element `<name>`, beim Assistenten ein Titel; lies `@@Praxis/namen-und-arten.md` |
| Zweck | ein Satz für `description` |
| AIBOS-Version | Standard: die letzte freigegebene Spec; `requires.aibos: ">=0.7,<0.8"` für jeden Typ (`@@Praxis/manifest-und-versionen.md`, M2) |

## 2. Tor — ist die Spezifikation freigegeben?

Nach deinem Workflow wird erst nach der Freigabe der Spezifikation gebaut (Phasen 6, 7). Sieh in den Memos des Pakets nach (`@Memo/<paket>/`, sonst `@Memo/`), ob es ein Memo der Freigabe gibt.

- **Ja:** weiter mit Schritt 3.
- **Nein:** Nenne die Phase, in der das Paket steht, und frage, ob trotzdem angelegt werden soll, etwa in der verkürzten Form oder als Gerüst für eine Probe. **Ohne Ja legst du nichts an**; schlage stattdessen den nächsten Schritt des Workflows vor.

## 3. Ort und Vorgaben

- Ist die Regel *AIBOS-Pakete — Vorgaben* (`name: aibos-pakete-vorgaben`, bis 0.8 auch `aios-pakete-vorgaben`) geladen, gilt ihr Ort, ihre Lizenz und ihr Namensschema. Eine andere geladene Regel, die den Ort von Paket-Repos nennt, gilt ebenso.
- Sonst gilt der Default: `<konzeptprojekt>/@Package/<paket>/`, und die `.gitignore` des Konzeptprojekts bekommt `/@Package/`. Sag das und dass die Regel den Ort ändern kann.
- Gibt es den Zielordner schon, brich ab und sag es.

## 4. Vorlage

Nimm ein bestehendes Paket desselben Typs als Vorlage, getaggter Stand, ohne `.git`:

| Typ | Vorlage |
|---|---|
| `assistant` | `assistant-gaertner` (mit Oberfläche und Wissen) oder `assistant-astronom` (schlank) |
| `skill` | `skill-memo` |
| `knowledge` | `knowledge-orion` |
| `app` | `app-taschenrechner` |
| `kernel` | `aibex-aibos` |
| `worker` | das Beispiel `worker-yt-dlp` in `@@Praxis/arbeitspferde.md` (Manifest, `requirements.in`, Hash-Datei mit `pip-compile --generate-hashes --strip-extras`) |
| `distribution` | der Kernel-Skill `/aibos-distribution`, der vom erprobten Stand eines Projekts ausgeht (`@@Praxis/kernel-distribution-tutorial.md`) |
| `tutorial` | keine; nur `manifest.yaml`, `README.md` und `knowledge/` (`@@Praxis/kernel-distribution-tutorial.md`) |

Woher: ein lokaler Klon (z. B. `~/git/<owner>/<repo>`, nach den Vorgaben), sonst ein flacher Klon des Tags in ein temporäres Verzeichnis: `git clone --depth 1 --branch v<version> https://github.com/aibex-aibos/<repo> <tmp>`.

Übernimm nur den **Aufbau**: Ordner, Frontmatter-Felder, Abschnitte des Rollentexts, `gui.yaml`-Gerüst. Leere die Inhalte und ersetze Namen, Titel, Beschreibung. Übernimm nie `contrib/`, Daten oder Wissen der Vorlage.

## 5. Plan zeigen und fragen

Zeige vor dem Schreiben: Zielordner, Dateien mit Kurzinhalt, `manifest.yaml` vollständig, Lizenzdateien, Eintrag in `.gitignore` (falls Default). Schreibe erst nach Ja (`@@Praxis/rueckfragen.md`).

## 6. Anlegen

1. Ordner anlegen, `git init -b main`.
2. `manifest.yaml` nach `@@Praxis/manifest-und-versionen.md`: `version: 0.1.0`, `contents` passend zu den Ordnern; Elemente in `skills/` und `assistants/` mit `version: 0.1.0`. Bei `worker`: kein `contents`, sondern `worker:` mit `kind`, `programs`, `check` und `requirements` (`@@Praxis/arbeitspferde.md`); die Hash-Datei erzeugst du mit Rückfrage, weil `pip-compile` aus dem Netz liest.
3. Lizenz nach den Vorgaben. Bei Apache-2.0: `license: Apache-2.0`, `LICENSE` als unveränderte Kopie (z. B. aus `@!Package/assistant-anja/LICENSE`), `NOTICE` mit Paketname, Copyright-Zeile und, falls vorgegeben, dem Satz zu den Marken. Ohne Vorgabe: Lizenz offen lassen und vor dem Veröffentlichen fragen.
4. `README.md`: Zweck, Voraussetzung (AIBOS v0.7, Vault gebunden), `aibos --fetch <paket>`, Aufruf bzw. Zuordnung, Lizenz. Ruft ein Skill des neuen Pakets `aibos` auf, bekommt er den Satz zum Aufruf über den Weiterleiter (`@@Praxis/rueckfragen.md`, R9).
5. Beim Default: `/@Package/` in die `.gitignore` des Konzeptprojekts eintragen und dort nur diese Zeile committen.
6. Im Paket-Repo committen: „<paket> 0.1.0: angelegt aus <vorlage>". **Kein Tag**; getaggt wird erst zur Probe, als Release-Kandidat `v0.1.0-rc1`.

## 7. Melden

Nenne Ort, Dateien und den nächsten Schritt: Inhalte schreiben, dann das Paket prüfen (dein Skill `anja-pruefen`).

## 8. `STATUS.md`

Ist für das Paket ein **Implementierungsplan freigegeben** (Workflow, Phase 7), legst du im Paket-Repo `STATUS.md` an: Titel mit Paket und Version, ein Satz mit dem Memo des Plans und seiner Freigabe, dann eine Tabelle mit den Schritten des Plans (`#`, Schritt, Stand ☐, Ergebnis leer). Zeige die Datei vorher und frage wie in Schritt 5; nach Ja committen. In jedem Commit eines Schritts führst du sie nach (☑ und Ergebnis); im Tag eines Release-Kandidaten sind alle Schritte ☑. In der verkürzten Form gibt es kein `STATUS.md`. Das gilt auch, wenn das Paket-Repo schon besteht und nur eine neue Version entsteht.
