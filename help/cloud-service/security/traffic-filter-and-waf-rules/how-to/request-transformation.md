---
title: Normalisieren von Anfragen
description: Erfahren Sie, wie Sie Anfragen normalisieren können, indem Sie sie mithilfe von Traffic-Filterregeln in AEM as a Cloud Service transformieren.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18313
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '259'
ht-degree: 8%

---

# Normalisieren von Anfragen

Erfahren Sie, wie Sie Anfragen normalisieren können, indem Sie sie mithilfe von Traffic-Filterregeln in AEM as a Cloud Service transformieren.

## Warum und wann Anforderungen transformiert werden sollten

Anforderungstransformationen sind nützlich, wenn Sie eingehenden Traffic normalisieren und unnötige Varianzen reduzieren möchten, die durch nicht benötigte Abfrageparameter oder Kopfzeilen verursacht werden. Diese Technik wird häufig verwendet, um:

- Verbessern Sie die Caching-Effizienz, indem Sie Cache-Busting-Parameter entfernen, die für das AEM-Programm nicht relevant sind.
- Schützen Sie den Ursprung vor Missbrauch, indem Sie Permutationen von Anfragen minimieren und unnötige Verarbeitung minimieren.
- Bereinigen oder vereinfachen Sie Anfragen, bevor sie an AEM weitergeleitet werden.

Diese Umwandlungen werden normalerweise auf CDN-Ebene angewendet, insbesondere für AEM-Veröffentlichungsebenen, die öffentlichen Traffic bereitstellen.

## Voraussetzungen

Bevor Sie fortfahren, stellen Sie sicher, dass Sie die erforderliche Einrichtung abgeschlossen haben, wie im Tutorial [Einrichten von Traffic-Filtern und WAF-Regeln](../setup.md) beschrieben. Außerdem haben Sie das [AEM WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd) geklont und in Ihrer AEM-Umgebung bereitgestellt.

## Beispiel: Abfrageparameter aufheben, die von der Anwendung nicht benötigt werden

In diesem Beispiel konfigurieren Sie eine Regel, die **alle Abfrageparameter mit Ausnahme von entfernt** `search` und `campaignId`, um die Cache-Fragmentierung zu reduzieren.

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

- Übertragen und pushen Sie die Änderungen in das Cloud Manager-Git-Repository.

- Stellen Sie die Änderungen mithilfe der Cloud Manager-Konfigurations-Pipeline ([ erstellt) in der AEM-](../setup.md#deploy-rules-using-adobe-cloud-manager) bereit.

- Testen Sie die Regel, indem Sie auf die Seite der WKND-Site zugreifen, z. B. `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar&otherParam=baz`.

- In AEM-Protokollen (`aemrequest.log`) sollten Sie sehen, dass die Anfrage in `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar` umgewandelt und die `otherParam` entfernt wird.

  ![WKND-Anforderungstransformation](../assets/how-to/aemrequest-log-transformation.png)

