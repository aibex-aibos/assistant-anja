# Inhalt der Praxis

Der Leitfaden ist geladen. Die Praxis ist verfügbar: Lies das passende Dokument mit Read, bevor du zu seinem Thema etwas sagst oder tust. Jedes Dokument nennt seine Quellen in der Spec und im Konzept-Repo.

| Datei | Inhalt | Lesen, wenn … |
|---|---|---|
| `@@Praxis/namen-und-arten.md` | Paketnamen, Elementnamen, Titel, verbotene Namen | ein Paket oder Element benannt oder ein Name geprüft wird |
| `@@Praxis/manifest-und-versionen.md` | vollständiges Manifest, `contents`, SemVer, Versionen der Elemente, Standardbereich, `STATUS.md` im Paket-Repo | ein Manifest geschrieben oder geprüft oder eine Version erhöht wird |
| `@@Praxis/getaggte-staende.md` | Tags, Release-Kandidaten, Cache bei `--from`, Ablauf einer neuen Version | getaggt, korrigiert oder eine neue Version gemacht wird |
| `@@Praxis/inhalt-eines-assistenten.md` | Aufbau, Frontmatter, Rollentext, Wissen und Auflösung eines Assistenten; private Skills; Vorlage *Rückfragen und Entscheidungen*; Vorlage der Selbstverbesserung | ein Assistent entworfen, spezifiziert oder geprüft wird |
| `@@Praxis/grenzen-und-ehrlichkeit.md` | Grenzen im Rollentext, nicht raten, Quelle nennen, Fundstelle vor Empfehlungen | ein Rollentext geschrieben wird, eine Frage außerhalb des Wissens liegt oder du zu Versionen, Tags oder Formaten etwas empfiehlst |
| `@@Praxis/arbeitspferde.md` | Arbeitspferde: Kriterien, Paket `worker-<name>`, Hash-Datei, Installation unter `~/.aibos/workers/`, Weiterleiter, Aufruf aus Skills, Beispiel | ein Paket ein Programm braucht (z. B. `yt-dlp`) oder ein `worker` entsteht oder geprüft wird |
| `@@Praxis/daten-einer-app.md` | `@Data/<paket>/`, mitgelieferte Daten, Speichern aus HTML-Teilen | ein Paket Daten speichert (App oder Assistent) |
| `@@Praxis/oberflaechen.md` | `gui.yaml`, Kacheln, HTML-Teile, Ausweg im Chat | eine Oberfläche entworfen oder geprüft wird |
| `@@Praxis/rueckfragen.md` | Plan, Rückfrage, Ausführen; Schritte nach außen; Aufruf von `aibos` über den Weiterleiter; Auswahlwerkzeug laden | ein Skill schreibt, `aibos` aufruft, etwas nach außen wirkt oder du Fragen stellst |
| `@@Praxis/abhaengigkeiten.md` | `depends`, Bereiche, Suche in Hubs, Resolver, Paketwechsel bei der Aktivierung | ein Paket ein anderes braucht |
| `@@Praxis/probe-vor-veroeffentlichung.md` | Ablauf der Probe in einem Testprojekt, `--to`, Release-Kandidaten, Wegwerf-Kandidat, Test-Chats als Subsession, Testprojekte in den Papierkorb | ein Paket ausprobiert oder ein Abnahmetest skizziert wird |
| `@@Praxis/veroeffentlichen.md` | Checkliste für das Veröffentlichen im Hub, Begleitpakete, Commits vor dem Push, `git remote -v` nach dem Anlegen | ein Paket veröffentlicht wird |
| `@@Praxis/rueckfluss.md` | `aibos --contribute`, Übernahme durch den Maintainer | Beiträge von Usern ankommen oder angenommen werden sollen |
| `@@Praxis/testvorlagen.md` | Test-Hubs, Testversionen, Live-Tests, Headless-Läufe als Standard für Schritte im Chat, Beispielskripte für Hooks | automatische Tests für Pakete oder den Kernel entstehen, ein Abnahmeschritt im Chat läuft oder ein Hook-Skript geprüft wird |
| `@@Praxis/entwicklungsprozess.md` | Best Practices P1–P12 hinter deinem Workflow, Plan und `STATUS.md`, Maß halten je Paketgröße | der User nach dem Sinn eines Schritts fragt oder du den Umfang des Workflows vorschlägst |
| `@@Praxis/kernel-distribution-tutorial.md` | Kernel, Kernel-Skills und Hüllen, Distributionen, Tutorials | ein Paket dieser Typen entsteht oder geprüft wird |

Lies nur das Dokument, das zur Frage passt. Steht die Antwort weder dort noch in der Spec, sag das, statt zu raten.
