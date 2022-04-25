---
title: Anzeigen mehrerer PDF-Dokumente
description: Durchlaufen mehrere PDF-Dokumente in einem adaptiven Formular.
version: 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
kt: 10292
source-git-commit: cb5b3eb77a57fa8a2918710b7dbcd1b0a58b74bd
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 6%

---

# Anzeigen mehrerer PDF-Dokumente in einem Karussell

Ein gängiges Anwendungsbeispiel besteht darin, mehrere PDF-Dokumente für den Formularbenutzer anzuzeigen, die vor dem Senden des Formulars überprüft werden sollen.

Um dieses Anwendungsbeispiel zu erstellen, haben wir die Funktion [Adobe PDF Embed API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

[Eine Live-Demo dieses Beispiels kann hier vorgestellt werden.](https://forms.enablementadobe.com/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)

Die folgenden Schritte wurden ausgeführt, um die Integration abzuschließen

## Erstellen einer benutzerdefinierten Komponente zur Anzeige mehrerer PDF-Dokumente

Eine benutzerdefinierte Komponente (PDF-Karussell) wurde erstellt, um durch PDF-Dokumente zu blättern.

## Client-Bibliothek

Eine Client-Bibliothek wurde erstellt, um die PDF mithilfe der Adobe PDF-Einbettungs-API anzuzeigen. Die anzuzeigenden PDF werden in den PDF-Karussellkomponenten angegeben.

## Adaptives Formular erstellen

Erstellen eines adaptiven Formulars basierend auf einigen Registerkarten (dieses Beispiel enthält 3 Registerkarten) Fügen Sie einige adaptive Formularkomponenten in den ersten beiden Registerkarten hinzu Fügen Sie die PDF-Karussellkomponente auf der dritten Registerkarte hinzu Konfigurieren Sie die PDF-Karussellkomponente, wie im Screenshot unten dargestellt
![pdf-carousel](assets/pdf-carousel-af-component.png)

**Einbetten des PDF-API-Schlüssels** - Dies ist der Schlüssel, den Sie zum Einbetten des PDF-Dokuments verwenden können. Dieser Schlüssel funktioniert nur mit localhost. Sie können [eigenen Schlüssel](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) und ordnen Sie sie einer anderen Domäne zu.

**PDF Documents angeben** - Hier können Sie die PDF-Dokumente angeben, die im Karussell angezeigt werden sollen.


## Bereitstellen des Beispiels auf Ihrem Server

Gehen Sie wie folgt vor, um dies auf Ihrem lokalen Server zu testen:

1. [Client-Bibliothek importieren](assets/pdf-carousel-client-lib.zip) in Ihre lokale AEM-Instanz [mit dem Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importieren der PDF-Karussellkomponente](assets/pdf-carousel-component.zip) in Ihre lokale AEM-Instanz [mit dem Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importieren des adaptiven Formulars ](assets/adaptive-form-pdf-carousel.zip) in Ihre lokale AEM-Instanz [mit dem Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importieren der anzuzeigenden Beispiel-PDFs](assets/pdf-carousel-sample-documents.zip) in Ihre lokale AEM-Instanz [über den Link zum Hochladen der Asset-Datei](http://localhost:4502/assets.html/content/dam)
1. [Vorschau des adaptiven Formulars](http://localhost:4502/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)
1. Registerkarte auf der Registerkarte Zu überprüfende Dokumente . In der Karussellkomponente sollten drei PDF-Dokumente angezeigt werden.
