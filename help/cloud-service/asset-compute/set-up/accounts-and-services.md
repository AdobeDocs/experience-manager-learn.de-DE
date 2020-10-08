---
title: Einrichten von Konten und Diensten für die Erweiterbarkeit von Asset Computing
description: Mitarbeiter von Asset Compute benötigen Zugriff auf Konten und Dienste, einschließlich AEM als Cloud Service, Adobe Project Firefly und Cloud-Datenspeicherung von Microsoft oder Amazon.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 2%

---


# Einrichten von Konten und Diensten

Für dieses Lernprogramm müssen die folgenden Dienste über das Adobe ID des Lernenden bereitgestellt und zugänglich sein.

Alle Adobe-Services müssen über dieselbe Adobe Org, mit Ihrem Adobe ID zugänglich sein.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [Adobe Project FireFly](#adobe-project-firefly)
   + Die Bereitstellung kann zwischen 2 und 10 Tagen dauern
+ Cloud-Datenspeicherung
   + [Azurblauch-Datenspeicherung](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + oder [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>Stellen Sie sicher, dass Sie Zugriff auf alle oben genannten Dienste haben, bevor Sie dieses Lernprogramm fortsetzen.
> 
> Überprüfen Sie die folgenden Abschnitte, wie Sie die erforderlichen Dienste einrichten und bereitstellen.

## AEM as a Cloud Service{#aem-as-a-cloud-service}

Der Zugriff auf eine AEM als Cloud Service-Umgebung ist erforderlich, um AEM Assets-Verarbeitungswerkzeuge zum Aufrufen des benutzerdefinierten Asset Compute-Workers zu konfigurieren.

Idealerweise steht ein Sandbox-Programm oder eine Nicht-Sandbox-Umgebung zur Verfügung.

Beachten Sie, dass ein lokales AEM-SDK nicht ausreicht, um dieses Lernprogramm abzuschließen, da das lokale AEM SDK nicht mit Asset Compute-Mikrodiensten kommunizieren kann, sondern stattdessen eine echte AEM als Cloud Service-Umgebung erforderlich ist.

## Projekt Adobe{#adobe-project-firefly}

Das [Adobe Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html) Framework wird zum Erstellen und Bereitstellen von benutzerdefinierten Aktionen für Adobe I/O Runtime, die serverllose Plattform der Adobe, verwendet. AEM Asset Compute-Projekte sind eigens für Firefly entwickelte Projekte, die über Processing-Profil in AEM Assets integriert werden und die Möglichkeit bieten, auf Asset-Binärdateien zuzugreifen und diese zu verarbeiten.

Um Zugriff auf das Projekt Firefly zu erhalten, melden Sie sich bei der Vorschau an.

1. [Melden Sie sich für die Projekt Firefly-Vorschau](https://adobeio.typeform.com/to/obqgRm)an.
1. Warten Sie etwa 2-10 Tage, bis Sie per E-Mail benachrichtigt werden, dass Sie freigeschaltet sind, bevor Sie mit dem Tutorial fortfahren.
   + Wenn Sie sich nicht sicher sind, ob Sie bereitgestellt wurden, fahren Sie mit den nächsten Schritten fort und wenn Sie kein __Projekt mit Firefly__ in der [Adobe Developer Console](https://console.adobe.io) erstellen können, wurden Sie noch nicht bereitgestellt.

## Cloud-Datenspeicherung

Für die lokale Entwicklung von Asset Compute-Projekten ist eine Cloud-Datenspeicherung erforderlich.

Wenn Asset Compute-Mitarbeiter für die direkte Verwendung durch AEM als Cloud Service auf dem Adobe I/O Runtime bereitgestellt werden, ist diese Cloud-Datenspeicherung nicht unbedingt erforderlich, da AEM die Cloud-Datenspeicherung bietet, von der aus das Asset gelesen und in die Darstellung geschrieben wird.

### Microsoft Azure Blob Storage{#azure-blob-storage}

Wenn Sie noch keinen Zugriff auf Microsoft Azurblut Datenspeicherung haben, registrieren Sie sich für ein [kostenloses 12-Monats-Konto](https://azure.microsoft.com/en-us/free/).

Dieses Tutorial wird die Datenspeicherung von Azurblut verwenden, aber [Amazon S3](#amazon-s3) kann ebenso verwendet werden wie nur geringfügige Variation des Tutorials.

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_Clickthrough der Bereitstellung der Azurblase-Datenspeicherung (kein Ton)_


1. Melden Sie sich bei Ihrem [Microsoft Azurblase-Konto](https://azure.microsoft.com/en-us/account/)an.
1. Navigieren Sie zum Bereich __Datenspeicherung Accounts__ Azurblase-Dienste.
1. Tippen Sie auf __+ Hinzufügen__ , um ein neues Blob Datenspeicherung-Konto zu erstellen.
1. Erstellen Sie bei Bedarf eine neue __Ressourcengruppe__ , z. B.: `aem-as-a-cloud-service`
1. Geben Sie beispielsweise einen Kontonamen __für die__ Datenspeicherung ein: `aemguideswkndassetcomput`
   + Der Kontoname __der__ Datenspeicherung wird zum [Konfigurieren der Cloud-Datenspeicherung](../develop/environment-variables.md) für das lokale Asset Compute Development Tool verwendet
   + Die __Zugriffstasten__ , die mit dem Datenspeicherung-Konto verknüpft sind, sind auch beim [Konfigurieren der Cloud-Datenspeicherung](../develop/environment-variables.md)erforderlich.
1. Belassen Sie alle anderen Elemente als Standard und klicken Sie auf die Schaltfläche __Überprüfen und erstellen__
   + Wählen Sie optional die __Position__ in Ihrer Nähe aus.
1. Überprüfen Sie die Bereitstellungsanforderung auf Richtigkeit und tippen Sie auf die Schaltfläche __Erstellen__ , wenn Sie zufrieden sind.

### Amazon S3{#amazon-s3}

Die Verwendung der [Microsoft Azurblase Datenspeicherung](#azure-blob-storage) wird für den Abschluss dieses Tutorials empfohlen, aber [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) kann auch verwendet werden.

Wenn Sie die Amazon S3-Datenspeicherung verwenden, geben Sie beim [Konfigurieren der Projektvariablen](../develop/environment-variables.md#amazon-s3)die Anmeldeinformationen für die Amazon S3-Cloud-Datenspeicherung an.

Wenn Sie Cloud-Datenspeicherung speziell für diese Übung bereitstellen müssen, empfehlen wir die Verwendung der [Azurblase-Datenspeicherung](#azure-blob-storage).
