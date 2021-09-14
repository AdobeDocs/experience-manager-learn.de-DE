---
title: Einrichten von Konten und Diensten für die Asset compute-Erweiterbarkeit
description: Die Entwicklung von Asset compute-Workern erfordert Zugriff auf Konten und Dienste, einschließlich AEM as a Cloud Service, Adobe Project Firefly und Cloud-Speicher, die von Microsoft oder Amazon bereitgestellt werden.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 707657ad-221e-4dab-ac2a-46a4fcbc55bc
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 3%

---

# Einrichten von Konten und Diensten

Für dieses Tutorial müssen die folgenden Dienste bereitgestellt und über die Adobe ID des Lernenden zugänglich sein.

Alle Adobe-Dienste müssen über dieselbe Adobe Org mit Ihrer Adobe ID verfügbar sein.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [Adobe Project FireFly](#adobe-project-firefly)
   + Die Bereitstellung kann zwischen 2 und 10 Tagen dauern
+ Cloud-Speicher
   + [Azure Blob Storage](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + oder [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>Stellen Sie sicher, dass Sie Zugriff auf alle oben genannten Dienste haben, bevor Sie mit diesem Tutorial fortfahren.
> 
> In den folgenden Abschnitten erfahren Sie, wie Sie die erforderlichen Dienste einrichten und bereitstellen.

## AEM als Cloud Service{#aem-as-a-cloud-service}

Der Zugriff auf eine AEM as a Cloud Service-Umgebung ist erforderlich, um AEM Assets-Verarbeitungsprofile so zu konfigurieren, dass der benutzerdefinierte Asset compute Worker aufgerufen wird.

Idealerweise ist ein Sandbox-Programm oder eine Nicht-Sandbox-Entwicklungsumgebung verfügbar.

Beachten Sie, dass ein lokales AEM-SDK nicht ausreicht, um dieses Tutorial abzuschließen, da das lokale AEM-SDK nicht mit Asset compute-Microservices kommunizieren kann. Stattdessen ist eine echte AEM als Cloud Service-Umgebung erforderlich.

## Adobe Project Firefly{#adobe-project-firefly}

Das Framework [Adobe Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html) wird zum Erstellen und Bereitstellen von benutzerdefinierten Aktionen für Adobe I/O Runtime verwendet, die Server-lose Plattform der Adobe. AEM Asset compute-Projekte sind speziell erstellte Firefly-Projekte, die über Verarbeitungsprofile in AEM Assets integriert werden und die Möglichkeit bieten, auf Asset-Binärdateien zuzugreifen und diese zu verarbeiten.

Um Zugriff auf Project Firefly zu erhalten, melden Sie sich für die Vorschau an.

1. [Registrieren Sie sich für die Project Firefly-Vorschau](https://adobeio.typeform.com/to/obqgRm).
1. Warten Sie ungefähr 2 bis 10 Tage, bis Sie per E-Mail über die Bereitstellung informiert werden, bevor Sie mit dem Tutorial fortfahren.
   + Wenn Sie sich nicht sicher sind, ob Sie bereitgestellt wurden, fahren Sie mit den nächsten Schritten fort und wenn Sie kein __Project Firefly__-Projekt in [Adobe Developer Console](https://console.adobe.io) erstellen können, wurden Sie noch nicht bereitgestellt.

## Cloud-Speicher

Cloud-Speicher ist für die lokale Entwicklung von Asset compute-Projekten erforderlich.

Wenn Asset compute-Sekundäre für die direkte Verwendung durch AEM als Cloud Service in der Adobe I/O Runtime bereitgestellt werden, ist dieser Cloud-Speicher nicht unbedingt erforderlich, da AEM den Cloud-Speicher bereitstellt, aus dem das Asset gelesen und in das Ausgabeformat geschrieben wird.

### Microsoft Azure Blob Storage{#azure-blob-storage}

Wenn Sie noch keinen Zugriff auf Microsoft Azure Blob Storage haben, melden Sie sich für ein [kostenloses 12-Monats-Konto](https://azure.microsoft.com/en-us/free/) an.

In diesem Tutorial wird Azure Blob Storage verwendet. [Amazon S3](#amazon-s3) kann jedoch auch nur eine geringfügige Variante des Tutorials verwendet werden.

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_Clickthrough der Bereitstellung des Azure Blob Storage (kein Audio)_


1. Melden Sie sich bei Ihrem [Microsoft Azure-Konto](https://azure.microsoft.com/en-us/account/) an.
1. Navigieren Sie zum Abschnitt __Speicherkonten__ Azure-Dienste .
1. Tippen Sie auf __+ Hinzufügen__, um ein neues Blob Storage-Konto zu erstellen.
1. Erstellen Sie nach Bedarf eine neue __Ressourcengruppe__, z. B.: `aem-as-a-cloud-service`
1. Geben Sie einen __Speicherkontonamen__ an, z. B.: `aemguideswkndassetcomput`
   + Der __Speicherkontoname__ wird für [Konfigurieren des Cloud-Speichers](../develop/environment-variables.md) für das lokale Asset compute Development Tool verwendet
   + Die __Zugriffsschlüssel__, die mit dem Speicherkonto verknüpft sind, sind auch bei der Konfiguration von [Cloud-Speicher](../develop/environment-variables.md) erforderlich.
1. Belassen Sie alles andere als Standard und tippen Sie auf die Schaltfläche __Überprüfen + Erstellen__
   + Wählen Sie optional den __Speicherort__ in Ihrer Nähe aus.
1. Überprüfen Sie die Bereitstellungsanforderung auf Richtigkeit und tippen Sie auf die Schaltfläche __Erstellen__ , falls zufrieden.

### Amazon S3{#amazon-s3}

Die Verwendung von [Microsoft Azure Blob Storage](#azure-blob-storage) wird zum Abschließen dieses Tutorials empfohlen. Es kann jedoch auch [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) verwendet werden.

Wenn Sie Amazon S3-Speicher verwenden, geben Sie die Anmeldeinformationen für den Amazon S3-Cloud-Speicher an, wenn Sie [die Umgebungsvariablen des Projekts](../develop/environment-variables.md#amazon-s3) konfigurieren.

Wenn Sie speziell für dieses Tutorial Cloud-Speicher bereitstellen müssen, empfehlen wir die Verwendung von [Azure Blob Storage](#azure-blob-storage).
