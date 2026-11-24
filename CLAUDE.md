# Notizen für Claude – Das Kartenspiel

Online-Kartenspiel für Ischan und seine Freunde. Antworte auf Deutsch, locker mit „du“.
Live: https://larry03y.github.io/Cardgame/ (GitHub Pages aus `main`, Repo Larry03Y/Cardgame, öffentlich).

## Aufbau
- Alles in **einer Datei**: `index.html` (HTML + CSS + JS). Keine Build-Schritte.
- Bilder: `img/bg0..bg33.jpg` (Hintergründe für „Is this game modded?“), `img/cards/<kartennummer>.jpg` (Kartenbilder, 420×600, Name steht oben drüber).
- Kartenbilder werden über `CARD_IMG` freigeschaltet (Nummer = Index in `CARDS`).
- Netzwerk: MQTT über den öffentlichen HiveMQ-Broker (wss), ohne Accounts. Raum-Ersteller (Host) rechnet das Spiel, Gäste schicken nur Aktionen.
- Ende-zu-Ende verschlüsselt (AES-GCM, Schlüssel aus dem Raum-Code per PBKDF2). Das nicht ausbauen – war Ischan wichtig.
- Bots laufen beim Host (`botDecide`, mit absichtlichen Fehlern).
- Sprachen: Deutsch/Englisch pro Spieler (`dk-lang`), Kartenbilder an/aus (`dk-pics`). Englisch: `CARD_EN`, `LORE_EN`, `L(de, zh, en)`, Spielverlauf wird beim Anzeigen über `TR`/`tr()` übersetzt. Neue Log-Texte brauchen eine passende `TR`-Regel.
- Modi: classic, normal, chaos, blind, quick, roulette (`MODES`, `buildMix`).

## Neue Karte hinzufügen (Checkliste)
1. Eintrag in `CARDS` (emoji, text, eff, zh, zeff; ggf. `target`/`choose`/`options`/`pick`).
2. Anzahl in `BASE_MIX`/`DECK_MIX` (Deck hat jede Karte doppelt).
3. Konstante + `case` in `effect()`.
4. `LORE` (de), `CARD_EN` + `LORE_EN` (en), ggf. `KIND`, Bot-Bewertung in `botDecide`.
5. `TR`-Regeln für neue Log-Texte.

## Versionen
- `VERSION` = Protokollnummer (nur erhöhen, wenn sich Spiellogik/Netzwerk ändert), `VERSION_LABEL` = Anzeige.
- Neue Inhalte = neue Hauptversion (17, 18 …). Reine Bugfixes/Bilder = 17.1, 17.2 … (so wie Ischan es sagt).
- Nach Spiellogik-Änderungen müssen alle neu laden: Link mit `?v=<VERSION_LABEL>` mitgeben. Wenn noch die alte Version erscheint: Strg+Umschalt+R oder ein paar Minuten warten.
- Commit-Nachricht: „Version X: …“.

## Testen vor dem Hochladen
- jsdom-Simulationen: Spielzüge direkt aufrufen (`doDraw`, `doPlay`, `botDecide`), viele Bot-Runden pro Modus, Grenzen prüfen (Herzen, Handgröße), kein Hängenbleiben.
- Englisch-Check: Bot-Spiele laufen lassen und `tr()` auf alle Log-Einträge – keine deutschen Reste.
- Mehrere Geräte: Kopie der Seite mit lokalem MQTT-Broker (aedes) und Playwright, 3 Spieler + Bots.
- Screenshots (Handy-Breite 390 px) ansehen.

## Bilder
- Ischan schickt Meme-/Fotobilder für Karten. Keine sexualisierten Bilder und keine anzüglichen Fotos echter Personen einbauen (Repo und Seite sind öffentlich). Bei grenzwertigen Bildern lieber einen harmlosen Ausschnitt nehmen oder nach einem anderen Bild fragen.
- Ginger please und Goth Baddie haben noch kein Bild.
