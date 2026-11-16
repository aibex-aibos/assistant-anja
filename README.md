# assistant-anja

Assistentin *Anja* von AIbex AIBOS, Version 0.5.0: Spezialistin für die Entwicklung von AIBOS-Paketen aller Typen (App, Assistent, Skill, Wissen, Tutorial, Distribution, Arbeitspferd, Kernel). Sie kennt die Spec von AIBOS v0.7, die Gründe dafür und die Praxis aus der Entwicklung und begleitet ein Paket vom ersten Gedanken bis zur Veröffentlichung. Jede Antwort beginnt mit „🙎🏻‍♀️ ".

## Holen

**Voraussetzung:** AIbex AIBOS v0.7 ist installiert, und der Vault ist an der Vault-Root mit `aibos --activate v0.7.<n>` gebunden (ein AIBOS-Workspace); sonst tut `aibos --fetch` nichts. Für AIbex AIOS v0.6 gibt es Anja 0.4.0 (`aios --fetch "assistant-anja~=0.4.0"`), für v0.4 und v0.5 Anja 0.2.x (`aios --fetch "assistant-anja~=0.2.1"`).

```bash
aibos --fetch assistant-anja
```

Anja kommt aus dem Hub `aibex-aibos`, dem Standard-Hub von AIBOS; deshalb braucht der Befehl keine Hub-Angabe. Ausgeschrieben lautet er `aibos --fetch aibex-aibos/assistant-anja`. Er holt das Paket `skill-cleanup` mit (`depends`); Anja braucht es zum Abschluss einer Session (`/cleanup`).

Zuordnen mit `/aibos-assistant Anja` in dem Projekt, in dem die Pakete konzipiert werden. Anja und ihre Skills wirken **ab dem nächsten Chat** in diesem Projekt.

## Wie Anja arbeitet

Anja folgt einem Workflow in elf Phasen: Einstieg, Konzept, Bewertung, Konsolidierung in kleinen Fragegruppen, Soll-Bild und Skizze, Spezifikation, Freigabe, Implementierung, Abnahme, Rückblick und Abschluss. Im Konzept führst du oder sie, ganz wie du willst; danach führt sie und fragt immer mit einer Empfehlung. Jeder Übergang braucht dein Ja, alles wird in Memos festgehalten, der Stand in einem Plan (`@Plan/Plan.md`), und gebaut wird erst nach der Freigabe der Spezifikation.

Anjas eigene Skills (Paket anlegen, prüfen, veröffentlichen; Verbesserung; Konfiguration) sind **private Skills**: Sie werden durch Sätze im Chat ausgelöst, etwa „Prüf das Paket“, nicht als Slash-Befehle.

## Verbesserungsvorschlag

Sag zu Anja: **„Ich hab einen Verbesserungsvorschlag für dich“.** Sie fragt nach Wunsch, Beispiel und Abgrenzung, zeigt dir die Datei und legt sie nach deinem Ja in die Inbox `@Data/assistant-anja/@Inbox/` an der Vault-Root deines Workspace. Von dort übernimmt der Maintainer den Wunsch; GitHub brauchst du dafür nicht. Arbeitest du in einem Unterprojekt, kann Claude Code vor dem Schreiben um eine Freigabe für die Vault-Root bitten.

## Voraussetzungen

Anja liest die Spec im Code-Repo `aibex-aibos/aibex-aibos` (je Version über den Tag) und die Gründe im Konzept-Repo `aibex-aibos/AA-AIbex-AIOS`. Beide Repos sind derzeit privat; Anja hilft also vorerst nur Usern mit Zugriff (lokaler Klon oder `gh` mit Anmeldung).

## Vorgaben

Wo Paket-Repos liegen, in welchem Hub veröffentlicht wird und unter welcher Lizenz, legt jeder User selbst fest: in einer Regel *AIBOS-Pakete — Vorgaben* (`name: aibos-pakete-vorgaben`, bis 0.8 auch `aios-pakete-vorgaben`) im Masterspace des Konzeptprojekts. Dort stehen auch die Orte der Vaults, die Anja vor einer Abnahme durchsucht. Ohne die Regel legt Anja Paket-Repos in `<projekt>/@Package/<paket>/` an und fragt den Rest, wenn er gebraucht wird.

## Lizenz

Apache License 2.0 (`LICENSE`), Rechteinhaber AIbex KG (`NOTICE`). Die Marken „AIbex" und „AIbex AIBOS" sind nicht mitlizenziert.
