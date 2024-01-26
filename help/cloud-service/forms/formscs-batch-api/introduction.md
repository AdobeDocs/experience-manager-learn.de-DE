---
title: Dokumenterstellung mithilfe der Batch-API in AEM Forms CS
description: Konfigurieren Sie Batch-Vorgänge zum Generieren von Dokumenten und lösen Sie diese aus.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
exl-id: 165e2884-4399-4970-81ff-1f2f8b041a10
duration: 32
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 100%

---

# Einführung

Bei einer Batch-Anfrage werden gleichzeitig Dutzende, Hunderte oder Tausende ähnlicher Dokumente generiert, z. B. wenn ein Finanzunternehmen Kreditkartenabrechnungen erstellt und an alle Kundinnen und Kunden sendet.
Batch-APIs (asynchrone APIs) eignen sich für Anwendungsfälle für die geplante Erstellung mehrerer Dokumente mit hohem Durchsatz. Diese APIs generieren Dokumente in Stapeln. Beispielsweise werden damit monatliche Telefonrechnungen, Kreditkartenauszüge und Leistungsmitteilungen generiert.

Zur Verwendung der AEM Forms CS-Batch-API sind die folgenden Konfigurationen erforderlich:

1. Konfigurieren eines Azure Storage-Kontos
1. Erstellen einer Azure Storage-basierten Cloud-Konfiguration
1. Erstellen der Batch-Datenspeicherkonfiguration
1. Ausführen der Batch-API

Sie sollten sich mit der [API-Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/batch-api.yaml?lang=de) vertraut machen, bevor Sie mit diesem Tutorial fortfahren.
