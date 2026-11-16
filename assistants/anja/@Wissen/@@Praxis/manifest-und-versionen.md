# Praxis — Manifest und Versionen

Quellen: Spec v0.7.0 (bis v0.6.6 mit „aios“), 03 *Format eines Skills*, *Versionen*, *Assistenten*; 06 *`manifest.yaml`*, *Prüfung beim Holen*, *Versionen und Bereiche*, *Distributionen*; 07 *Paket `worker-<name>`*. AA-AIbex-AIOS, V0.4, Memos 19 (L5), 44 (X1), 70; V0.6, Memos 38, 50. MAA-Anja-AIOS-Expert, Memo 83 (F37). Pakete im Hub `aibex-aibos`.

## Manifest

```yaml
name: app-taschenrechner
version: 0.6.0
type: app
description: Programmierbarer RPN-Taschenrechner nach dem Vorbild des HP-15C mit lernenden Funktionstasten (Demo-App von AIbex AIBOS)
requires:
  aibos: ">=0.7,<0.8"
depends:
  skill-memo: "~=6.0"
contents:
  skills: [taschenrechner, taschenrechner-gui]
  knowledge: true
  gui: taschenrechner-gui
license: MIT
```

| # | Regel |
|---|---|
| M1 | Pflicht: `name`, `version`, `type`, `description` (ein Satz), `requires.aibos` |
| M2 | `requires.aibos` nach der AIBOS-Version, für die das Paket gebaut ist: für v0.7 **jedes Paket** `">=0.7,<0.8"`, für jeden Typ, auch bestehende in ihrer nächsten Minor-Version (Beispiel oben; V0.7, Memo 10: F283, F284). Bis 0.8 liest AIBOS auch den alten Schlüssel `requires.aios`; Pakete mit der Obergrenze `<0.7` lassen sich mit v0.7 nicht holen, `aibos --activate` stellt sie auf ihre Version für 0.7 um oder bricht ab (Spec 06, *Alter Schlüssel bis 0.8*; Praxis *Abhängigkeiten*). Vorabkennungen des Kernels zählen nicht (`0.7.0-rc1` gilt als `0.7.0`) |
| M3 | `contents` nennt jedes Element; jeder Name muss als Ordner vorhanden sein, sonst holt `aibos --fetch` das Paket nicht. `gui` muss in `skills` stehen; Elementnamen dürfen sich nicht wiederholen |
| M4 | Ein Assistent steht unter `contents.assistants`; seine privaten Skills (`assistants/<name>/@!Skill/`) stehen **nicht** in `contents.skills` |
| M5 | `depends` nur für Pakete, ohne die das Paket nicht läuft; `aibos` gehört nach `requires` (Praxis *Abhängigkeiten*) |
| M6 | `license` setzen, wenn Daten Dritter enthalten sind (`knowledge-orion`: CC-BY-4.0); sonst nach den Vorgaben des Users |
| M7 | `contents.hooks` ist in v0.7 ohne Wirkung; `aibos --fetch` meldet ihn |
| M8 | `type` ist einer von `kernel`, `app`, `assistant`, `skill`, `knowledge`, `tutorial`, `distribution`, `worker`; `worker:` nur bei `type: worker` (Praxis *Arbeitspferde*), `kernel:` nur bei `type: distribution` (Praxis *Kernel, Distributionen und Tutorials*); `workers:` aus v0.4 wird abgelehnt |

## Versionen

| # | Regel | Beispiel |
|---|---|---|
| V1 | SemVer `MAJOR.MINOR.PATCH`; ein neues Paket beginnt mit `0.1.0` | `knowledge-orion` 0.1.0 |
| V2 | Jedes `SKILL.md` und `ASSISTANT.md` braucht eine gültige `version`, sonst wird das Element nicht registriert bzw. aufgelöst | — |
| V3 | Die Elemente in `skills/` und `assistants/` tragen die **Version des Pakets ohne Vorabkennung** (Konvention von AIbex; die Spec verlangt es nicht). Ein Element mit `0.3.0-rc1` verwirft AIBOS (Spec 03: `MAJOR.MINOR.PATCH`), und eine andere Kopie gewinnt still | `assistant-gaertner` 0.3.1 → `ASSISTANT.md` 0.3.1 |
| V4 | **Private Skills** eines Assistenten behalten ihre eigene Version und steigen nur bei eigener Änderung | `gaertner-gui` mit eigener Version in `assistant-gaertner` |
| V5 | Vor 1.0 darf jede Minor-Version brechen: Korrektur = Patch, neue Funktion oder Bruch = Minor | Taschenrechner 0.3.2 → 0.4.0 (Funktionstasten statt Kacheln) |
| V6 | Ohne Bereich geholt gilt unter 1.0 `~=0.Y.Z` (nur Patches derselben Minor-Version), ab 1.0 `~=X.Y`, jeweils von der höchsten Version, die zur Bindung passt (`requires.aibos`) | `app-taschenrechner` 0.1.0 → `>=0.1.0,<0.2.0` |
| V7 | Jede Änderung am Inhalt, auch ein Satz im Rollentext, ist eine neue Version | Astronom 0.1.0 → 0.2.0 für einen Satz (V0.4, Memo 27) |

## Dateien im Paket-Repo

Das Paket-Repo enthält nur, was beim Ausrollen gebraucht wird: `manifest.yaml`, `README.md`, Lizenzdateien und die Elemente. Einzige Ausnahme ist **`STATUS.md`** mit dem Stand eines freigegebenen Implementierungsplans (Schritte mit ☐/☑ und Ergebnis), solange er läuft; im Tag eines Release-Kandidaten sind alle Schritte ☑ (Workflow, Phase 7; MAA, Memo 120, R18, R19). Der Commit nach der Abnahme, der `version` auf `<x.y.z>` setzt, entfernt `STATUS.md`; sonst kommt es beim Holen in die Projekte der Anwender (MAA, Memo 138, B57). Memos, Notizen und Testdaten gehören ins Konzeptprojekt.
