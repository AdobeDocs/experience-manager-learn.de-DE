---
title: Normalisieren von Anfragen
description: Erfahren Sie, wie Sie Anfragen durch Transformation mit Traffic-Filterregeln in AEM as a Cloud Service normalisieren.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18313
thumbnail: null
exl-id: eee81cd6-9090-45d6-b77f-a266de1d9826
source-git-commit: 71454ea9f1302d8d1c08c99e937afefeda2b1322
workflow-type: ht
source-wordcount: '259'
ht-degree: 100%

---

# Normalisieren von Anfragen

Erfahren Sie, wie Sie Anfragen durch Transformation mit Traffic-Filterregeln in AEM as a Cloud Service normalisieren.

## Gründe und Zeitpunkt für die Transformation von Anfragen

Die Transformation von Anfragen ist hilfreich, wenn Sie eingehenden Traffic normalisieren und unnötige Varianzen reduzieren möchten, die durch nicht benötigte Abfrageparameter oder Header verursacht werden. Diese Technik wird häufig für folgende Zwecke verwendet:

- Verbessern der Caching-Effizienz durch Entfernen von Cache-Busting-Parametern, die für die AEM-Anwendung irrelevant sind.
- Schützen des Ursprungs vor Missbrauch durch Minimierung von Anfrage-Permutationen und Begrenzung unnötiger Verarbeitungsschritte.
- Bereinigen oder Vereinfachen von Anfragen, bevor sie an AEM weitergeleitet werden.

Diese Transformationen werden normalerweise auf CDN-Ebene angewendet, insbesondere für AEM-Veröffentlichungsebenen, die öffentlichen Traffic bereitstellen.

## Voraussetzungen

Bevor Sie fortfahren, stellen Sie sicher, dass Sie die erforderliche Einrichtung wie im Tutorial [Einrichten von Traffic-Filter- und WAF-Regeln](../setup.md) beschrieben abgeschlossen haben. Außerdem müssen Sie das [AEM WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd) geklont und in Ihrer AEM-Umgebung bereitgestellt haben.

## Beispiel: Aufheben von Abfrageparametern, die von der Anwendung nicht benötigt werden

In diesem Beispiel konfigurieren Sie eine Regel, die **alle Abfrageparameter außer** und `search` entfernt`campaignId`, um die Cache-Fragmentierung zu reduzieren.

- Fügen Sie die folgende Regel zur Datei `/config/cdn.yaml` des WKND-Projekts hinzu.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  requestTransformations:
    rules:
    # Unset all query parameters except those needed for search and campaignId
    - name: unset-all-query-params-except-those-needed
      when:
        reqProperty: tier
        in: ["publish"]
      actions:
        - type: unset
          queryParamMatch: ^(?!search$|campaignId$).*$
```

- Übernehmen Sie die Änderungen und pushen Sie sie in das Cloud Manager-Git-Repository.

- Implementieren Sie die Änderungen mit der [zuvor erstellten](../setup.md#deploy-rules-using-adobe-cloud-manager) Cloud Manager-Konfigurations-Pipeline in der AEM-Entwicklungsumgebung.

- Testen Sie die Regel, indem Sie auf die Seite der WKND-Site zugreifen, z. B. `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar&otherParam=baz`.

- In den AEM-Protokollen (`aemrequest.log`) sollten Sie sehen, dass die Anfrage in `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar` transformiert und `otherParam` entfernt wurde.

  ![WKND-Anfragetransformation](../assets/how-to/aemrequest-log-transformation.png)
