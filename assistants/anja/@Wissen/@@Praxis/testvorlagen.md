# Praxis — Testvorlagen

Quellen: Spec v0.6.6, 02 *Freigabe*; 04 *Umlenkung für Tests* (`AIOS_HUBS`, `AIOS_GITHUB`); 06 *Hub-Liste*, *Rückfluss* (Tests). AA-AIbex-AIOS, V0.4, Memos 19 (L4), 27, 44, 61, 62 (G1), 68, 71. MAA-Anja-AIOS-Expert, Memos 77, 80 (F28), 114, 122. Code-Repo `aibex-aios`, `tests/hub/`, `tests/live/`.

Testvorlagen braucht, wer **automatische Tests** gegen Pakete schreibt: der Kernel und jedes Paket mit eigenem Code (Befehle, Arbeitspferde). Für ein einfaches Paket genügt die Probe.

| # | Regel | Beispiel |
|---|---|---|
| E1 | Hubs für Tests liegen lokal unter `file://` und werden mit `AIOS_HUBS` eingesetzt; GitHub mit `AIOS_GITHUB` umgelenkt; `gh` durch eine Attrappe im `PATH` ersetzt | `AIOS_HUBS="aibex-aios=file://… aibex-hub=file://…"` |
| E2 | Eine Vorlage einer **echten** Version ist **gleich dem Tag im Repo**; geprüft mit `diff` | `skill-memo` 6.0.0 im Test-Hub = Tag `v6.0.0` |
| E3 | **Testversionen** (eigens für einen Test verändert) bekommen eine **höhere Nummer** als jede echte Version und werden mit jeder neuen echten Version weiter hochgezählt | Taschenrechner: echt 0.4.2, Testversion 0.4.3 |
| E4 | Reine Testpakete tragen das Präfix `test-` und kommen nie in einen echten Hub | `test-…` |
| E5 | Zwei Hubs mit einem gleichnamigen Paket prüfen die Rückfrage bei Mehrdeutigkeit | `skill-memo` in beiden Test-Hubs |
| E6 | Live-Tests mit einem kleinen Modell (haiku) prüfen Hooks, Skills, Assistenten und Oberflächen; sie nehmen die Pakete aus dem Test-Hub | `tests/live/assistants.sh` |
| E7 | **Headless-Läufe sind der Standard für Abnahmeschritte im Chat.** Sie laufen ohne die Regeln des Users und zeigen, was der Hook allein freigibt: `claude -p "<prompt>" --setting-sources project --settings <datei mit nur den Hooks> --permission-mode default --output-format json` im Testprojekt, stdin geschlossen (`< /dev/null`); zum Schreiben `--permission-mode acceptEdits`; mehrere Züge mit `--resume <session_id>`, ebenfalls mit geschlossenem stdin. Abgelehnte Aufrufe stehen im Ergebnis unter `permission_denials`, der Verlauf im Transkript | `MAAP-Probe-Anja-040/probe/` (MAA, Memos 77, 122) |
| E8 | **Vor dem ersten Lauf** `claude -p "ok"`: Bei 401 erneuert der User die Anmeldung im Terminal; sonst scheitern alle Läufe gleich | MAA, Memo 114 |
| E9 | Das **Auswahlwerkzeug** wird headless abgelehnt (`permission_denials`); im nächsten Lauf antwortest du mit dem Text der gewählten Option. Was nur interaktiv geht (Auswahlwerkzeug sichtbar, Freigabe-Dialog des Hosts, Sitzungswerkzeuge), läuft als Subsession mit genanntem Modus | Abnahme Anja 0.4.0, Schritte 8, 19 |
| E10 | **Schreiben außerhalb des Startverzeichnisses** braucht `--add-dir <ordner>`, und der Ordner muss existieren; ein fehlender hilft nicht. Schreiben in `.claude/` braucht immer den User | Vault-Root: `--add-dir <vault-root>` (MAA, Memo 122, H11) |

## Zu beachten

- Nach jeder neuen echten Version die Test-Hubs neu bauen und E2 erneut prüfen.
- Testversionen nie veröffentlichen; ihre höhere Nummer würde sonst bei `--upgrade` gewinnen.
- **Warum E7:** Erlaubt der User global `Bash` (`permissions.allow`), fragt der Host bei keinem Befehl; ob ein Skill auch bei anderen Usern ohne Rückfrage läuft, zeigt erst ein Lauf ohne diese Regel. Ein Lauf je Variante zeigt eine Tendenz, keine Gewissheit.

## Beispielskripte für Hooks prüfen

Ein Hook-Skript, das ein Paket als Beispiel mitbringt, prüfst du vor dem Paket mit **mindestens 15 Fällen** über alle Wege: `Write`, `Edit`, `Read`, `Bash` mit Anführungszeichen, `>`, `>>`, `2>&1`, Pipes und Ketten, dazu lesende Befehle, die durchgehen müssen. Jeder Fall ist ein JSON auf stdin mit dem erwarteten Exit-Code (`0` erlaubt, `2` blockiert). Ein Durchlauf für dieses Beispiel fand einen Fehler (Fall 17: `tee` hinter einer Pipe), der ohne die Fälle ins Paket gekommen wäre.

```python
#!/usr/bin/env python3
"""PreToolUse-Hook: blockiert Schreiben in geholte Pakete (@!Package/); Lesen bleibt erlaubt."""
import json, re, shlex, sys

d = json.load(sys.stdin)
tool, inp = d.get("tool_name", ""), d.get("tool_input", {})
GESCHUETZT = "/@!Package/"

def block(was):
    print(f"Geholte Pakete sind schreibgeschützt: {was}", file=sys.stderr)
    sys.exit(2)

if tool in ("Write", "Edit", "MultiEdit", "NotebookEdit"):
    pfad = inp.get("file_path") or inp.get("notebook_path") or ""
    if GESCHUETZT in pfad:
        block(pfad)
elif tool == "Bash":
    befehl = inp.get("command", "")
    befehl = re.sub(r"\d?>&\d", "", befehl)            # 2>&1 ist keine Umleitung in eine Datei
    for m in re.finditer(r">>?\s*(\"[^\"]*\"|'[^']*'|\S+)", befehl):
        if GESCHUETZT in m.group(1):
            block(m.group(1))
    for teil in re.split(r"\|\|?|;|&&", befehl):     # jeder Befehl einer Kette einzeln
        try:
            teile = shlex.split(teil)
        except ValueError:
            teile = teil.split()
        if not teile:
            continue
        prog, args = teile[0], [a for a in teile[1:] if not a.startswith("-")]
        ziele = {"tee": args, "rm": args, "touch": args, "mkdir": args,
                 "cp": args[-1:], "mv": args[-1:]}.get(prog, [])
        if prog == "sed" and "-i" in teile:
            ziele = args[1:]
        for z in ziele:
            if GESCHUETZT in z:
                block(z)
sys.exit(0)
```

| # | Werkzeug | Pfad bzw. Befehl | Exit |
|---|---|---|---|
| 1 | `Write` | `/v/@/@!Package/p/a.md` | 2 |
| 2 | `Write` | `/v/P/@Memo/01.md` | 0 |
| 3 | `Edit` | `/v/@/@!Package/p/a.md` | 2 |
| 4 | `Edit` | `/v/P/a.md` | 0 |
| 5 | `Read` | `/v/@/@!Package/p/a.md` | 0 |
| 6 | `Bash` | `cat /v/@/@!Package/p/a.md` | 0 |
| 7 | `Bash` | `ls -la /v/@/@!Package/` | 0 |
| 8 | `Bash` | `echo x > /v/@/@!Package/p/a.md` | 2 |
| 9 | `Bash` | `echo x >> /v/@/@!Package/p/a.md` | 2 |
| 10 | `Bash` | `echo x > "/v/@/@!Package/p/a b.md"` | 2 |
| 11 | `Bash` | `cat /v/@/@!Package/p/a.md > /tmp/a.md` | 0 |
| 12 | `Bash` | `grep -r x /v/@/@!Package/ 2>&1` | 0 |
| 13 | `Bash` | `cp /tmp/a.md /v/@/@!Package/p/` | 2 |
| 14 | `Bash` | `cp /v/@/@!Package/p/a.md /tmp/` | 0 |
| 15 | `Bash` | `rm -f /v/@/@!Package/p/a.md` | 2 |
| 16 | `Bash` | `sed -i '' s/a/b/ /v/@/@!Package/p/a.md` | 2 |
| 17 | `Bash` | `echo x \| tee /v/@/@!Package/p/a.md` | 2 |
| 18 | `Bash` | `git -C /v status` | 0 |
| 19 | `Bash` | `cd /tmp && rm /v/@/@!Package/p/a.md` | 2 |
| 20 | `Bash` | `ls /v/@/@!Package/ \| wc -l` | 0 |

Ausführen, Fälle als `<exit>` Tab `<json>` je Zeile in `faelle.tsv`:

```bash
while IFS=$'\t' read -r exp json; do printf '%s' "$json" | python3 schutz-package.py 2>/dev/null; [ $? = "$exp" ] || echo "Fehler: $json"; done < faelle.tsv
```
