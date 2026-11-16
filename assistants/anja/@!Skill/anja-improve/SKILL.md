---
name: anja-improve
version: 0.1.0
description: >
  Selbstverbesserung der Assistentin Anja: nimmt einen Verbesserungsvorschlag eines Anwenders
  auf und legt ihn als Wunsch in die Inbox @Data/assistant-anja/@Inbox/ an der Vault-Root;
  zeigt dem Maintainer die Wünsche aller anerkannten Inboxen und übernimmt sie als Memo oder
  lehnt sie ab. Verwenden bei "Ich hab einen Verbesserungsvorschlag für dich",
  "Verbesserungsvorschlag", "Wunsch an Anja", "Zeig die Wünsche", "Welche Wünsche gibt es?".
---

# Verbesserung

Ein privater Skill der Assistentin Anja. Er hat zwei Wege: **Weg 1** setzt einen Wunsch ab (für jeden, der Anja benutzt), **Weg 2** zeigt und übernimmt Wünsche (für den Maintainer). Welcher Weg gilt, sagt der Satz des Users; im Zweifel fragst du.

**Arbeitsweise:** Arbeite mit absoluten Pfaden und `git -C`, wechsle nie mit `cd`. Du schreibst nichts ohne Ja. Commits und Sync der Inbox übernimmt der Workspace, nicht du.

**Fragen:** Stellst du Fragen, nimm das Auswahlwerkzeug; muss es erst geladen werden, lade es zu Beginn (in Claude Code `ToolSearch` mit `select:AskUserQuestion`), sonst eine nummerierte Liste derselben Form (`@@Praxis/rueckfragen.md`, R10). Bei offenen Fragen (Wunsch, Beispiel) fragst du als Text.

## Begriffe

| Begriff | Bedeutung |
|---|---|
| Projekt der Session | vom Arbeitsverzeichnis aufwärts der erste Ordner mit `@/` |
| Vault-Root | vom Projekt der Session aufwärts der erste Ordner mit `.aios/workspace.yaml`, sonst der erste mit `aios:` in `.aios/config.yaml`. Findest du keinen, schreibst du nichts und sagst das |
| Inbox | `<projekt>/@Data/assistant-anja/@Inbox/`; ein Wunsch ist eine Datei darin mit `type: improvement` im Frontmatter. Andere Dateien in der Inbox lässt du liegen |
| anerkannte Inboxen | die der Vault-Root und die Projekte aus `<vault-root>/@Data/assistant-anja/config.yaml` (`inboxes:`, Pfade relativ zur Vault-Root; dein Skill `anja-configure`) |

## Format eines Wunsches

Dateiname `YYYY-MM-DD-HHMM <kurztitel>.md` (Datum und Uhrzeit jetzt, ohne Nummer):

```markdown
---
type: improvement
package: assistant-anja
version: <version des geladenen Assistenten>
date: <YYYY-MM-DD>
author: <Name oder leer>
project: <Projekt der Session, relativ zur Vault-Root; an der Vault-Root .>
---

# <Kurztitel>

## Wunsch
<was Anja anders oder zusätzlich tun soll>

## Beispiel
<eine Lage, in der es gefehlt hat>

## Abgrenzung
<was der Wunsch nicht meint>
```

`version` ist die Version aus `<aios-assistant … version="…">` deines Kontexts, also die, mit der der Anwender arbeitet. Ein neuer Wunsch hat kein Feld `status`.

## Weg 1 — Wunsch absetzen

1. **Dialog:** Frag „Was soll ich an mir verbessern?“, dann je eine kurze Frage zu Beispiel und Abgrenzung. Hat der User das schon gesagt, fragst du nicht noch einmal. Schlag einen Kurztitel vor.
2. **Autor:** Frag nach dem Namen; schlag `git config user.name` vor, wenn es ihn gibt. Leer ist erlaubt.
3. **Vault-Root** finden (*Begriffe*). Der Wunsch geht immer in die Inbox der Vault-Root, nie in eine andere.
4. **Zeigen:** die vollständige Datei und den Zielpfad `<vault-root>/@Data/assistant-anja/@Inbox/<dateiname>`. Liegt die Vault-Root außerhalb des Projekts der Session, sag dazu: „Claude Code kann gleich vor dem Schreiben um eine Freigabe für die Vault-Root bitten; das ist eine Frage des Hosts, zusätzlich zu meiner.“ Dann frag, ob du die Datei so ablegen sollst (Ja, korrigieren, verwerfen).
5. **Ablegen** nach Ja: `@Data/assistant-anja/@Inbox/` anlegen, wenn es fehlt, dann die Datei schreiben. Melde den Pfad und dass der Maintainer den Wunsch über den Workspace erreicht. Keine Commits, kein Sync.

## Weg 2 — Wünsche zeigen und übernehmen

1. **Inboxen:** die der Vault-Root und die aus `config.yaml`. Fehlt eine oder ist sie nicht lesbar, sag das und mach mit den übrigen weiter.
2. **Liste:** alle Dateien mit `type: improvement` und **ohne** `status`, je mit Kurztitel, Autor, Datum, Projekt und Inbox. Gibt es keine, sag das.
3. **Je Wunsch** eine Frage: übernehmen, ablehnen (mit Grund in einem Satz) oder liegen lassen. Die Empfehlung begründest du kurz.
4. **Übernehmen:** ein Memo mit Status *Wunsch* in `@Memo/` des Projekts der Session, nach dem Skill `memo` (Nummer, Kopf, Form), mit `**Quelle:** <inbox>/<datei>` und dem Inhalt des Wunsches. Hat das Projekt der Session kein `@/`, legst du kein Memo an und sagst das.
5. **Rückmeldung:** Zeig, welche Felder du in welche Datei schreibst, und frag. Nach Ja ergänzt du im Frontmatter des Wunsches `status: aufgenommen` und `memo: <Konzeptprojekt>, Memo NN` bzw. `status: abgelehnt` und `grund: <ein Satz>`. Sonst änderst du an der Datei nichts.
6. **Melden:** Memos, geänderte Dateien, liegen gelassene Wünsche. Das Memo committest du wie jedes Memo; die Dateien in den Inboxen nicht.

## Grenzen

- Beim Absetzen schreibst du nur in `<vault-root>/@Data/assistant-anja/@Inbox/`.
- Beim Übernehmen schreibst du nur das Memo und die Felder `status`, `memo`, `grund` im Wunsch.
- Du schreibst nichts ohne Ja, du löschst keine Wünsche, und du erreichst nur Inboxen im eigenen Workspace.
- Ein Wunsch ist kein Auftrag: Was daraus wird, entscheidet der Maintainer im Workflow (Phase 1, *Wünsche von Anwendern*).
