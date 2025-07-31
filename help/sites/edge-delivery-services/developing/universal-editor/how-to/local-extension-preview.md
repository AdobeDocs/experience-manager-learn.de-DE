---
title: Vorschau einer Erweiterung des universellen Editors
description: Erfahren Sie, wie Sie während der Entwicklung eine Vorschau einer lokal ausgeführten universellen Editor-Erweiterung anzeigen.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner, Intermediate, Experienced
doc-type: Tutorial
jira: KT-18658
source-git-commit: f0ad5d66549970337118220156d7a6b0fd30fd57
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 0%

---


# Vorschau einer lokalen Erweiterung des universellen Editors

>[!TIP]
> Erfahren Sie, wie [eine universelle Editor-Erweiterung erstellen](https://developer.adobe.com/uix/docs/services/aem-universal-editor/).

Um eine Vorschau einer Erweiterung des universellen Editors während der Entwicklung anzuzeigen, müssen Sie Folgendes tun:

1. Führen Sie die Erweiterung lokal aus.
2. Akzeptieren Sie das selbstsignierte Zertifikat.
3. Öffnen Sie eine Seite im universellen Editor.
4. Aktualisieren Sie die Standort-URL, um die lokale Erweiterung zu laden.

## Lokales Ausführen der Erweiterung

Dabei wird davon ausgegangen, dass Sie bereits eine [Erweiterung des universellen Editors](https://developer.adobe.com/uix/docs/services/aem-universal-editor/) erstellt haben und sie beim lokalen Testen und Entwickeln in der Vorschau anzeigen möchten.

Starten Sie die Erweiterung des universellen Editors mit:

```bash
$ aio app run
```

Es werden Ausgaben wie die folgenden angezeigt:

```
To view your local application:
  -> https://localhost:9080
To view your deployed application in the Experience Cloud shell:
  -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://localhost:9080
```

Dadurch wird Ihre Erweiterung standardmäßig unter `https://localhost:9080` ausgeführt.


## Akzeptieren des selbstsignierten Zertifikats

Der universelle Editor erfordert HTTPS, um Erweiterungen zu laden. Da die lokale Entwicklung ein selbstsigniertes Zertifikat verwendet, muss Ihr Browser diesem explizit vertrauen.

Öffnen Sie eine neue Browser-Registerkarte und navigieren Sie zur URL der lokalen Erweiterung, die über den `aio app run`-Befehl ausgegeben wird:

```
https://localhost:9080
```

Ihr Browser zeigt eine Zertifikatwarnung an. Akzeptieren Sie das Zertifikat, um fortzufahren.

![Akzeptieren des selbstsignierten Zertifikats](./assets/local-extension-preview/accept-certificate.png)

Nach der Annahme wird die Platzhalterseite der lokalen Erweiterung angezeigt:

![Erweiterung ist verfügbar](./assets/local-extension-preview/extension-accessible.png)


## Öffnen einer Seite im universellen Editor

Öffnen Sie den universellen Editor über die [universelle Editor-Konsole](https://experience.adobe.com/#/@myOrg/aem/editor/canvas/) oder indem Sie eine Seite in AEM Sites bearbeiten, die den universellen Editor verwendet:

![Öffnen Sie eine Seite im universellen Editor](./assets/local-extension-preview/open-page-in-ue.png)


## Erweiterung laden

Suchen Sie im universellen Editor das Feld **Speicherort** oben in der Mitte der Benutzeroberfläche. Erweitern Sie sie und aktualisieren Sie die **URL im Feld Speicherort**, **nicht die Browser-Adressleiste**.

Fügen Sie die folgenden Abfrageparameter an:

* `devMode=true` - Aktiviert den Entwicklungsmodus für den universellen Editor.
* `ext=https://localhost:9080` - Lädt die lokal ausgeführte Erweiterung.

Zum Beispiel:

```
https://author-pXXX-eXXX.adobeaemcloud.com/content/aem-ue-wknd/index.html?devMode=true&ext=https://localhost:9080
```

![Aktualisieren der Speicherort-URL des universellen Editors](./assets/local-extension-preview/update-location-url.png)


## Vorschau der Erweiterung

Führen Sie einen **harten Neustart** des Browsers durch, um sicherzustellen, dass die aktualisierte URL verwendet wird.

Der universelle Editor lädt jetzt Ihre lokale Erweiterung - nur in Ihrer Browser-Sitzung.

Alle Code-Änderungen, die Sie lokal vornehmen, werden sofort übernommen.

![Lokale Erweiterung geladen](./assets/local-extension-preview/extension-loaded.png)

