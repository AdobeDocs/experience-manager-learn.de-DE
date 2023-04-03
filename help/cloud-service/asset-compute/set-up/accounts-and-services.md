---
title: Einrichten von Konten und Diensten für die Asset compute-Erweiterbarkeit
description: Die Entwicklung von Asset compute-Workern erfordert Zugriff auf Konten und Dienste, einschließlich AEM as a Cloud Service, App Builder und Cloud-Speicher, die von Microsoft oder Amazon bereitgestellt werden.
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
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '621'
ht-degree: 3%

---

# Einrichten von Konten und Diensten

Für dieses Tutorial müssen die folgenden Dienste bereitgestellt und über die Adobe ID des Lernenden zugänglich sein.

Alle Adobe-Dienste müssen über dieselbe Adobe Org mit Ihrer Adobe ID verfügbar sein.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [App Builder](#app-builder)
   + Die Bereitstellung kann zwischen 2 und 10 Tagen dauern
+ Cloud-Speicher
   + [Azure Blob Storage](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + oder [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>Stellen Sie sicher, dass Sie Zugriff auf alle oben genannten Dienste haben, bevor Sie mit diesem Tutorial fortfahren.
> 
> In den folgenden Abschnitten erfahren Sie, wie Sie die erforderlichen Dienste einrichten und bereitstellen.

## AEM as a Cloud Service{#aem-as-a-cloud-service}

Der Zugriff auf eine AEM as a Cloud Service Umgebung ist erforderlich, um AEM Assets-Verarbeitungsprofile so zu konfigurieren, dass der benutzerdefinierte Asset compute Worker aufgerufen wird.

Idealerweise ist ein Sandbox-Programm oder eine Nicht-Sandbox-Entwicklungsumgebung verfügbar.

Beachten Sie, dass ein lokales AEM-SDK nicht ausreicht, um dieses Tutorial abzuschließen, da das lokale AEM-SDK nicht mit Asset compute-Microservices kommunizieren kann. Stattdessen ist eine echte AEM as a Cloud Service Umgebung erforderlich.

## App Builder{#app-builder}

Die [App Builder](https://developer.adobe.com/app-builder/) Framework wird zum Erstellen und Bereitstellen von benutzerdefinierten Aktionen für Adobe I/O Runtime verwendet, die Server-lose Plattform der Adobe. AEM Asset compute-Projekte sind speziell erstellte App Builder-Projekte, die über Verarbeitungsprofile in AEM Assets integriert werden und die Möglichkeit bieten, auf Asset-Binärdateien zuzugreifen und diese zu verarbeiten.

Um Zugriff auf App Builder zu erhalten, melden Sie sich für die Vorschau an.

1. [Für App Builder-Testversion anmelden](https://developer.adobe.com/app-builder/trial/).
1. Warten Sie ungefähr 2 bis 10 Tage, bis Sie per E-Mail über die Bereitstellung informiert werden, bevor Sie mit dem Tutorial fortfahren.
   + Wenn Sie sich nicht sicher sind, ob Sie bereitgestellt wurden, fahren Sie mit den nächsten Schritten fort und können keine __App Builder__ Projekt in [Adobe Developer-Konsole](https://developer.adobe.com/console/) Sie wurden noch nicht bereitgestellt.

## Cloud-Speicher

Cloud-Speicher ist für die lokale Entwicklung von Asset compute-Projekten erforderlich.

Wenn Asset compute-Sekundäre für die direkte Verwendung durch AEM as a Cloud Service in der Adobe I/O Runtime bereitgestellt werden, ist dieser Cloud-Speicher nicht unbedingt erforderlich, da AEM den Cloud-Speicher bereitstellt, aus dem das Asset gelesen und in die Ausgabedarstellung geschrieben wird.

### Microsoft Azure Blob Storage{#azure-blob-storage}

Wenn Sie noch keinen Zugriff auf Microsoft Azure Blob Storage haben, melden Sie sich für eine [kostenloses 12-Monats-Konto](https://azure.microsoft.com/en-us/free/).

In diesem Tutorial wird Azure Blob Storage verwendet, jedoch [Amazon S3](#amazon-s3) kann auch nur eine geringfügige Änderung des Tutorials verwendet werden.

>[!VIDEO](https://video.tv.adobe.com/v/40377?quality=12&learn=on)

_Clickthrough der Bereitstellung des Azure Blob Storage (kein Audio)_

1. Melden Sie sich bei Ihrer [Microsoft Azure-Konto](https://azure.microsoft.com/en-us/account/).
1. Navigieren Sie zum __Speicherkonten__ Azure-Dienstabschnitt
1. Tippen __+ Hinzufügen__ , um ein neues Blob Storage-Konto zu erstellen
1. Erstellen Sie eine neue __Ressourcengruppe__ nach Bedarf, z. B.: `aem-as-a-cloud-service`
1. Bereitstellung einer __Name des Speicherkontos__, zum Beispiel: `aemguideswkndassetcomput`
   + Die __Name des Speicherkontos__  verwendet für [Konfigurieren des Cloud-Speichers](../develop/environment-variables.md) im lokalen Asset compute Development Tool
   + Die __Zugriffsschlüssel__ mit dem Speicherkonto verknüpft sind, ist auch erforderlich, wenn [Konfigurieren des Cloud-Speichers](../develop/environment-variables.md).
1. Belassen Sie alles andere als Standard und tippen Sie auf __Überprüfen und erstellen__ button
   + Wählen Sie optional die __location__ in Ihrer Nähe.
1. Überprüfen Sie die Bereitstellungsanforderung auf Richtigkeit und tippen Sie auf __Erstellen__ Schaltfläche, wenn zufrieden gestellt

### Amazon S3{#amazon-s3}

Verwenden [Microsoft Azure Blob Storage](#azure-blob-storage) wird zum Abschluss dieses Tutorials empfohlen, jedoch [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) kann auch verwendet werden.

Wenn Sie Amazon S3-Speicher verwenden, geben Sie die Amazon S3-Cloud-Speicheranmeldeinformationen an, wenn [Konfigurieren der Umgebungsvariablen des Projekts](../develop/environment-variables.md#amazon-s3).

Wenn Sie speziell für dieses Tutorial Cloud-Speicher bereitstellen müssen, empfehlen wir die Verwendung von [Azure Blob Storage](#azure-blob-storage).
