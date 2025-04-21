# Lernjournal 1 Python

## Repository und Library

| | Bitte ausfüllen |
| -------- | ------- |
| Repository (URL)  | https://github.com/hariis-00/SW2 |
| Kurze Beschreibung der App-Funktion | Es ist ein einfacher Taschenrechner, welcher die Addition und Subtraktion von 2 Zahlen durchführen kann.|
| Verwendete Library aus PyPi (Name) |blinker, click, flask, jinja2, markupsafe, werkzeug|
| Verwendete Library aus PyPi (URL) | https://pypi.org/project/blinker/1.9.0/ , https://pypi.org/project/click/8.1.8/ ,  https://pypi.org/project/Jinja2/3.1.5/ https://pypi.org/project/MarkupSafe/3.0.2 , https://pypi.org/project/Werkzeug/3.1.3/  |

## App, Funktionalität

Ziel war es, eine einfache Web-Applikation zu entwickeln, bei der zwei Zahlen eingegeben und anschließend addiert oder subtrahiert werden können. Die Umsetzung erfolgte mit **Flask (Backend)**, **Vanilla JS (Frontend-Logik)** sowie **Bootstrap (Styling)**.

Die Benutzeroberfläche (`index.html`) enthält:
- Zwei Eingabefelder
- Zwei Buttons für `+` und `−`
- Eine Ergebnisanzeige im Bootstrap-Style

Die Logik zur Kommunikation mit dem Backend ist in `script.js` implementiert. Sobald ein Button geklickt wird, sendet die `fetch()`-Funktion einen JSON-Request an die `/calculate`-Route im Flask-Backend.

Das Backend (`app.py`) verarbeitet diesen Request:
- Die Werte `num1`, `num2` und `operation` werden aus dem Request extrahiert
- Je nach Operation (`add` oder `subtract`) wird das Resultat berechnet
- Die Antwort erfolgt als JSON-Response zurück ans Frontend

## Dependency Management

Für das Dependency Management wird eine `requirements.txt` Datei verwendet, die alle notwendigen Bibliotheken und deren Versionen auflistet. Diese Datei ermöglicht es, die benötigten Abhängigkeiten einfach zu installieren, indem der Befehl `pip install -r requirements.txt` ausgeführt wird. Dadurch wird sichergestellt, dass alle Entwickler und Benutzer der Applikation die gleichen Versionen der Bibliotheken verwenden, was die Kompatibilität und Stabilität der Anwendung gewährleistet.

## Deployment

Das Deployment wurde auf zu einem auf Github gemacht und des anderen auf Azure. Somit kann das veröffentlichte Modell abgerufen.

