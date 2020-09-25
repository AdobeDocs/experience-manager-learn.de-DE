---
title: Anpassen von Anmerkungen in AEM Assets
seo-title: Anpassen von Anmerkungen in AEM Assets
description: Format und Stil von AEM Assets bei der Ausgabe in PDF können von AEM Entwicklern konfiguriert werden.
feature: asset-share
topics: authoring, collaboration, operations, sharing
audience: developer
doc-type: feature video
activity: developer
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '65'
ht-degree: 0%

---


# Anpassen von Anmerkungen in AEM Assets{#using-annotations-in-aem-assets}

AEM unterstützt die Anpassung der Ausgabe der Anmerkung in PDF.

## PDF-Anmerkungssling:OSGiConfig-Definition

Um PDF-Anmerkungen anzupassen, erstellen Sie im AEM unter ****

`/apps/my-project/config.author/com.day.cq.dam.core.impl.annotation.pdf.AnnotationPdfConfig.xml` und passen Sie die Werte nach Bedarf an:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"

    cq.dam.config.annotation.pdf.document.padding.vertical="{Long}20"
    cq.dam.config.annotation.pdf.reviewStatus.color.changesRequested="fad269"
    cq.dam.config.annotation.pdf.document.height="{Long}792"
    cq.dam.config.annotation.pdf.document.width="{Long}612"
    cq.dam.config.annotation.pdf.reviewStatus.width="{Long}150"
    cq.dam.config.annotation.pdf.font.size="{Long}7"
    cq.dam.config.annotation.pdf.font.color="3B3B3B"
    cq.dam.config.annotation.pdf.font.light="A4A4A4"
    cq.dam.config.annotation.pdf.reviewStatus.color.rejected="fa7d73"
    cq.dam.config.annotation.pdf.minImageHeight="{Long}100"
    cq.dam.config.annotation.pdf.reviewStatus.color.approved="009933"
    cq.dam.config.annotation.pdf.marginTextImage="{Long}14"
    cq.dam.config.annotation.pdf.document.padding.horizontal="{Long}20"
    cq.dam.config.annotation.pdf.annotationMarker.width="{Long}5"
    />
```
