---
title: Speichern und Fortsetzen von Briefen
seo-title: Save and resume letters
description: Erfahren Sie, wie Sie Briefentwürfe speichern und abrufen
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
jira: KT-10208
exl-id: e032070b-7332-4c2f-97ee-7e887a61aa7a
last-substantial-update: 2022-01-07T00:00:00Z
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 100%

---

# Einführung

Interaktive Kommunikation ermöglicht es den Agentinnen und Agenten, Ad-hoc-Korrespondenzen vorzubereiten, teilweise abgeschlossene Korrespondenzen zu speichern und diese abzurufen, um die Arbeit fortzusetzen. AEM Forms stellt Ihnen dafür die [Dienstleister-Schnittstelle](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/ccm/ccr/ccrDocumentInstance/api/services/CCRDocumentInstanceService.html) bereit. Von der Kundschaft wird erwartet, dass sie diese Schnittstelle implementiert, um die Funktion zum Speichern und Fortsetzen zu erhalten.

In diesem Artikel werden die Metadaten der Briefinstanz mithilfe der MySQL-Datenbank gespeichert. Die Briefdaten werden im Dateisystem gespeichert.

Das folgende Video zeigt den Anwendungsfall:

>[!VIDEO](https://video.tv.adobe.com/v/342129?quality=12&learn=on)

## Voraussetzungen

Sie benötigen Folgendes, um die Lösung entsprechend Ihren Anforderungen zu implementieren

* Arbeitserfahrung mit AEM Forms
* AEM Server 6.5 mit Forms-Add-on
* Vertraut sein mit der Erstellung von OSGI-Bundles
