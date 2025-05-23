﻿# Lernjournal 3 ONNX

## Übersicht

| | Bitte ausfüllen |
| -------- | ------- |
| ONNX Modell für Analyse (Netron) | https://github.com/onnx/models/blob/main/validated/vision/classification/efficientnet-lite4/model/efficientnet-lite4-11-int8.onnx |
| onnx-image-classification Fork (EfficientNet-Lite) | https://github.com/hariis-00/onnx-image-classification/blob/main/efficientnet-lite4-11.onnx |

## Dokumentation ONNX Analyse

In diesem Teil des Lernjournals beschäftigte ich mich mit dem Austausch und der Analyse von ONNX-Modellen inenrhalb einer bestehenden Anwendnung. Ziel war es, ein neues, quantisiertes Modell auszuwählen und dessen Architektur zu untersuchen, um ein besseres Verständnis für die Modellstrutkur zu gewinnen.

Gewähltes Modell:
Ich habe das Modell "efficientnet-lite4-int8.onnx" gewählt, welches im offiziellen ONNX Model Zoo zur Verfügung steht. Es handelt sich dabei um eine quantisierte Version des ursprünglich in der App verwendeten Modells "efficient-lite4-11.onxx". 

**Download Link:**

(https://github.com/onnx/models/blob/main/validated/vision/classification/efficientnet-lite4/model/efficientnet-lite4-11-int8.onnx)

Der Hauptunterschied zur Originalversion besteht darin, dass das Modell vollständig quantisiert wurde (int8), was zu einer signifikanten Reduktion von Speicherbedarf und Latenz führ - bei potenziell leichtem Genauigkeits verlust

**Layer 1:**

Zur Analyse des Modells habe ich das Tool Netron.app verwendet. Die grafische Darstellung zeigt die ersten Schritte des Netzwerks beginnen mit dem Input-Tensor images:0 mit Shape 1x224x224x3 welcher nach einer Transpose-Operation in das Netzwerk eingespeist wird.
<img src="images/lj3_jusmahar_onnx_netron1.png" alt="Web App" style="max-width: 100%; height: auto;">

**Layer 2:**

Dieser Layer nimmt das quantisierte Eingangsbild entgegen und führt eine klassische Faltung durch, jedoch diesmal vollständig im INT8-Raum
Ziel dieses Layers ist die Extraktion erster Feature Map, also einfach Textur-, Kanten- oder Farbkontraste werden herausgefiltert.
<img src="images/lj3_jusmahar_onnx_netron2.png" alt="Web App" style="max-width: 100%; height: auto;">

**Layer 3:**

Der nächste QLinearConv-Layer baut auf dem Ergebnis des vorherigen auf und verwendet einen neuen Satz quantisierter Filter mit anderen Skalenwerten. Beim dritten layer ist das Ziel die Erkennung mittelhohe Feature-Kombinationen wie z.B. Konturen, Übergänge oder symmetrische Strukturen. Dies ist ein Zwischenschritt bevor es in die Richtung semantischer Bildinhalte geht.
<img src="images/lj3_jusmahar_onnx_netron3.png" alt="Web App" style="max-width: 100%; height: auto;">


**Gesamtanalyse: was macht EfficientNet-Lite4 eigentlich?**
EfficientNet-Lite4 ist auf Effizienz optimiertes Convolutional Neural Network (CNN) für Bildklassifikation. Es basiert auf dem ursprünglichen EfficientNet von Google.

## Dokumentation onnx-image-classification

Als erstes habe ich das Repositoriy onnx-image-classification geforked und lokal zum laufen gebracht. Dies konnte ich via flask starten und konnte die ersten 3 Bilder, die im Repo hinterlegt waren mal austesten. Folgende Resultate habe ich erhalten:

<img src="images/lj3_jusmahar_onnx_lite4_car.png" alt="Web App" style="max-width: 100%; height: auto;">
<img src="images/lj3_jusmahar_onnx_lite4_matterhorn.png" alt="Web App" style="max-width: 100%; height: auto;">
<img src="images/lj3_jusmahar_onnx_lite4_train.png" alt="Web App" style="max-width: 100%; height: auto;">

Danach habe ich das Modell ausgewechselt und das neue Modell "efficientnet-lite4-int8" ins Repository hinzugefügt. Des Weiteren musste ich die App.py anpassen, sodass auf das neue Modell zugegriffen wird:
<img src="images/lj3_jusmahar_onnx_apppy.png" alt="Web App" style="max-width: 100%; height: auto;">

Sobald ich das Modell ausgetauscht habe, konnte ich wieder die Flask-Anwendung starten und habe die Resultate nochmals ausgewertet:
<img src="images/lj3_jusmahar_onnx_lite4-lit8_car.png" alt="Web App" style="max-width: 100%; height: auto;">
<img src="images/lj3_jusmahar_onnx_lite4-lit8_matterhorn.png" alt="Web App" style="max-width: 100%; height: auto;">
<img src="images/lj3_jusmahar_onnx_lite4-lit8_train.png" alt="Web App" style="max-width: 100%; height: auto;">

**Vergleich Modelversionen:**
Ich habe die gleiche Bildklassifikation mit beiden Modellversionen durchgeführt und Unterschiede in den Prediction-Werten beobachtet:

- **Beispiel `train.jpg`:**
  - **Alt:** Höchster Score 0.3470 (`streetcar, tram, …`)
  - **Neu:** Score bei 0.2526 – ebenfalls gleiche Klasse, aber etwas konservativer

- **Beispiel `car.jpg`:**
  - **Alt:** Höchster Score 0.2627 (`beach wagon`)
  - **Neu:** Deutlich höherer Score von 0.3709 für dieselbe Klasse → Modell wirkt sicherer

- **Beispiel `matterhorn.jpg`:**
  - **Alt:** Score für `lakeside` am höchsten mit 0.0131
  - **Neu:** Neue höchste Klasse ist `valley` mit 0.0113 – leicht andere Priorisierung

Insgesamt zeigen sich nur kleinere Unterschiede, was darauf hindeutet, dass die Modellstruktur und -gewichte im Wesentlichen gleich geblieben sind. Trotzdem war der das Testing sinnvoll, um sicherzugehen, dass das neue Modell wirklich geladen und verwendet wird.

**Push Github**
Zu guter Letzt habe ich die Veränderungen mit dem neuen Modell ins Github-Repo gepusht --> (https://github.com/hariis-00/onnx-image-classification)
