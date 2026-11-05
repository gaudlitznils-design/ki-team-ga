# Unser KI-Team

Microsite über das KI-Team der Familie: wer im Team ist, wie der Essensplan entsteht und welche Gerichte wir kochen.

- `index.html` ist die komplette Seite, ohne Build-Schritt. Einfach im Browser öffnen.
- Die Daten (Team, Gerichte, Verlauf) stehen als JSON im Block `<script type="application/json" id="kiteam-data">` am Ende der Datei. Stand: 05.11.2026.
- Quelle der Daten sind die Tabelle „KI-Team“ und die Gerichte-DB in Google Drive. Aktualisiert wird von Hand.
- Die Porträts liegen in `img/<skill>.jpg` (z. B. `img/kunibert.jpg`, 480 × 480 px). Fehlt ein Foto, zeigt die Seite die gezeichnete Figur.
