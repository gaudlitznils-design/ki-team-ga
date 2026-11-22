# Microsite „Unser KI-Team“

Statische Seite für GitHub Pages, ohne Build-Schritt. Inhalt und Design sind getrennt:
`index.html` enthält nur Darstellung und Logik, alle Inhalte stehen in `data/`, die Bilder in `img/`.

## Regel

**data/ und img/ pflegt der Teamchef bzw. Media-Mike, Design nur in index.html.**

Neue Teammitglieder, Texte, Farben oder Gerichte also nie in `index.html` eintragen, sondern in den JSON-Dateien.
Umgekehrt gehören Layout, Farben der Seite selbst und Verhalten nicht in `data/`.

## Dateistruktur

```
index.html          Seite: HTML, CSS und JavaScript (lädt data/*.json per fetch)
data/team.json      Teammitglieder (Organigramm, Detailansicht, Köpfe oben)
data/gerichte.json  Gerichte-Liste; verlauf nur Ersatz für den Live-Wochenplan
img/<skill>.jpg     Porträts, eins je Teammitglied
.nojekyll           GitHub Pages ohne Jekyll ausliefern
README.md           Kurzbeschreibung
CLAUDE.md           diese Datei
```

## data/team.json

```json
{
  "stand": "YYYY-MM-DD",
  "team": [
    {
      "name": "Kunibert",
      "skill": "kunibert",
      "rolle": "Unser Koch",
      "farbe": "#D9861C",
      "bild": "img/kunibert.jpg",
      "kurz": "Ein Satz für die Karte.",
      "stimme": "Typischer Spruch, erscheint als Sprechblase in der Detailansicht.",
      "faehigkeiten": [
        { "name": "Woche planen", "text": "Ein Satz in Ich-Form." }
      ],
      "beispiel": "Beispielsatz, wie man ihn anspricht."
    }
  ]
}
```

- `stand`: Datum der letzten Änderung, erscheint unten auf der Seite.
- `skill`: Kennung in Kleinbuchstaben mit Bindestrich, z. B. `media-mike`. Daraus werden der Bildname und der Seitenanker `#p-<skill>`.
- Alle Einträge außer dem Teamchef stehen in der Reihenfolge der Datei oben im Organigramm: höchstens 4 Karten pro Reihe, die letzte Reihe mittig (auf dem Handy untereinander). Dahinter hängt die Seite automatisch die Platzhalter-Karte „Noch frei“ an.
- Der Eintrag mit `"skill": "teamchef"` steht, egal wo in der Datei, mit Abstand darunter. Bei einer Reihe führt von jeder Karte eine Linie zu ihm, bei mehreren Reihen eine Klammer unter allen Karten. Auf dem Handy gibt es keine Linien.
- Beliebig viele Mitglieder sind möglich, am Layout muss dafür nichts geändert werden.
- `farbe`: Teamfarbe als `#RRGGBB`. Die Seite leitet daraus Schrift-, Flächen- und Dunkelmodus-Farbe ab.
- `bild`: Pfad zum Porträt oder `null`. Ohne Bild erscheint ein neutraler Kreis mit dem Anfangsbuchstaben in der Teamfarbe.
- Vergebene Farben: teamchef `#6D4FA0`, kunibert `#D9861C`, gustav `#23847E`, weihnachtself `#C0392B`, media-mike `#2F6FB5`.

## data/gerichte.json

```json
{
  "stand": "YYYY-MM-DD",
  "gerichte": [
    {
      "n": "Pasta Verdura",
      "text": "Kurzbeschreibung",
      "status": "bewährt",
      "beliebt": "Favorit",
      "koch": "Nils",
      "dauer": 45,
      "rezept": "<Google-Doc-ID oder leer>",
      "zuletzt": "YYYY-MM-DD"
    }
  ],
  "verlauf": [
    { "d": "YYYY-MM-DD", "g": "Gericht", "v": "Variante oder leer", "koch": "Miriam" }
  ]
}
```

- Die Seite nutzt bei Gerichten: `n` (Name, Pflicht), `text`, `status` (`"offen"` = neu zum Ausprobieren, sonst bekannt), `beliebt` (`"Favorit"` = Liebling), `koch` (`"beide"` wird zu „kochen beide“), `dauer` (Minuten), `rezept` (ID eines Google Docs, ergibt den Link „Rezept“), `zuletzt` (Sortierung).
- Weitere Felder aus der Gerichte-DB (`aufwand`, `solo`, `saison`, `basis`, `eiweiss`, `varianten`) dürfen drinstehen, werden aber nicht angezeigt.
- `verlauf`: nur noch der **Ersatz**, falls der Live-Wochenplan (siehe unten) nicht lädt. Dann zeigt die Seite wie früher die Woche Freitag bis Donnerstag, in die der letzte Eintrag fällt.
- `rezept` wird auch im Wochenplan genutzt: Steht ein Gericht dort mit gleichem Namen wie in `gerichte`, erscheint daneben der Link „Rezept“.

## Wochenplan (live aus Google Sheets)

Die Wochenansicht im Abschnitt Essensplan lädt den Plan live aus dem im Web veröffentlichten Tab **Verlauf** der Gerichte-DB:

https://docs.google.com/spreadsheets/d/e/2PACX-1vTLKSD6NGHIiTl33GfQa0dpWdMLv830pj5JikuVwsweEjrAvn_FExhWzu8eTBkGk828w16RplYroPI2/pub?gid=1003&single=true&output=csv

(Ansicht als Tabelle: dieselbe Adresse mit `pubhtml` statt `pub` und ohne `&output=csv`.) Die Adresse steht in `index.html` als `PLAN_CSV`.

- Angezeigt wird die Woche Freitag bis Donnerstag, in der heute liegt, mit heute hervorgehoben. Steht für die nächste Woche schon etwas im Verlauf, kommt sie darunter.
- Pro Tag: Gericht mit Variante, Koch, darunter klein die Zutaten. Andere Mahlzeiten als Abendessen bekommen ein kleines Etikett (z. B. „WE-Mittag“). Ein Donnerstag ohne Eintrag zeigt „Reste“.
- Die Seite liest die Spalten über die Kopfzeile: `Datum`, `Wochentag`, `Mahlzeit`, `Gericht`, `Variante`, `Koch`, `Neu?`, `Zutaten`. **Diese Spaltennamen im Tab Verlauf dürfen sich nicht ändern.** Die Reihenfolge der Spalten ist egal, neue Spalten stören nicht.
- `Datum` muss als TT.MM.JJJJ kommen. Zeilen ohne gültiges Datum oder ohne Gericht werden übersprungen.
- Lädt die CSV nicht (offline, gesperrt, nach 8 Sekunden keine Antwort), nimmt die Seite `verlauf` aus `data/gerichte.json`.
- Google aktualisiert die veröffentlichte CSV mit einigen Minuten Verzögerung.

## Bilder

- Format: **512 × 512 Pixel, JPG**, quadratisch, Gesicht mittig (wird rund zugeschnitten).
- Dateiname: `img/<skill>.jpg`, in `data/team.json` unter `bild` eintragen.
- Möglichst unter 60 KB halten (JPG-Qualität um 80).

## Lokal testen

Die Seite lädt ihre Daten per `fetch`, direkt als Datei geöffnet (`file://`) funktioniert das nicht. Im Repo-Ordner starten:

```bash
python3 -m http.server
```

Dann http://localhost:8000 im Browser öffnen. Prüfen: Organigramm, Detailansicht (Klick auf eine Karte), Wochenplan (live aus Google Sheets, sonst der Ersatz), Handy-Breite, Dunkelmodus.
