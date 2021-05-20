---
title: Anpassen von Anmerkungen in AEM Assets
description: AEM Assets-Format und -Stil bei der Ausgabe in PDF können von AEM Entwicklern konfiguriert werden.
feature: Zusammenarbeit
version: 6.3, 6.4, 6.5
topic: Zusammenarbeit
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '62'
ht-degree: 3%

---


# Anpassen von Anmerkungen in AEM Assets{#using-annotations-in-aem-assets}

AEM unterstützt die Anpassung der Ausgabe der Anmerkung in PDF.

## PDF-Anmerkung sling:OsgiConfig-Definition

Um PDF-Anmerkungen anzupassen, erstellen Sie einen **sling:OsgiConfig** -Knoten in Ihrem AEM Projekt unter

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
