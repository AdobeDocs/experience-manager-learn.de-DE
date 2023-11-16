---
title: Analyse der CDN-Cache-Trefferquote
description: Erfahren Sie, wie Sie die AEM as a Cloud Service bereitgestellten CDN-Protokolle analysieren. Erhalten Sie Einblicke wie das Cache-Trefferverhältnis und die Top-URLs der Cache-Typen MISS und PASS für Optimierungszwecke.
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-11-10T00:00:00Z
jira: KT-13312
thumbnail: KT-13312.jpeg
source-git-commit: 4e93bc88b0ee5a805f2aaf1e66900f084ae01247
workflow-type: tm+mt
source-wordcount: '1433'
ht-degree: 2%

---


# Analyse der CDN-Cache-Trefferquote

Inhalte, die im CDN zwischengespeichert werden, reduzieren die Latenz, mit der Website-Benutzer konfrontiert sind, die nicht warten müssen, bis die Anforderung wieder zum Apache/Dispatcher oder AEM Publish gelangt. Vor diesem Hintergrund ist es sinnvoll, das CDN-Cache-Trefferverhältnis zu optimieren, um die im CDN zwischenspeicherbare Menge an Inhalten zu maximieren.

Erfahren Sie, wie Sie die bereitgestellten AEM as a Cloud Service analysieren können. **CDN-Protokolle** und gewinnen Einblicke wie **Cache-Trefferverhältnis**, und **Top-URLs von _FEHLER_ und _PASS_ Cache-Typen** zu Optimierungszwecken.


Die CDN-Protokolle sind im JSON-Format verfügbar, das verschiedene Felder enthält, darunter `url`, `cache`. Weitere Informationen finden Sie unter [CDN-Protokollformat](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/logging.html?lang=en#cdn-log:~:text=Toggle%20Text%20Wrapping-,Log%20Format,-The%20CDN%20logs). Die `cache` -Feld enthält Informationen zu _Status des Caches_ und die möglichen Werte sind HIT, MISS oder PASS. Sehen wir uns die Details möglicher Werte an.

| Cache-Status </br> Möglicher Wert | Beschreibung |
|------------------------------------|:-----------------------------------------------------:|
| HIT | Die angeforderten Daten sind _im CDN-Cache gefunden werden und keinen Abruf erfordern_ Anfrage an den AEM Server. |
| FEHLER | Die angeforderten Daten sind _nicht im CDN-Cache gefunden und muss angefordert werden_ vom AEM Server aus. |
| PASS | Die angeforderten Daten sind _explizit auf Nicht zwischenspeichern gesetzt_ und immer vom AEM-Server abgerufen werden. |

Im Rahmen dieses Tutorials wird die Variable [AEM WKND-Projekt](https://github.com/adobe/aem-guides-wknd) wird in der AEM as a Cloud Service Umgebung bereitgestellt und ein kleiner Leistungstest wird ausgelöst durch [Apache JMeter](https://jmeter.apache.org/).

Dieses Tutorial ist so strukturiert, dass Sie den folgenden Prozess durchlaufen:
1. Herunterladen von CDN-Protokollen über Cloud Manager
1. Analysieren Sie diese CDN-Protokolle, die mit zwei Ansätzen ausgeführt werden können: einem lokal installierten Dashboard oder einem remote auf Jupityer Notebook (für diejenigen, die Adobe Experience Platform lizenzieren) zugänglichen Jupityer-Notebook.
1. CDN-Cache-Konfiguration optimieren

## CDN-Protokolle herunterladen

Gehen Sie wie folgt vor, um die CDN-Protokolle herunterzuladen:

1. Melden Sie sich bei Cloud Manager an unter [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) und wählen Sie Ihre Organisation und das Programm aus.

1. Wählen Sie für eine gewünschte AEMCS-Umgebung **Protokolle herunterladen** aus dem Menü mit den Auslassungspunkten.

   ![Protokolle herunterladen - Cloud Manager](assets/cdn-logs-analysis/download-logs.png){width="500" zoomable="yes"}

1. Im **Protokolle herunterladen** wählen Sie das **Veröffentlichen** Dienst aus dem Dropdown-Menü und klicken Sie dann auf das Download-Symbol neben dem **cdn** Zeile.

   ![CDN-Protokolle - Cloud Manager](assets/cdn-logs-analysis/download-cdn-logs.png){width="500" zoomable="yes"}


Wenn die heruntergeladene Protokolldatei von _heute_ Die Dateierweiterung lautet `.log` Andernfalls für vergangene Protokolldateien ist die Erweiterung `.log.gz`.

## Heruntergeladene CDN-Protokolle analysieren

Um Einblicke wie das Cache-Trefferverhältnis und die Top-URLs der Cache-Typen MISS und PASS zu erhalten, analysieren Sie die heruntergeladene CDN-Protokolldatei. Diese Einblicke helfen bei der Optimierung der [CDN-Cache-Konfiguration](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=de) und die Site-Leistung verbessern.

Um die CDN-Protokolle zu analysieren, bietet dieser Artikel zwei Optionen: die **Elasticsearch, Logstash und Kibana (ELK)** [Dashboard-Tools](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool) und [Jupyter Notebook](https://jupyter.org/). Die ELK-Dashboard-Tools können lokal auf Ihrem Laptop installiert werden, während auf die Jupityr-Notebook-Tools remote zugegriffen werden kann [als Teil von Adobe Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/data-science-workspace/jupyterlab/analyze-your-data.html?lang=en) ohne zusätzliche Software zu installieren, für diejenigen, die Adobe Experience Platform lizenziert haben.


### Option 1: Verwenden der Werkzeuge des ELK-Dashboards

Die [ELK-Stapel](https://www.elastic.co/elastic-stack) ist ein Satz von Tools, die eine skalierbare Lösung für die Suche, Analyse und Visualisierung der Daten bieten. Es besteht aus Elasticsearch, Logstash und Kibana.

Um die Schlüsseldetails zu identifizieren, verwenden wir die [AEMCS-CDN-Log-Analysis-ELK-Tool](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool) Dashboard-Tool-Projekt. Dieses Projekt stellt einen Docker-Container des ELK-Stapels und ein vorkonfiguriertes Kibana-Dashboard zur Analyse der CDN-Protokolle bereit.

1. Führen Sie die Schritte aus [Einrichten des ELK-Docker-Containers](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool#how-to-set-up-the-elk-docker-container) und importieren Sie die **CDN-Cache-Trefferverhältnis** Kibana-Dashboard.

1. Gehen Sie wie folgt vor, um das CDN-Cache-Trefferverhältnis und die Top-URLs zu identifizieren:

   1. Kopieren Sie die heruntergeladenen CDN-Protokolldateien in den umgebungsspezifischen Ordner.

   1. Öffnen Sie die **CDN-Cache-Trefferverhältnis** Dashboard durch Klicken auf das Navigationsmenü oben links im Bildschirm > Analysen > Dashboard > CDN-Cache-Trefferverhältnis.

      ![CDN-Cache-Trefferverhältnis - Kibana-Dashboard](assets/cdn-logs-analysis/cdn-cache-hit-ratio-dashboard.png){width="500" zoomable="yes"}

   1. Wählen Sie oben rechts den gewünschten Zeitraum aus.

      ![Zeitbereich - Kibana-Dashboard](assets/cdn-logs-analysis/time-range.png){width="500" zoomable="yes"}

   1. Die **CDN-Cache-Trefferverhältnis** Das Dashboard erklärt sich von selbst.

   1. Die _Gesamtanfrageanalyse_ zeigt die folgenden Details an:
      - Cache-Verhältnisse nach Cache-Typ
      - Cache-Zählungen nach Cache-Typ

      ![Analyse der Anfragen insgesamt - Kibana-Dashboard](assets/cdn-logs-analysis/total-request-analysis.png){width="500" zoomable="yes"}

   1. Die _Analyse nach Anforderung oder MIME-Typen_ zeigt die folgenden Details an:
      - Cache-Verhältnisse nach Cache-Typ
      - Cache-Zählungen nach Cache-Typ
      - Top-MISS- und PASS-URLs

      ![Analyse nach Anforderung oder MIME-Typen - Kibana-Dashboard](assets/cdn-logs-analysis/analysis-by-request-or-mime-types.png){width="500" zoomable="yes"}

#### Filtern nach Umgebungs- oder Programm-ID

Gehen Sie wie folgt vor, um die erfassten Protokolle nach Umgebungsnamen zu filtern:

1. Klicken Sie im Dashboard &quot;CDN Cache Hit Ratio&quot;auf das **Filter hinzufügen** Symbol.

   ![Filter - Kibana-Dashboard](assets/cdn-logs-analysis/filter.png){width="500" zoomable="yes"}

1. Im **Filter hinzufügen** modal, wählen Sie die `aem_env_name.keyword` aus dem Dropdown-Menü ein und `is` Operator und gewünschter Umgebungsname für das nächste Feld und klicken Sie schließlich auf _Filter hinzufügen_.

   ![Filter hinzufügen - Kibana-Dashboard](assets/cdn-logs-analysis/add-filter.png){width="500" zoomable="yes"}

#### Filtern nach Hostnamen

Gehen Sie wie folgt vor, um die erfassten Protokolle nach Hostnamen zu filtern:

1. Klicken Sie im Dashboard &quot;CDN Cache Hit Ratio&quot;auf das **Filter hinzufügen** Symbol.

   ![Filter - Kibana-Dashboard](assets/cdn-logs-analysis/filter.png){width="500" zoomable="yes"}

1. Im **Filter hinzufügen** modal, wählen Sie die `host.keyword` aus dem Dropdown-Menü ein und `is` Operator und gewünschter Hostname für das nächste Feld und klicken Sie schließlich auf _Filter hinzufügen_.

   ![Host Filter - Kibana-Dashboard](assets/cdn-logs-analysis/add-host-filter.png){width="500" zoomable="yes"}

Fügen Sie dem Dashboard entsprechend den Analyseanforderungen weitere Filter hinzu.

### Option 2: Verwenden von Jupyter Notebook

Für diejenigen, die die Software lieber nicht lokal installieren möchten (d. h. die ELK-Dashboard-Tools aus dem vorherigen Abschnitt), gibt es eine andere Option, für die jedoch eine Lizenz für Adobe Experience Platform erforderlich ist.

Die [Jupyter Notebook](https://jupyter.org/) ist eine Open-Source-Webanwendung, mit der Sie Dokumente erstellen können, die Code, Text und Visualisierung enthalten. Sie wird für die Datenumwandlung, Visualisierung und statistische Modellierung verwendet. Es kann remote aufgerufen werden [als Teil von Adobe Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/data-science-workspace/jupyterlab/analyze-your-data.html?lang=en).

#### Herunterladen der interaktiven Python-Notebook-Datei

Laden Sie zunächst die [AEM-as-a-CloudService - CDN Logs Analysis - Jupyter Notebook](./assets/cdn-logs-analysis/aemcs_cdn_logs_analysis.ipynb) -Datei, die bei der Analyse der CDN-Protokolle hilft. Diese &quot;Interactive Python Notebook&quot;-Datei ist selbsterklärend. Die wichtigsten Highlights jedes Abschnitts sind jedoch:

- **Zusätzliche Bibliotheken installieren**: installiert die `termcolor` und `tabulate` Python-Bibliotheken.
- **CDN-Protokolle laden**: lädt die CDN-Protokolldatei mit `log_file` -Wert. Achten Sie darauf, den zugehörigen Wert zu aktualisieren. Außerdem wird dieses CDN-Protokoll in das [Pandas DataFrame](https://pandas.pydata.org/docs/reference/frame.html).
- **Analyse durchführen**: Der erste Codeblock lautet . _Analyseergebnis für Gesamt-, HTML-, JS/CSS- und Bildanforderungen anzeigen_; es bietet Cache-Trefferverhältnis-Prozentsatz, Balken- und Tortendiagramme.
Der zweite Codeblock ist _Die 5 wichtigsten MISS- und PASS-Anforderungs-URLs für HTML, JS/CSS und Bild_; es zeigt URLs und deren Zählungen im Tabellenformat an.

#### Ausführen des Jupyter-Notebooks

Führen Sie anschließend das Jupyter-Notebook in Adobe Experience Platform aus, indem Sie die folgenden Schritte ausführen:

1. Melden Sie sich beim [Adobe Experience Cloud](https://experience.adobe.com/)auf der Startseite > **Schnellzugriff** > klicken Sie auf **Experience Platform**

   ![Experience Platform](assets/cdn-logs-analysis/experience-platform.png){width="500" zoomable="yes"}

1. Klicken Sie auf der Adobe Experience Platform-Startseite > Abschnitt &quot;Datenwissenschaft&quot;> auf das **Notebooks** Menüelement. Um die Jupyter Notebooks-Umgebung zu starten, klicken Sie auf die Schaltfläche **JupyterLab** Registerkarte.

   ![Update der Protokolldatei](assets/cdn-logs-analysis/datascience-notebook.png){width="500" zoomable="yes"}

1. Verwenden Sie im Menü &quot;JupyterLab&quot;die **Hochladen von Dateien** Symbol, laden Sie die heruntergeladene CDN-Protokolldatei hoch und `aemcs_cdn_logs_analysis.ipynb` -Datei.

   ![Dateien hochladen - JupyteLab](assets/cdn-logs-analysis/jupyterlab-upload-file.png){width="500" zoomable="yes"}

1. Öffnen Sie die `aemcs_cdn_logs_analysis.ipynb` Datei durch Doppelklick.

1. Im **CDN-Protokolldatei laden** -Abschnitt des Notebooks aktualisieren Sie die `log_file` -Wert.

   ![Update der Protokolldatei](assets/cdn-logs-analysis/notebook-update-variable.png){width="500" zoomable="yes"}

1. Um die ausgewählte Zelle auszuführen und den Vorgang fortzusetzen, klicken Sie auf das **Play** Symbol.

   ![Update der Protokolldatei](assets/cdn-logs-analysis/notebook-run-cell.png){width="500" zoomable="yes"}

1. Nach dem Ausführen der **Analyseergebnis für Gesamt-, HTML-, JS/CSS- und Bildanforderungen anzeigen** -Codezelle angezeigt, zeigt die Ausgabe die Cache-Trefferquote in Prozent, Balken und Kreisdiagrammen an.

   ![Update der Protokolldatei](assets/cdn-logs-analysis/output-cache-hit-ratio.png){width="500" zoomable="yes"}

1. Nach dem Ausführen der **Die 5 wichtigsten MISS- und PASS-Anforderungs-URLs für HTML, JS/CSS und Bild** -Codezelle angezeigt, zeigt die Ausgabe die 5 wichtigsten MISS- und PASS-Anforderungs-URLs an.

   ![Update der Protokolldatei](assets/cdn-logs-analysis/output-top-urls.png){width="500" zoomable="yes"}

Sie können das Jupyter Notebook erweitern, um die CDN-Protokolle basierend auf Ihren Anforderungen zu analysieren.

## CDN-Cache-Konfiguration optimieren

Nach der Analyse der CDN-Protokolle können Sie die CDN-Cache-Konfiguration optimieren, um die Site-Leistung zu verbessern. Die AEM Best Practice ist, ein Cache-Trefferverhältnis von 90 % oder höher zu haben.

Weitere Informationen finden Sie unter [CDN-Cache-Konfiguration optimieren](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#caching).

Das AEM WKND-Projekt verfügt über eine Referenz-CDN-Konfiguration. Weitere Informationen finden Sie unter [CDN-Konfiguration](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L137-L190) aus dem `wknd.vhost` -Datei.
