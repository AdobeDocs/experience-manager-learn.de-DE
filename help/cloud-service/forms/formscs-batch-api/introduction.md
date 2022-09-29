---
title: Dokumenterstellung mithilfe der Batch-API in AEM Forms CS
description: Konfigurieren und Trigger von Batch-Vorgängen zum Generieren von Dokumenten.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
exl-id: 165e2884-4399-4970-81ff-1f2f8b041a10
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 24%

---

# Einführung   

Bei einer Batch-Anfrage werden gleichzeitig Zehntausende ähnlicher Dokumente generiert. Beispiel: Ein Finanzunternehmen kann Kreditkartenabschlüsse generieren, die an alle Kunden gesendet werden.
Batch-APIs (asynchrone APIs) eignen sich für Anwendungsfälle für die geplante Erstellung mehrerer Dokumente mit hohem Durchsatz. Diese APIs generieren Dokumente in Stapeln. Beispielsweise werden damit monatliche Telefonrechnungen, Kreditkartenauszüge und Leistungsmitteilungen generiert.

Um die Batch-Vorgang-API von AEM Forms CS zu verwenden, sind die folgenden Konfigurationen erforderlich

1. Azure-Speicherkonto konfigurieren
1. Erstellen einer Azure Storage-basierten Cloud-Konfiguration
1. Erstellen der Batch-Datenspeicherkonfiguration
1. Ausführen der Batch-API

Es wird empfohlen, sich mit dem [API-Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/batch-api.yaml?lang=en) bevor Sie mit der Verwendung dieses Tutorials fortfahren.
