---
title: Einrichten von Traffic-Filterregeln einschließlich WAF-Regeln
description: Erfahren Sie, wie Sie die Ergebnisse von Traffic-Filterregeln einschließlich WAF-Regeln erstellen, bereitstellen, testen und analysieren.
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-20T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
source-git-commit: fa28ae232a5353eb34788fd2abe8402b42a62f66
workflow-type: tm+mt
source-wordcount: '563'
ht-degree: 3%

---


# Einrichten von Traffic-Filterregeln einschließlich WAF-Regeln

Lernen **Einrichtung** Traffic-Filterregeln, einschließlich WAF-Regeln. Erfahren Sie mehr über das Erstellen, Bereitstellen, Testen und Analysieren von Ergebnissen.

## Setup

Der Einrichtungsprozess umfasst Folgendes:

- _Erstellen von Regeln_ mit einer entsprechenden AEM Projektstruktur und Konfigurationsdatei.
- _Bereitstellen von Regeln_ über die Konfigurationspipeline von Adobe Cloud Manager.
- _Testregeln_ Verwendung verschiedener Tools zum Generieren von Traffic
- _Ergebnisanalyse_ Verwendung von AEMCS-CDN-Protokollen und Dashboard-Tools.

### Erstellen von Regeln in Ihrem AEM Projekt

Gehen Sie wie folgt vor, um Regeln zu erstellen:

1. Erstellen Sie auf der obersten Ebene Ihres AEM-Projekts einen Ordner `config`.

1. Innerhalb der `config` Ordner erstellen, erstellen Sie eine neue `cdn.yaml`.

1. Fügen Sie die folgenden Metadaten zum `cdn.yaml` Datei:

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
```

Ein Beispiel für `cdn.yaml` -Datei im WKND Sites-Projekt der AEM Guides:

![WKND AEM Projektregeldatei und -ordner](./assets/wknd-rules-file-and-folder.png)

### Bereitstellen von Regeln über Cloud Manager {#deploy-rules-through-cloud-manager}

Gehen Sie wie folgt vor, um Regeln bereitzustellen:

1. Melden Sie sich unter [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) bei Cloud Manager an und wählen Sie die entsprechende Organisation und das entsprechende Programm aus.

1. Navigieren Sie zum _Pipelines_ Karte aus der _Programmübersicht_ und klicken Sie auf **+Hinzufügen** und wählen Sie den gewünschten Pipeline-Typ aus.

   ![Cloud Manager Pipelines-Karte](./assets/cloud-manager-pipelines-card.png)

   Im obigen Beispiel dienen Demozwecke dazu _Hinzufügen einer produktionsfremden Pipeline_ ausgewählt ist, da eine Entwicklungsumgebung verwendet wird.

1. Im _Hinzufügen einer produktionsfremden Pipeline_ wählen Sie aus und geben Sie die folgenden Details ein:

   1. Konfigurationsschritt:

      - **Typ**: Implementierungs-Pipeline
      - **Pipeline-Name**: Dev-Config

      ![Dialogfeld &quot;Cloud Manager-Konfigurations-Pipeline&quot;](./assets/cloud-manager-config-pipeline-step1-dialog.png)

   2. Schritt Quellcode :

      - **Bereitzustellender Code**: Zielgerichtete Bereitstellung
      - **Einschließen**: Konfiguration
      - **Bereitstellungsumgebung**: Name Ihrer Umgebung, z. B. wknd-program-dev.
      - **Repository**: Das Git-Repository, aus dem die Pipeline den Code abrufen soll, z. B. `wknd-site`
      - **Git-Verzweigung**: Der Name der Git-Repository-Verzweigung.
      - **Code-Speicherort**: `/config`, der dem Konfigurationsordner der obersten Ebene entspricht, der im vorherigen Schritt erstellt wurde.

      ![Dialogfeld &quot;Cloud Manager-Konfigurations-Pipeline&quot;](./assets/cloud-manager-config-pipeline-step2-dialog.png)

### Testen von Regeln durch Traffic-Generierung

Zum Testen von Regeln stehen verschiedene Tools von Drittanbietern zur Verfügung und Ihre Organisation kann über ein bevorzugtes Tool verfügen. Verwenden wir für Demozwecke die folgenden Tools:

- [Curl](https://curl.se/) für grundlegende Tests wie das Aufrufen einer URL und das Überprüfen des Antwort-Codes.

- [Vegeta](https://github.com/tsenart/vegeta) zur Durchführung von Denial of Service (DOS). Befolgen Sie die Installationsanweisungen aus dem [Vegeta GitHub](https://github.com/tsenart/vegeta#install).

- [Nikto](https://github.com/sullo/nikto/wiki) um potenzielle Probleme und Sicherheitslücken wie XSS, SQL-Injection und mehr zu finden. Befolgen Sie die Installationsanweisungen aus dem [Nikto GitHub](https://github.com/sullo/nikto).

- Stellen Sie sicher, dass die Tools in Ihrem Terminal installiert und verfügbar sind, indem Sie die folgenden Befehle ausführen:

  ```shell
  # Curl version check
  $ curl --version
  
  # Vegeta version check
  $ vegeta -version
  
  # Nikto version check
  $ cd <PATH-OF-CLONED-REPO>/program
  ./nikto.pl -Version
  ```

### Ergebnisse mithilfe der Dashboard-Werkzeuge analysieren

Nach dem Erstellen, Bereitstellen und Testen der Regeln können Sie die Ergebnisse analysieren mithilfe von **Elasticsearch, Logstash und Kibana (ELK)** Dashboard-Tools. Es kann die AEMCS-CDN-Protokolle analysieren, sodass Sie die Ergebnisse in Form verschiedener Diagramme und Grafiken visualisieren können.

Dashboard-Tools können direkt aus dem [AEMCS-CDN-Log-Analysis-ELK-Tool GitHub-Repository](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool) und befolgen Sie die Schritte zum Installieren und Laden der **Traffic-Filterregeln (einschließlich WAF)** Dashboard.

- Nach dem Laden des Beispiel-Dashboards sollte Ihre Seite mit dem Tool für das Elastic-Dashboard wie folgt aussehen:

  ![Dashboard &quot;ELK-Traffic-Filter-Regeln&quot;](./assets/elk-dashboard.png)

>[!NOTE]
>
>    Da noch keine AEMCS-CDN-Protokolle erfasst wurden, ist das Dashboard leer.


## Nächster Schritt

Erfahren Sie, wie Sie Traffic-Filterregeln deklarieren, einschließlich WAF-Regeln im [Beispiele und Ergebnisanalyse](./examples-and-analysis.md) -Kapitel, unter Verwendung des AEM WKND Sites-Projekts.
