# Praxis — Rückfragen

Quellen: Spec v0.6.6, 01 Invarianten I7, I10, I11; 02 *Freigabe*, *Block der Kernel-Skills*; 04 *Benutzer* (`PATH`); 06 Invariante P3, *Holen*; 07 *Abläufe*, *Paket finden*. AA-AIbex-AIOS, V0.4, Memos 65, 67 (B2), 74. MAA-Anja-AIOS-Expert, Memos 75, 77, 78 (F21), 114, 124. Skill `taschenrechner`.

| # | Regel | Beispiel |
|---|---|---|
| R1 | Jede schreibende Handlung zeigt vorher, **was** sie tut, und wartet auf Bestätigung; ein Skill holt den Plan mit `--plan`, fragt und ruft erst nach Ja mit `--yes` auf | `aios --fetch … --plan` → Plan → Ja → `--yes` |
| R2 | **Der Wunsch ist keine Bestätigung des Plans.** „Installiere den Taschenrechner" erlaubt nicht, ohne Rückfrage zu holen; der User kennt den Plan (z. B. eine Abhängigkeit) noch nicht | Abnahme v0.4: `skill-memo` kam ungefragt mit (B2) |
| R3 | Ohne Rückfrage nur, wenn der User den **gezeigten** Plan bestätigt oder ausdrücklich „ohne Rückfrage" gesagt hat | — |
| R4 | Installationen und Skripte eines Pakets laufen nie ohne Bestätigung: das setup eines Arbeitspferds steht im Plan beim Laden (bei `kind: python` die Liste `paket==version`, bei `kind: script` der Inhalt der Skripte); Befehle nur, wenn der User sie aufruft | Plan von `aios --fetch skill-video-analyse` mit setup von `worker-yt-dlp` |
| R5 | Bei Mehrdeutigkeit fragen statt wählen, mit dem Auswahlwerkzeug | „Hol mir skill-memo" → Auswahl des Hubs |
| R6 | Auch ein Skill, der Daten speichert, zeigt vorher: was, wohin, mit welchem Inhalt | Skill `taschenrechner` |
| R7 | Rückfragen klein halten: wenige Fragen auf einmal, je mit empfohlener Antwort zuerst und kurzer Folge | — |
| R8 | **Ausnahme für Memo-Skills:** Ein Skill, dessen Aufruf ausdrücklich ein Memo verlangt, darf es ohne Rückfrage schreiben, wenn die Spec des Pakets das als Grenze nennt; der Aufruf gilt dann als Auftrag | `memo`, `aios-memo`, `5-8-zusammenfassung` |
| R9 | **Aufruf von `aios`:** `aios` steht in der Regel nicht im `PATH`, und der Hook `PreToolUse` gibt im A/W nur einen einzelnen Befehl frei, der mit dem ausgeschriebenen Pfad des Weiterleiters beginnt (nicht `~`, nicht `$HOME`, kein `cd`, `;`, `&&`, `|`, `>`). Jeder Skill, der `aios` aufruft, sagt deshalb wörtlich: „Rufe `aios` über den Weiterleiter auf: mit dem absoluten Pfad aus dem Block `<aios-kernel-skills>` deines Master-Kontexts, als einzelnen Befehl ohne `cd`, `;`, `&&`, `|` oder Umleitung." Ohne den Satz hält sich die KI an den Wortlaut, und der Aufruf scheitert (MAA, Memo 77) | in `SKILL.md`: `aios --fetch <paket> --plan` plus der Satz |
| R10 | **Auswahlwerkzeug immer:** Jede Frage mit Optionen stellt der Assistent mit dem Auswahlwerkzeug, nie als Liste im Text, auch die Frage am Ende einer Antwort (nächster Schritt, Ja/Nein, „… oder …?“; MAA, Memo 144, B60); der User will es so (MAA, Memo 142, B59). Muss das Werkzeug erst geladen werden, lädt der Assistent es zu Beginn und ruft es dann auf. In Claude Code ist `AskUserQuestion` ein nachgeladenes Werkzeug: erst `ToolSearch` mit `select:AskUserQuestion`, dann der Aufruf; ohne das Laden ruft das Modell es nie auf und stellt die Fragen als Text. Erst **nachdem ein Aufruf abgelehnt wurde** (etwa in einem Headless-Lauf) oder wenn der Host gar kein Auswahlwerkzeug kennt, stellt der Assistent dieselben Fragen **sofort** als nummerierte Liste derselben Form (Empfehlung zuerst, eigene Antwort zuletzt), statt den Zug ohne Fragen zu beenden | Sandra 0.1 und 0.2: Fragen als Text statt Auswahl (MAA, Memo 114) |

## Was ein Paket deshalb mitbringen muss

- Jeder Skill, der schreibt, beschreibt **Plan → Rückfrage → Ausführen** ausdrücklich in seiner `SKILL.md`. Ein Satz wie „ist der Wunsch eindeutig, ohne Rückfrage" ist ein Fehler (B2), außer bei einem Memo-Skill nach R8.
- Befehle und Arbeitspferde haben einen Plan-Modus oder werden von einem Skill aufgerufen, der den Plan zeigt.
- Jeder Skill, der `aios` aufruft, trägt den Satz aus R9. Arbeitspferde (`~/.aios/bin/<programm>`) gibt der Hook nicht frei; ihre Aufrufe fragt der Host wie gewohnt (Praxis *Arbeitspferde*).

## Schritte nach außen

Repo anlegen, pushen, Tag pushen, Topic setzen, Eintrag in `aios-index`, Pull Request: **jeder Schritt mit eigener Rückfrage**, auch wenn der User „veröffentliche" gesagt hat (Praxis *Veröffentlichen*).
