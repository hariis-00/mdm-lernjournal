# Lernjournal 2 Container

## Docker Web-Applikation

### Verwendete Docker Images

| | Bitte ausfüllen |
| -------- | ------- |
| Image 1 | Metabase |
| Image 1 | https://hub.docker.com/r/metabase/metabase |
| Image 2 | PostgreSQL |
| Image 2 | (https://hub.docker.com/_/postgres) |
| Docker Compose | [URL Github Repo](https://github.com/hariis-00/jusmahar_dockercompose/tree/main) |

### Dokumentation manuelles Deployment

Inital habe ich versucht die Anwendung (Metabase + PostgreSQL) manuell einzurichten via Netzwerke. Dabei wurden die beiden Container gezielt über ein gemeinsames Docker-Netzwerk verbunden. Ziel war es, das Zusammenspiel von Images, Container und Docker-Netzwerken besser zu verstehen und kontrolliert zu testen.

Schritte:
1. Zuerst wurde ein separates Netzwerk erstellt, damit sich die Container gegenseitig per Namen erreichen können.
<img src="images/lj2_jusmahar_docker_createnetwork.png" alt="Web App" style="max-width: 100%; height: auto;">
2. Der PostgreSQL-Container wurde im Hintergrund gestartet und direkt mit dem Netzwerk `metabase-net` verbunden. Die Datebank wird beim Start automatisch konfiguriert.
<img src="images/lj2_jusmahar_docker_connectnetwork.png" alt="Web App" style="max-width: 100%; height: auto;">
3. Im zweiten Schritt wurde der Metabase-Container gestartet - ebenfalls im gleichen Netzwerk. Über die Umgebungsvariablen wird die PostgreSQL-Datenbank als Backend angegeben.
<img src="images/lj2_jusmahar_docker_connectlocalhost.png" alt="Web App" style="max-width: 100%; height: auto;">
4. Nun sind im Docker die beiden Images ersichtlich und der Connection-String zum Localhost:3000 wird angegeben.
<img src="images/lj2_jusmahar_docker_ansichtdocker1.png" alt="Web App" style="max-width: 100%; height: auto;">


### Dokumentation Docker-Compose Deployment

Für die Docker-Compose Deployment habe ich ein Beispiel mit 2 Containern umgesetzt: Metabase (Analyse- und Visualisierungsplattform) und PostreSQL (Datenbank)

Vorgehen:
Die gesamte Konfiguration erfolgte über eine docker-compose.yml Datei, die beide Container definiert. PostgreSQL wurde so konfiguriert, dass eine Datenbank namens metabase_db beim Start automatisch angelegt wird. Metabase wurde so konfiguriert, dass es diese Datenbank als interne Konfigurationsdatenbank für Metabase fungiert.

Schritte:
1. Ich habe folglich ein Ordner im Visual Studio Code eröffnet, in dem ich meine docker-compose.yml angesetzt habe. Ich habe dort festgelegt, wie die beiden Images ausgeführt werden sollten, wenn ich Compose Up ausführe.
2. Das System habe ich anschliessend mit dem Befehl `docker-compose up -d` gestartet. Alternativ kann das System via Rechtsklick auf dem Docker-compose File gestartet werden.
<img src="images/lj2_jusmahar_docker_composeup.png" alt="Web App" style="max-width: 100%; height: auto;">
3. Nach wenigen Sekunden war die Metabase-Obefläche unter `localhost:3000` erreichbar. Beim ersten Start durchlief ich den Initialisierungsprozess im Browser (Benutzername, Passwort, Verbindung zur DB). Anschliessend war die Anwendung einsatzbereit.
<img src="images/lj2_jusmahar_docker_metabase.png" alt="Web App" style="max-width: 100%; height: auto;">

Erkenntnisse aus der Vorgehensweise:
1. Durch das `docker-compose.yml` lassen sich beide Container nahtlos betreiben.
2. Die Anwendung lässt sich in 2-3 Minuten aufsetzen, sieht aber wie eine professionelle BI-Anwendung aus, die auch für echte Use-Cases verwendet werden kann.

Ansicht Docker:

<img src="images/lj2_jusmahar_docker_ansichtdocker.png" alt="Web App" style="max-width: 100%; height: auto;">

## Deployment ML-App

### Variante und Repository

| Gewähltes Beispiel | Bitte ausfüllen |
| -------- | ------- |
| onnx-sentiment-analysis | Ja/Nein |
| onnx-image-classification | Ja/Nein |
| Repo URL Fork | URL |
| Docker Hub URL | URL |

### Dokumentation lokales Deployment

* [ ] TODO

### Dokumentation Deployment Azure Web App

* [ ] TODO

### Dokumentation Deployment ACA

* [ ] TODO

### Dokumentation Deployment ACI

* [ ] TODO
