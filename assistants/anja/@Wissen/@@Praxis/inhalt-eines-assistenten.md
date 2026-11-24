# Praxis — Inhalt eines Assistenten

Quellen: Spec v0.7.0 (bis v0.6.6 mit „aios“), 03 *Assistenten*, *Auflösung*; 02 *Klassen*, *Assistent der Session*. AA-AIbex-AIOS, V0.4, Memos 27, 70. Pakete `assistant-gaertner`, `assistant-astronom` 0.3.1. MAA-Anja-AIOS-Expert, Memos 113, 124; Spec 05 von Anja (Selbstverbesserung). MAS-Sandra-SDLC-Expert, Spec 04 *Rückfragen* (Q1–Q10).

## Aufbau im Paket

```
assistant-<name>/
├── manifest.yaml                      type: assistant, contents.assistants: [<name>]
├── README.md
└── assistants/<name>/
    ├── ASSISTANT.md                   Frontmatter und Rollentext
    ├── @!Skill/<skill>/SKILL.md       private Skills
    ├── @!Skill/<name>-gui/            Oberfläche (optional)
    └── @Wissen/
        ├── @Grundlagen/…              geladen, wenn der Assistent aktiv ist
        ├── @Content/…                 geladen: Inhaltsverzeichnis mit „Lesen, wenn …"
        └── @@<Thema>/…                verfügbar, gelesen bei Bedarf
```

Vorbilder: `assistant-gaertner` (mit Oberfläche und Wissen), `assistant-astronom` (schlank).

## Frontmatter

| # | Regel |
|---|---|
| A1 | Pflicht: `name` (gleich dem Ordner), `title`, `version`, `description` (eine Zeile) |
| A2 | `skills` nennt jeden Skill, den der Assistent braucht, auch die Oberfläche; `gui` muss in `skills` stehen. Ein nicht gefundener Skill ist ein Fehler |
| A3 | Skills werden zuerst im Assistentenordner gesucht, dann in der Kette; ein geteilter Skill (z. B. `memo`) muss im Projekt verfügbar sein oder als `depends` ins Manifest |
| A4 | `model` ist ein Wunsch, in der Desktop-App nur ein Hinweis; für genaues Arbeiten `opus`, für einfache Rollen `sonnet` |
| A5 | `tools` ist in v0.7 nur dokumentierend |

## Rollentext

Der Rollentext spricht den Assistenten mit **Du** an. Bewährte Gliederung (V0.4, Memo 70):

| Abschnitt | Inhalt |
|---|---|
| Einleitung | wer du bist, Ton, Sprache, Anrede; bei Demo-Assistenten: dient der Veranschaulichung |
| *Deine Personalität* | Antwort-Prefix (z. B. „👩‍🌾 ") und Anrede als Liste |
| *Dein Handwerk* | was der Assistent tut und mit welchem Skill; „halte dich genau an deinen Skill" |
| Fachabschnitt | woher das Wissen kommt, Quelle nennen, was bei Lücken zu sagen ist |
| *Deine Oberfläche* | nur wenn es eine gibt |
| *Deine Grenzen* | Praxis *Grenzen und Ehrlichkeit* |

## Wissen

| # | Regel |
|---|---|
| W1 | Was der Assistent immer braucht, kurz halten und laden; alles Weitere verfügbar, mit Inhaltsverzeichnis, das sagt, **wann** ein Dokument zu lesen ist |
| W2 | Daten, die mehrere Assistenten nutzen können, gehören in ein eigenes Wissenspaket ohne `depends`; der Assistent findet es über die Kette (Astronom und `knowledge-orion`) |
| W3 | Eigene Daten speichert ein Assistent in `@Data/assistant-<name>/` (Praxis *Daten einer App*) |

## Zu beachten

- Ein Assistent ist dem **Projekt** zugeordnet (`assistant:` in `.aibos/config.yaml`), nicht dem Chat; zum Ausprobieren ein Unterprojekt anlegen.
- Ein lokaler Assistent gleichen Namens in `@!Assistant/` verdeckt den aus dem Paket; `/aibos-context` meldet das. Bei mehreren Kopien entlang der Kette gewinnt die nächste (z. B. das `@!Package/` eines Testprojekts vor dem der Vault-Root).
- Private Skills, die `aibos` aufrufen, tragen den Satz zum Aufruf über den Weiterleiter (Praxis *Rückfragen*, R9).
- **Private Skills sind keine Slash-Befehle.** Sie werden durch Sätze ausgelöst, die in ihrer `description` stehen; der Rollentext und das README nennen diese Sätze, nicht `/<skill>`.
- Bringt ein Assistent Beispielskripte für Hooks mit, prüfst du sie vor dem Paket nach der Praxis *Testvorlagen* (Hook-Skripte, mindestens 15 Fälle).

## Rückfragen und Entscheidungen

Wie ein Assistent fragt, entscheidet in der Abnahme über die meisten Befunde: Bei Sandra 0.1 betrafen sechs von zehn Befunden das Fragen (MAA, Memo 113). Deshalb steht die Form der Fragen schon in der Skizze (Workflow, Phase 4) und danach in der Spec des Assistenten. Kopiere diesen Block in die Skizze und passe Namen und Beispiele an:

```markdown
## Rückfragen und Entscheidungen

| # | Regel |
|---|---|
| Q1 | <Name> nennt die offenen Punkte kurz als Liste und geht sie **Frage für Frage** durch. |
| Q2 | Jede Frage hat **mehrere Optionen**, jede mit ihrer Folge in einem Satz. |
| Q3 | Die **erste Option ist die Empfehlung**, im Label gekennzeichnet mit „(Empfohlen)“. Jede Frage hat eine Empfehlung. |
| Q4 | Wo nützlich, eine Option **„Diskutieren“**: erklären, Rückfragen beantworten, danach die Frage neu stellen. |
| Q5 | Die **letzte Option ist eine eigene Antwort** des Users (im Auswahlwerkzeug die freie Eingabe). |
| Q6 | Jede Frage mit Optionen stellt <Name> mit dem Auswahlwerkzeug, nie als Liste im Text; muss es erst geladen werden, lädt <Name> es zu Beginn (Praxis *Rückfragen*). Eine nummerierte Liste derselben Form nur, nachdem ein Aufruf abgelehnt wurde. Sagt der User „weiter“, gelten die Empfehlungen; <Name> nennt sie. |
| Q7 | Steht am Ende eines Schritts eine Entscheidung an (annehmen, korrigieren, ablehnen, nächster Schritt), legt <Name> sie als **Auswahl** derselben Form vor; die Option zur Annahme ist ausdrücklich formuliert („Ja, <Artefakt> ist angenommen“). |
| Q8 | **Jeder Schritt endet mit der nächsten Frage im Auswahlwerkzeug**, auch nach einer Ablehnung oder einer erklärenden Antwort; nie eine Aufforderung im Text wie „Sag, wenn du so weit bist“. |
| Q9 | Eine **Bestätigung je Entscheidung:** Wählt der User die Annahme, ist das zugleich das Ja zu Status und Commit; <Name> fragt nicht ein zweites Mal und nennt danach Änderung und Commit. |
| Q10 | Ist der Vault ein AIBOS-Workspace, bietet jede Annahme zwei Wege an: „Ja, … (Commit)“ (Empfohlen) und „Ja, … (Commit, Sync)“; in Testprojekten nur „(Commit)“. |
```

## Selbstverbesserung

Ein Assistent braucht für seine Verbesserung keine eigenen Skills. Wünsche erfasst, erprobt und sammelt der Skill `improve` aus dem Paket `skill-improve` (Spec 05 von Anja, *Vorlage für andere Assistenten*; MAA, Memo 155, U6). Ein Assistent `<name>` bekommt:

| Teil | Festlegung |
|---|---|
| Manifest | `depends: skill-improve: "~=0.1.0"` |
| Rollentext | in der Einleitung ein Absatz **Zu Beginn einer Session** nach dem Muster aus Anjas Rollentext, mit dem eigenen Paketnamen `assistant-<name>`: Wünsche im Postfach `<vault-root>/@/@!Improve/` zählen (`grep -l -x` auf `type: improvement` und `package: assistant-<name>`, über den vollen Pfad), bei 0 nichts sagen, sonst die Zahl nennen und `improve` gleich aufrufen; einmal je Session. In der Einleitung, weil eine Regel nur in Skills oder in der Praxis in normalen Antworten nicht wirkt (MAA, Memo 165) |
| Sammelregel | `@@Praxis/rule-collect.md` im eigenen Wissen nur, wenn der Assistent selbst Pakete entwickelt und Coach sein soll; dazu ein Skill zum Aufgreifen wie Anjas `anja-adopt` |
| nicht mehr | kein `<name>-improve`, kein `<name>-configure`, keine Inbox unter `@Data/` |

Ein Wunsch ändert den Assistenten nie von selbst: Im Probebetrieb (IMPROVED) gilt er nur für eine Session und ändert keine Datei; was dauerhaft gilt, entscheidet der Maintainer im Workflow.
