# assistant-anja

Assistentin *Anja* von AIbex AIBOS, Version 0.6.1: Spezialistin für die Entwicklung von AIBOS-Paketen aller Typen (App, Assistent, Skill, Wissen, Tutorial, Distribution, Arbeitspferd, Kernel, Mod). Sie kennt die Spec von AIBOS v0.7, die Gründe dafür und die Praxis aus der Entwicklung und begleitet ein Paket vom ersten Gedanken bis zur Veröffentlichung. Jede Antwort beginnt mit „🙎🏻‍♀️ ".

## Holen

**Voraussetzung:** AIbex AIBOS 0.7.1 oder neuer (unter 0.8) ist installiert, und der Vault ist an der Vault-Root mit `aibos --activate v0.7.<n>` daran gebunden (ein AIBOS-Workspace); sonst tut `aibos --fetch` nichts. Für AIBOS 0.7.0 gibt es Anja 0.6.0 (`aibos --fetch "assistant-anja==0.6.0"`). Für AIbex AIOS v0.6 gibt es Anja 0.4.0 (`aios --fetch "assistant-anja~=0.4.0"`).

```bash
aibos --fetch assistant-anja
```

Anja kommt aus dem Hub `aibex-aibos`, dem Standard-Hub von AIBOS; deshalb braucht der Befehl keine Hub-Angabe. Ausgeschrieben lautet er `aibos --fetch aibex-aibos/assistant-anja`. Er holt die Pakete `skill-cleanup` und `skill-improve` mit (`depends`): Anja braucht das erste zum Abschluss einer Session (`/cleanup`), das zweite für Verbesserungsvorschläge (`/improve`).

Zuordnen mit `/aibos-assistant Anja` in dem Projekt, in dem die Pakete konzipiert werden. Anja und ihre Skills wirken **ab dem nächsten Chat** in diesem Projekt.

## Wie Anja arbeitet

Anja folgt einem Workflow in elf Phasen: Einstieg, Konzept, Bewertung, Konsolidierung in kleinen Fragegruppen, Soll-Bild und Skizze, Spezifikation, Freigabe, Implementierung, Abnahme, Rückblick und Abschluss. Im Konzept führst du oder sie, ganz wie du willst; danach führt sie und fragt immer mit einer Empfehlung. Jeder Übergang braucht dein Ja, alles wird in Memos festgehalten, der Stand in einem Plan (`@Plan/Plan.md`), und gebaut wird erst nach der Freigabe der Spezifikation.

Anjas eigene Skills (Paket anlegen, prüfen, veröffentlichen; Wünsche aufgreifen) sind **private Skills**: Sie werden durch Sätze im Chat ausgelöst, etwa „Prüf das Paket“, nicht als Slash-Befehle.

## Verbesserungsvorschlag

Einen Wunsch an Anja (oder an jedes andere Paket) erfasst du mit **`/improve`** aus dem Paket `skill-improve`. Er landet als Ticket im Postfach `@/@!Ticket/` an der Vault-Root deines Workspace; GitHub brauchst du dafür nicht.

- **Probebetrieb:** Liegen Wünsche an Anja im Postfach, bietet sie zu Beginn einer Session an, sie auszuprobieren (IMPROVED). Die gewählten Wünsche gelten dann nur für diese Session, und Anjas Prefix trägt 🏗️. Keine Datei wird geändert.
- **Coach:** In einem Konzeptprojekt (ein Projekt mit Paket-Repos unter `@Package/`) ist Anja Coach für jedes Paket dort. Sie sammelt die Wünsche zu einem Paket als Memos und greift sie mit dir auf („Greif die Wünsche auf“). Was daraus wird, entscheidest du.
- **Reichweite:** Wünsche erreichen den Coach nur im eigenen Workspace.

Mehr dazu steht im README von `skill-improve`.

## Voraussetzungen

Anja liest die Spec im Code-Repo `aibex-aibos/aibex-aibos` (je Version über den Tag) und die Gründe im Konzept-Repo `aibex-aibos/AA-AIbex-AIOS`. Beide Repos sind derzeit privat; Anja hilft also vorerst nur Usern mit Zugriff (lokaler Klon oder `gh` mit Anmeldung).

## Vorgaben

Wo Paket-Repos liegen, in welchem Hub veröffentlicht wird und unter welcher Lizenz, legt jeder User selbst fest: in einer Regel *AIBOS-Pakete — Vorgaben* (`name: aibos-pakete-vorgaben`, bis 0.8 auch `aios-pakete-vorgaben`) im Masterspace des Konzeptprojekts. Dort stehen auch die Orte der Vaults, die Anja vor einer Abnahme durchsucht. Ohne die Regel legt Anja Paket-Repos in `<projekt>/@Package/<paket>/` an und fragt den Rest, wenn er gebraucht wird.

## Lizenz

Apache License 2.0 (`LICENSE`), Rechteinhaber AIbex KG (`NOTICE`). Die Marken „AIbex" und „AIbex AIBOS" sind nicht mitlizenziert.
