---
name: anja-configure
version: 0.1.0
description: >
  Legt fest, welche Inboxen für Wünsche an die Assistentin Anja sie beim Maintainer anerkennt
  (<vault-root>/@Data/assistant-anja/config.yaml): anzeigen, hinzufügen, entfernen.
  Verwenden bei "Erkenne auch die Inbox in … an", "Welche Inboxen kennst du?",
  "Konfiguriere Anja".
---

# Konfiguration

Ein privater Skill der Assistentin Anja. Er pflegt die Liste der Inboxen, die dein Skill `anja-improve` im Weg 2 liest.

**Arbeitsweise:** Arbeite mit absoluten Pfaden, wechsle nie mit `cd`. Du schreibst nichts ohne Ja.

**Dateien finden:** Prüfe und lies Dateien über ihren vollständigen Pfad mit Read oder mit `ls` (Bash), nicht mit Glob: Unter Pfaden mit Sonderzeichen wie `§` findet Glob vorhandene Dateien nicht verlässlich (MAA, Memo 137, B52).

**Fragen:** Jede Frage mit Optionen stellst du mit dem Auswahlwerkzeug, nie als Liste im Text. In Claude Code lädst du es zu Beginn mit `ToolSearch` und `select:AskUserQuestion`, noch bevor du die erste Frage stellst, und rufst es dann für jede Frage mit Optionen auf. Eine nummerierte Liste derselben Form gibt es nur, nachdem ein Aufruf abgelehnt wurde (etwa headless) (`@@Praxis/rueckfragen.md`, R10).

## Datei

`<vault-root>/@Data/assistant-anja/config.yaml`; die Vault-Root findest du wie in `anja-improve` (*Begriffe*).

```yaml
inboxes:
  - M-Module/MA-Assistants/MAA-Anja-AIOS-Expert/MAAP-Probe-Anja-040
```

| Punkt | Regel |
|---|---|
| Einträge | Projektpfade **relativ zur Vault-Root**; gemeint ist jeweils `<projekt>/@Data/assistant-anja/@Inbox/` |
| Vault-Root | gilt immer und steht nicht in der Liste |
| Reichweite | nur der eigene Workspace; absolute Pfade und Pfade mit `..` gibt es nicht. Einen anderen Workspace erreicht der Maintainer, indem er ihn einbindet und dort diesen Skill aufruft |
| Fehler | ein Pfad, dessen Ordner kein `@/` hat, ist kein Projekt; du trägst ihn nicht ein und sagst das |

## Ablauf

1. **Anzeigen:** die Vault-Root und jeden Eintrag aus `config.yaml`, je mit der Angabe, ob die Inbox existiert, geprüft mit `ls`: `vorhanden` nur, wenn `<projekt>/@Data/assistant-anja/@Inbox/` existiert; `noch leer`, wenn das Projekt `@/` hat, aber `@Inbox/` fehlt; `fehlt`, wenn es das Projekt nicht gibt. Dieselbe Angabe gilt in der Meldung nach dem Schreiben. Gibt es die Datei nicht, nennst du nur die Vault-Root.
2. **Ändern:** Will der User eine Inbox hinzufügen oder entfernen, rechnest du den Pfad relativ zur Vault-Root aus (auch aus einem absoluten Pfad oder einem Projektnamen, den er nennt) und prüfst ihn (*Datei*, Fehler).
3. **Plan zeigen:** die vollständige neue `config.yaml`. Liegt die Vault-Root außerhalb des Projekts der Session, sag dazu, dass Claude Code vor dem Schreiben um eine Freigabe bitten kann. Dann frag.
4. **Schreiben** nach Ja; `@Data/assistant-anja/` anlegen, wenn es fehlt. Keine Commits, kein Sync.
5. **Melden:** die Liste wie in Schritt 1.

## Grenzen

Du schreibst nur `<vault-root>/@Data/assistant-anja/config.yaml`. Inboxen legst du nicht an und änderst keine Wünsche.
