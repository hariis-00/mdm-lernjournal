﻿# Lernjournal 2 Container

## Docker Web-Applikation

### Verwendete Docker Images

| | Bitte ausfüllen |
| -------- | ------- |
| Image 1 | Metabase |
| Image 1 | https://hub.docker.com/r/metabase/metabase |
| Image 2 | PostgreSQL |
| Image 2 | https://hub.docker.com/_/postgres |
| Docker Compose | https://hub.docker.com/r/hariis/onnx-image-classification |

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
2. Das System habe ich anschliessend mit dem Befehl:
```bash
docker-compose up -d` gestartet. Alternativ kann das System via Rechtsklick auf dem Docker-compose File gestartet werden.
```
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
| onnx-sentiment-analysis | Nein |
| onnx-image-classification | Ja |
| Repo URL Fork | https://github.com/mosazhaw/onnx-image-classification.git |
| Docker Hub URL | https://hub.docker.com/r/hariis/onnx-image-classification |

### Dokumentation lokales Deployment

Im zweiten Teil des Lernjournals habe ich das Beispiel onnx-image-classification auf Github geforkt und lokal geklont. Das Projekt enthält bereits ein funktionierendes Dockerfile, welches die benötigten Dateien (ONNX-Modell, labels_map.txt, app.py, Web-UI etc.) sowie die benötigten Python Librarys (requirements.txt) definiert.

Schritte:
1. Fork auf Github von onnx-image-classfication
2. Projekt in VS-Code geöffnet und ins Dockerfile gewechselt
3. Terminal geöffnet und mittels Dockerfile ein Image in mein Docker Hub (hariis) erstellt mittels: `docker build -t hariis/onnx-image-classification:latest .`
<img src="images/lj2_jusmahar_docker_build.png" alt="Web App" style="max-width: 100%; height: auto;">
4. Ich konnte das Image nun in meinem Docker Hub ansehen:
<img src="images/lj2_jusmahar_docker_ansichtdocker2.png" alt="Web App" style="max-width: 100%; height: auto;">
5. Anschliessend habe ich den Container lokal gestartet. Port 5000 im Container wurde auf Port 9000 von meinem Rechner gemappt:
<img src="images/lj2_jusmahar_docker_run9000.png" alt="Web App" style="max-width: 100%; height: auto;">
6. Die Anwendung war im Anschluss direkt über den Browser unter localhost:9000 erreichbar. Die Benutzeroberfläche wurde korrekt geladen und ich konnte mittels einem Bild eines Elefanten auch das Testing der Webapplikation durchführen.
<img src="images/lj2_jusmahar_docker_webapponnx.png" alt="Web App" style="max-width: 100%; height: auto;">
7. Nach einem erfolgreichen Testlauf habe ich das lokale Image auf Docker Hub veröffentlicht:
<img src="images/lj2_jusmahar_docker_dockerhub.png" alt="Web App" style="max-width: 100%; height: auto;">
<img src="images/lj2_jusmahar_docker_dcokerhub1.png" alt="Web App" style="max-width: 100%; height: auto;">

### Dokumentation Deployment Azure Web App

Im Anschluss an den lokalen Build und Push meines ONNX Image Classfication Containeres auf Docker Hub habe ich das Image in einer Azure Web App deployed.

Vorgehen:
1. Ressourcengruppe erstellt via:
```bash
az group create --name mdm-lj2-rg --location switzerlandnorth
```
<img src="images/lj2_jusmahar_azure_createrg2.png" alt="Web App" style="max-width: 100%; height: auto;">
3. Erstellung des App Service Plan (F1):
<img src="images/lj2_jusmahar_azure_createappservice2.png" alt="Web App" style="max-width: 100%; height: auto;">
4. Web App mit Docker Image deployen
<img src="images/lj2_jusmahar_azure_createwebapp2.png" alt="Web App" style="max-width: 100%; height: auto;">
5. Das Deployment wurde erfolgreich durchgeführt und ist via (https://haris-onnx-imageapp.azurewebsites.net) abrufbar. 
<img src="images/lj2_jusmahar_azure_ansichtazure.png" alt="Web App" style="max-width: 100%; height: auto;">

Besonderheiten:
1. Da ich mein Image auf einem ARM-basierten Mac (M4) gebaut hatte, musste ich es mit dem Zusatz `--platform linux/amd64` via `docker buildx` neu erstellen, da Azure Web Apps nur amd64 unterstützt.
2. Im Azure-Portal wurde zu Beginn beim Status "Kontingent überschritten" angezeigt. Das liegt daran, dass der F1-Plan nur 1 GB & 1 Web App gleichzeitig erlaubt. Gem. Logs ist jedoch das Deployment korrekt verlaufen. Schlussendlich musste man ein paar Stunden abwarten, bis wieder der Speicher frei war.

### Dokumentation Deployment ACA

In einem zweiten Schritt habe ich das Deployment via Azure Container Apps (ACA) durchgeführt. Ziel war es, mein Docker-Image hariis/onnx-image-classification:latest, welches zuvor auf Docker Hub veröffentlicht wurde, in eine öffentlich erreichbare Containerumgebung zu deployen.

Vorgehen:
1. Container App Umgebung erstellen
<img src="images/lj2_jusmahar_azure_aca_create.png" alt="Web App" style="max-width: 100%; height: auto;">
2. Container App mit Docker-Image deployen (Ich habe hier einen Access Token generieren lassen für Docker, ohne das funktioniert es nicht)
<img src="images/lj2_jusmahar_azure_aca_deployment.png" alt="Web App" style="max-width: 100%; height: auto;">
3. Die Anwendung konnte nach ein paar Sekunden via https://haris-onnx-aca.delightfulwave-fba05301.switzerlandnorth.azurecontainerapps.io geöffnet werden. Ich habe zur Sicherheit nochmals getestet, ob es eine Bilderkennung funktioniert:
<img src="images/lj2_jusmahar_azure_aca_frontend.png" alt="Web App" style="max-width: 100%; height: auto;">

Besonderheiten:
1. Anscheinend muss man die Lizenz via:
```bash
az provider register -n Microsoft.OperationalInsights --wait
```
noch wie "aktiviert" werden, da ansonsten die Erstellung der Container App Umgebung nicht funktioniert.

Übersicht Azure Container Apps:
<img src="images/lj2_jusmahar_azure_aca_ansichtazure.png" alt="Web App" style="max-width: 100%; height: auto;">


### Dokumentation Deployment ACI

Im dritten und letzten Schritt habe ich die ONNX-Image-Classification-Anwendung in Azure Container Instances (ACI) deployed. Das Ziel war, mein Docker-Image hariis/onnx-image-classification:latest in eine ACI-Umgebung zu deployen und über eine öffentliche URL via HTTP erreichbar zu machen.

Vorgehen:
1. Container create von ACI (Wichtig: Ich musste auch die CPU & RAM zuweisen, dass es funktioniert)
<img src="images/lj2_jusmahar_azure_aci_create.png" alt="Web App" style="max-width: 100%; height: auto;">
2. Der Container wurde nach 1-2 Minuten erstellt und konnte über http://haris-onnx-aci.switzerlandnorth.azurecontainer.io:5000
<img src="images/lj2_jusmahar_azure_aci_frontend.png" alt="Web App" style="max-width: 100%; height: auto;">

Besonderheiten:
1. Anders als bei Web App oder ACA muss man hier bereits explizit angeben wie viel CPU und RAM zugewiesen werden sollte.
2. Auch bei ACI muss ein DockerHub-Login erfolgen, selbst wenn das Image Öffentlich zugänglich ist
3. Man gibt bereits den gewünschten Port an, wo man die Applikation aufrufen möchte

Übersicht Azure Container Instances:
<img src="images/lj2_jusmahar_azure_aci_ansichtazure.png" alt="Web App" style="max-width: 100%; height: auto;">

