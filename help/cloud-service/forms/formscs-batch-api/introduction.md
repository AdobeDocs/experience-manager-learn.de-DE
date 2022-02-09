---
title: Dokumenterstellung mithilfe der Batch-API in AEM Forms CS
description: Konfigurieren und Trigger von Batch-Vorgängen zum Generieren von Dokumenten.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
source-git-commit: 228da29e7ac0d61359c2b94131495b5b433a09dc
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

Es wird empfohlen, sich mit dem [API-Dokumentation](https://experienceleague.corp.adobe.com/docs/experience-manager-cloud-service/assets/batch-api.yaml?lang=en) bevor Sie mit der Verwendung dieses Tutorials fortfahren.




