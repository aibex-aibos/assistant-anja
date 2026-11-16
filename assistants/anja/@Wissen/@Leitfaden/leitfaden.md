# Leitfaden für AIBOS-Pakete

Kurzfassung dessen, was bei jedem Paket gilt. Einzelheiten stehen in der Praxis (Inhaltsverzeichnis) und in der Spec.

## Spec lesen

| Spec (v0.7) | Inhalt | Lesen |
|---|---|---|
| 06 *Pakete und Workspaces* | `manifest.yaml`, Typen, Versionen und Bereiche, Hubs, Resolver, Paket laden, Ausrollen, Paketwechsel bei der Aktivierung, Distributionen, `@Data/`, Rückfluss | vor jedem Paket |
| 07 *Arbeitspferde* | Typ `worker`, Hash-Datei, Installation je Version, Weiterleiter | bei Arbeitspferden und Skills, die ein Programm brauchen |
| 03 *Skills und Assistenten* | Frontmatter von `SKILL.md` und `ASSISTANT.md`, Ausführungsmodell, Kernel-Skills im Vault (Hüllen), Auflösung, Zuordnung, Oberflächen, `/aibos-distribution` | bei Skills, Assistenten, Oberflächen |
| 02 *Master-Kontext* | Klassen `@`, `@@`, `@!` für Wissen; Block der Kernel-Skills; *Freigabe* (was der Hook ohne Rückfrage zulässt) | bei Wissen und bei jedem Skill, der `aibos` aufruft |
| 04 *Installation und Konfiguration* | CLI `aibos`, `config.yaml`, Installationen unter `~/.aibos/`, Weiterleiter, Umgebungsvariablen | bei Befehlen und Tests |
| 01 *Überblick* | Begriffe, Grundregeln, Invarianten I1–I14, Abnahmetest | bei Begriffsfragen |

**Einstieg** ist `doc/README.md` der installierten Version: Den Pfad nennt der Master-Kontext im Block `<aibos-kernel-skills>` (Zeile `Documentation of this version …`). Es sagt, welche Version installiert ist und welche Spec-Datei was regelt; danach liest du genau diese Datei. Die Spec liegt unter `~/.aibos/versions/v<x.y.z>/doc/spec/` (mit `aibos --install --system` unter `/opt/aibos/v<x.y.z>/doc/spec/`); andere Versionen über den Tag im Code-Repo (Rollentext, *Deine Quellen*).

## Begriffe

- **Paket:** versionierte Einheit mit `manifest.yaml`, ein Git-Repo in einem Hub, Version = Tag `v<version>`.
- **Element:** Skill, Assistent, Wissen oder Befehl in einem Paket.
- **Modul:** ein Paket, nach dessen Holen AIBOS etwas Neues kann (`kernel`, `app`, `assistant`, `skill`, `worker` oder mit Skills, Assistenten, Befehlen). `knowledge`, `tutorial`, `distribution` ohne solche Inhalte sind keine Module.
- **A/W** (AIBOS-Workspace): ein Vault, dessen Vault-Root an AIBOS ≥ 0.6.0 gebunden ist (`aibos:` in `<vault>/.aibos/config.yaml`); nur dort wirkt AIBOS.
- **Kernel-Skill**, **Hülle:** die Skills `aibos-*` des Kernels; im A/W als dünne Hülle an der Vault-Root und in jedem Projekt registriert und im Block `<aibos-kernel-skills>` des Master-Kontexts genannt (bis v0.5 *AIOS-Skill*, global registriert).
- **Weiterleiter:** `~/.aibos/bin/aibos` (für Arbeitspferde `~/.aibos/bin/<programm>`); steht in der Regel nicht im `PATH` und wird mit absolutem Pfad aufgerufen.
- **Arbeitspferd:** ein Programm, das ausgeführt wird, auf dem Rechner wohnt und vor Ort gebaut wird; Paket `type: worker` (Praxis *Arbeitspferde*).
- **Distribution:** optional ein Kernel und Pakete in exakten Versionen; wird installiert (Praxis *Kernel, Distributionen und Tutorials*).
- **Release-Kandidat:** Tag `v<x.y.z>-rc<n>` für Probe und Abnahme; `v<x.y.z>` erst nach der Abnahme (Praxis *Getaggte Stände*).
- **holen** (`aibos --fetch`, für den User „Paket laden" mit setup der Arbeitspferde) gilt für Pakete, **ausrollen** (`aibos --deploy`) für Elemente, **registrieren** für Skills; **installieren** für Kernel, Arbeitspferde und Distributionen.

## Regeln in Kürze

| Thema | Regel |
|---|---|
| Name | `<typ>-<name>`, englisch, gleich dem Typ: `app-`, `assistant-`, `skill-`, `knowledge-`, `tutorial-`, `distribution-`, `worker-`; Element `<name>`; Kernel `aibos` |
| Manifest | Pflicht `name`, `version`, `type`, `description`, `requires.aibos`; jedes Paket für v0.7 `">=0.7,<0.8"`; Elemente in `contents` |
| Versionen | SemVer, Start `0.1.0`; Elemente in `skills/` und `assistants/` tragen die Paketversion, private Skills ihre eigene |
| Tags | kein Tag wird verschoben; Probe und Abnahme auf Release-Kandidaten; jede Änderung, auch am README, ist eine neue Version |
| Paket-Repo | enthält nur, was beim Ausrollen nötig ist; Memos, Notizen, Testdaten gehören ins Konzeptprojekt. Ausnahme: `STATUS.md` mit dem Stand eines Implementierungsplans, solange er läuft (Workflow, Phase 7) |
| Rückfragen | Plan zeigen, fragen, dann ausführen; der Wunsch des Users ist keine Bestätigung des Plans |
| Aufruf von `aibos` | über den Weiterleiter mit dem absoluten Pfad aus dem Block `<aibos-kernel-skills>`, als einzelner Befehl ohne `cd`, `;`, `&&`, `|` oder Umleitung; jeder Skill, der `aibos` aufruft, sagt das (Praxis *Rückfragen*) |
| Werkzeuge | Fragen mit Optionen immer über das Auswahlwerkzeug, nie als Liste im Text, auch die Frage am Ende einer Antwort (nächster Schritt, Ja/Nein, „… oder …?“). Muss ein Werkzeug erst geladen werden (etwa das Auswahlwerkzeug), lädt der Assistent es zu Beginn (Praxis *Rückfragen*) |

## Konzeptprojekt und Paket-Repo

Ein **Konzeptprojekt** ist ein AIBOS-Projekt mit `@/`, dem du als Assistentin zugeordnet bist; es hält den Plan (`@Plan/Plan.md`), Memos, Entscheidungen, Testprojekte und die Vorgaben des Users. Ein Konzeptprojekt kann **viele Pakete** konzipieren; nur große Pakete bekommen ein eigenes. Das **Paket-Repo** ist ein eigenes Git-Repo ohne `@/`.

## Vorgaben

Die Vorgaben eines Users stehen in der Regel **„AIBOS-Pakete — Vorgaben"** (`name: aibos-pakete-vorgaben`, bis 0.8 auch `aios-pakete-vorgaben`) im Masterspace des Konzeptprojekts oder darüber; ist sie geladen, gilt sie. Sie nennt: Ort der Paket-Repos, Hub und Owner, Konzept-Repos, Sichtbarkeit, Standardlizenz, Sprache der Inhalte, Namensschema, Ort des Konzepts, Orte der Vaults (für den Bestandslauf vor einer Abnahme, Workflow Phase 8).

**Ohne Vorgaben:**

- Paket-Repos liegen in **`<konzeptprojekt>/@Package/<paket>/`** als eigenes Git-Repo; die `.gitignore` des Konzeptprojekts enthält `/@Package/`. Nenne den Ort und dass die Regel ihn ändern kann.
- `@Package/` (Quellen, die hier entstehen) ist nicht `@/@!Package/` (geholte Kopien).
- Hub, Owner und Lizenz fragst du, wenn sie gebraucht werden, spätestens vor dem Veröffentlichen; die Orte der Vaults vor dem Bestandslauf. Biete an, die Regel anzulegen.

## Ablauf eines Pakets

Der ganze Weg vom Konzept bis zum Abschluss steht in deinem Workflow. Die Tabelle zeigt die Arbeit am Paket-Repo selbst (Workflow, Phasen 7 und 8):

| Schritt | Ergebnis | Praxis |
|---|---|---|
| Entwurf | Typ, Name, Inhalt entschieden; Paket-Repo mit Manifest und Elementen | Namen und Arten; Manifest und Versionen; je Typ Inhalt eines Assistenten, Daten einer App, Oberflächen, Arbeitspferde, Kernel/Distribution/Tutorial |
| Prüfen | Manifest, Frontmatter, Versionen und Aufbau stimmen mit der Spec | Manifest und Versionen; Abhängigkeiten; Rückfragen |
| Probe | Release-Kandidat getaggt, in leerem Testprojekt geholt und ausprobiert | Probe vor Veröffentlichung; Getaggte Stände |
| Abnahme | Bestandslauf, Abnahmetest der Spezifikation bestanden, Befunde behoben | Entwicklungsprozess (P7); Testvorlagen |
| Veröffentlichen | Repo, Tag, Topic, Index, Probe gegen den Hub; Commits vor jedem Push gezeigt | Veröffentlichen; Rückfragen |
| Pflege | Beiträge übernommen, neue Version | Rückfluss; Getaggte Stände |
