# Projekt 1 Python

## Übersicht

| | Bitte ausfüllen |
| -------- | ------- |
| Variante | Eigenes Projekt (Booking.com) |
| Datenherkunft | Format (HTML, XML, JSON, ...), Beschreibung |
| Datenherkunft | Booking.com |
| ML-Algorithmus | Polynominale Regression |
| Repo URL | URL |

## Dokumentation

### Data Scraping

Der erste Schritt des Projekts bestand darin, Hoteldaten von Booking.com zu scrapen.

<img src="images/booking.png" alt="Web App" style="max-width: 100%; height: auto;">

Dazu wurde die Python Library **Playwright** verwendet, da sie im Vergleich zu Selenium moderner, schneller und robuster gegenüber dynamischen Webseiteninhalten ist. Des Weitern konnte das Scraping mit scrapy nicht durchgeführt werden, da die Spiders meist fehlerhaft waren. 

<img src="images/playwright.png" alt="Web App" style="max-width: 100%; height: auto;">

Im Script **bookingscraper.py** wurde eine For-Schleife über 10 Ergebnisseiten aufgebaut. Bei rund 25 Hotelanzeigen macht das 250 Hotelangebote. Für jedes Hotel wurden die wichtigsten Mermale extrahiert:
- Hotelname
- Preis
- Bewertungsscore
- Anzahl Reviews
- Ort/Adresse

<img src="images/scraper1.png" alt="Web App" style="max-width: 100%; height: auto;">
<img src="images/forschleife.png" alt="Web App" style="max-width: 100%; height: auto;">

Das Scraping dauerte einige wenige Minuten, sodass ich mich für eine eher kleinere Anzahl entschieden habe, sodass wir noch genug Daten für eine ML-Anwendung haben.

Ich habe auch bereits im Script festgelegt, welche Daten dabei berücksichtigt werden, nämlich der Zeitraum von 09.08.2025 bis 17.08.2025, also typische Sommerferien im Kanton Zürich. 

<img src="images/buchungsdatum.png" alt="Web App" style="max-width: 100%; height: auto;">

Die Daten wurden anschliessend in ein Excel- und CSV-Format exportiert, sodass sie später in eine Datenbank (in meinem Fall MongoDB) abgespeichert werden konnten. 
<img src="images/exportcsv.png" alt="Web App" style="max-width: 100%; height: auto;">
<img src="images/csvdaten.png" alt="Web App" style="max-width: 100%; height: auto;">

Damit die Daten auch von der CSV-Datei korrekt in die MongoDB geladen werden, habe ich ein Python-Script erstellt. Via MONGO_URI kann ich auf meine "Booking" Datenbank zugreifen und die Daten in meine "Hotel" Collection einfügen. 
<img src="images/mongodbpipeline.png" alt="Web App" style="max-width: 100%; height: auto;">

Nach wenigen Sekunden waren die Daten in der MongoDB abgelegt:
<img src="images/mongodbansicht.png" alt="Web App" style="max-width: 100%; height: auto;">

### Training

Das Machine Learning Modell wird im Script `machinelearningmodel.py` trainiert. Es basiert auf den aus MongoDB geladenen Daten. Die wesentlichen Schritte:

1. **Datenbereinigung**:
   - Preise wurden von „CHF“-Symbolen befreit
   - Reviews als Integer gespeichert
   - Score als Float interpretiert

2. **Feature Engineering**:
   - Eingesetzte Features: `score` & `reviews count`
   - Zielwert: `price`

3. **Modellwahl**:
   - Ein **Polynomial Regressionsmodell (Grad 2)** wurde über `sklearn.pipeline` implementiert.
   - Die Wahl fiel darauf, da lineare Zusammenhänge nicht ausreichten, um nicht-lineare Muster (z. B. starke Preise bei hohen Scores) zu erfassen.

4. **Evaluierung**:
   - Verwendete Metriken: R²-Score, MSE, MSLE, Median Absolute Error
   - Die Metriken wurden ausgegeben und dienten zur Beurteilung der Modellgüte.

5. **Speicherung**:
   - Das Modell wurde mit `joblib` als `.pkl` gespeichert und optional im Azure Blob Storage versioniert hochgeladen.

### ModelOps Automation

* [ ] TODO

### Deployment

* [ ] TODO
