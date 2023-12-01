---
title: Einrichten von Konten und Diensten zur Asset Compute-Erweiterbarkeit
description: Die Entwicklung von Asset Compute-Sekundären erfordert Zugriff auf Konten und Dienste, einschließlich AEM as a Cloud Service, App-Entwicklung und Cloud-Speicherplatz, die von Microsoft oder Amazon bereitgestellt werden.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
jira: KT-6264
thumbnail: 40377.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 707657ad-221e-4dab-ac2a-46a4fcbc55bc
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '621'
ht-degree: 100%

---

# Einrichten von Konten und Diensten

Für dieses Tutorial müssen die folgenden Dienste bereitgestellt und über die Adobe ID der Person zugänglich sein.

Alle Adobe-Dienste müssen über dieselbe Adobe-Organisation mit Ihrer Adobe ID verfügbar sein.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [App Builder](#app-builder)
   + Die Bereitstellung kann zwischen 2 und 10 Tagen dauern
+ Cloud-Speicherplatz
   + [Azure Blob Storage](https://azure.microsoft.com/de-de/services/storage/blobs/)
   + oder [Amazon S3](https://aws.amazon.com/de/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>Stellen Sie sicher, dass Sie Zugriff auf alle oben genannten Dienste haben, bevor Sie mit diesem Tutorial fortfahren.
> 
> In den folgenden Abschnitten erfahren Sie, wie Sie die erforderlichen Dienste einrichten und bereitstellen.

## AEM as a Cloud Service{#aem-as-a-cloud-service}

Der Zugriff auf eine AEM as a Cloud Service-Umgebung ist erforderlich, um AEM Assets-Verarbeitungsprofile so zu konfigurieren, dass der benutzerdefinierte Asset Compute-Sekundär aufgerufen wird.

Idealerweise ist ein Sandbox-Programm oder eine Nicht-Sandbox-Entwicklungsumgebung verfügbar und kann genutzt werden.

Beachten Sie, dass ein lokales AEM SDK nicht ausreicht, um dieses Tutorial zu absolvieren, da ein lokales AEM SDK nicht mit Asset Compute-Microservices kommunizieren kann. Stattdessen ist eine echte AEM as a Cloud Service-Umgebung erforderlich.

## App-Entwicklung{#app-builder}

Das [App-Entwicklungs](https://developer.adobe.com/app-builder/)-Framework wird zum Erstellen und Bereitstellen von benutzerdefinierten Aktionen für Adobe I/O Runtime verwendet, die Server-lose Plattform von Adobe. AEM Asset Compute-Projekte sind speziell erstellte App-Entwicklungs-Projekte, die über Verarbeitungsprofile in AEM Assets integriert werden und die Möglichkeit bieten, auf Asset-Binärdateien zuzugreifen und diese zu verarbeiten.

Um Zugriff auf die App-Entwicklung zu erhalten, melden Sie sich für die Vorschau an.

1. [Für App-Entwicklungs-Testversion anmelden](https://developer.adobe.com/app-builder/trial/).
1. Nach ca. 2 bis 10 Tagen werden Sie per E-Mail über die Bereitstellung informiert, sodass Sie mit dem Tutorial fortfahren können.
   + Wenn Sie sich nicht sicher sind, ob die Bereitstellung erfolgt ist, fahren Sie mit den nächsten Schritten fort. Wenn Sie kein __App-Entwicklungs__-Projekt in [Adobe Developer Console](https://developer.adobe.com/console/) erstellen können, ist die Bereitstellung noch nicht erfolgt.

## Cloud-Speicherplatz

Cloud-Speicherplatz ist für die lokale Entwicklung von Asset Compute-Projekten erforderlich.

Wenn Asset Compute-Sekundäre für die direkte Verwendung durch AEM as a Cloud Service in Adobe I/O Runtime bereitgestellt werden, ist dieser Cloud-Speicherplatz nicht unbedingt erforderlich, da AEM den Cloud-Speicherplatz bereitstellt, aus dem das Asset gelesen und in die Ausgabedarstellung geschrieben wird.

### Microsoft Azure Blob Storage{#azure-blob-storage}

Wenn Sie noch keinen Zugriff auf Microsoft Azure Blob Storage haben, melden Sie sich für ein [kostenloses 12-Monats-Konto](https://azure.microsoft.com/de-de/free/) an.

In diesem Tutorial wird Azure Blob Storage verwendet, es kann jedoch auch [Amazon S3](#amazon-s3) (nur minimal abweichend vom Tutorial) genutzt werden.

>[!VIDEO](https://video.tv.adobe.com/v/40377?quality=12&learn=on)

_Clickthrough der Bereitstellung von Azure Blob Storage (kein Audio)_

1. Melden Sie sich bei Ihrem [Microsoft Azure-Konto](https://azure.microsoft.com/de-de/account/) an.
1. Navigieren Sie zum Abschnitt __Storage-Konten__ (Azure-Dienste)
1. Tippen Sie auf __+ Hinzufügen__, um ein neues Blob Storage-Konto zu erstellen
1. Erstellen Sie bei Bedarf eine neue __Ressourcengruppe__, z. B.: `aem-as-a-cloud-service`
1. Legen Sie einen __Storage-Kontonamen__ fest, zum Beispiel: `aemguideswkndassetcomput`
   + Der __Storage-Kontoname__, der zum [Konfigurieren des Cloud-Speichers](../develop/environment-variables.md) im lokalen Asset Compute-Entwicklungs-Tool verwendet wird
   + Die __Zugriffsschlüssel__, die mit dem Storage-Konto verknüpft sind, sind auch zum [Konfigurieren des Cloud-Speichers](../develop/environment-variables.md) erforderlich.
1. Belassen Sie alles andere wie voreingestellt, und tippen Sie auf die Schaltfläche __Überprüfen und Erstellen__.
   + Wählen Sie optional einen nahegelegenen __Speicherort__.
1. Überprüfen Sie die Bereitstellungsanfrage auf Richtigkeit und tippen Sie dann auf die Schaltfläche __Erstellen__.

### Amazon S3{#amazon-s3}

Für dieses Tutorial wird [Microsoft Azure Blob Storage](#azure-blob-storage) empfohlen. [Amazon S3](https://aws.amazon.com/de/s3/?did=ft_card&amp;trk=ft_card) kann jedoch auch verwendet werden.

Wenn Sie Amazon S3-Speicher verwenden, geben Sie beim [Konfigurieren der Umgebungsvariablen des Projekts](../develop/environment-variables.md#amazon-s3) die Amazon S3-Cloud-Speicher-Anmeldeinformationen an.

Wenn Sie speziell für dieses Tutorial Cloud-Speicher bereitstellen müssen, empfehlen wir die Verwendung von [Azure Blob Storage](#azure-blob-storage).
