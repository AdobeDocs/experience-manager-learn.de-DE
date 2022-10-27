---
title: Speichern und Fortsetzen von Briefen
seo-title: Save and resume letters
description: Erfahren Sie, wie Sie Entwurfsbriefe speichern und abrufen
seo-description: Learn how to save and retrieve draft letters
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
kt: 10208
exl-id: e032070b-7332-4c2f-97ee-7e887a61aa7a
last-substantial-update: 2022-01-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---

# Einführung   

Interaktive Kommunikation ermöglicht es den Agenten, Ad-hoc-Korrespondenzen vorzubereiten, teilweise abgeschlossene Korrespondenzen zu speichern und diese abzurufen, um die Arbeit fortzusetzen. AEM Forms stellt Ihnen Folgendes bereit: [Service Provider-Schnittstelle](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/ccm/ccr/ccrDocumentInstance/api/services/CCRDocumentInstanceService.html). Von dem Kunden wird erwartet, dass er diese Schnittstelle implementiert, um die Funktion zum Speichern und Fortsetzen zu erhalten.

In diesem Artikel werden die Metadaten der Briefinstanz mithilfe der MySQL-Datenbank gespeichert. Die Briefdaten werden im Dateisystem gespeichert.

Das folgende Video zeigt den Anwendungsfall:

>[!VIDEO](https://video.tv.adobe.com/v/342129/quality=9)

## Voraussetzungen

Sie benötigen Folgendes, um die Lösung entsprechend Ihren Anforderungen zu implementieren

* Arbeitserfahrung mit AEM Forms
* AEM Server 6.5 mit Forms Add on
* Sollte beim Erstellen von OSGI-Bundles vertraut sein
