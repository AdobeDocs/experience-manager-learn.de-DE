---
title: Einrichten von Konten und Diensten für die Asset compute-Erweiterbarkeit
description: Entwickler von Asset compute-Mitarbeitern benötigen Zugriff auf Konten und Dienste, einschließlich AEM als Cloud Service, Adobe Project Firefly und Cloud-Datenspeicherung von Microsoft oder Amazon.
feature: asset compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
topic: Integrationen, Entwicklung
role: Entwickler
level: Vermittelt, erfahren
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '635'
ht-degree: 2%

---


# Einrichten von Konten und Diensten

Für dieses Lernprogramm müssen die folgenden Dienste über das Adobe ID des Lernenden bereitgestellt und zugänglich sein.

Alle Adobe-Services müssen über dieselbe Adobe Org, mit Ihrem Adobe ID zugänglich sein.

+ [ zu AEM as a Cloud Service](#aem-as-a-cloud-service)
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

Der Zugriff auf eine AEM als Cloud Service-Umgebung ist erforderlich, um AEM Assets-Verarbeitungswerkzeuge zum Aufrufen des benutzerdefinierten Asset compute-Workers zu konfigurieren.

Idealerweise steht ein Sandbox-Programm oder eine Nicht-Sandbox-Umgebung zur Verfügung.

Beachten Sie, dass ein lokales AEM SDK nicht ausreicht, um dieses Lernprogramm abzuschließen, da das lokale AEM SDK nicht mit Asset compute microservices kommunizieren kann, sondern stattdessen eine echte AEM als Cloud Service-Umgebung erforderlich ist.

## Adobe Project Firefly{#adobe-project-firefly}

Das [Adobe Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html)-Framework wird zum Erstellen und Bereitstellen von benutzerdefinierten Aktionen auf Adobe I/O Runtime, der serverllosen Plattform der Adobe, verwendet. AEM Asset compute-Projekte sind speziell entwickelte Firefly-Projekte, die über Verarbeitungsmodule in AEM Assets integriert werden und die Möglichkeit bieten, auf Asset-Binärdateien zuzugreifen und diese zu verarbeiten.

Um Zugriff auf das Projekt Firefly zu erhalten, melden Sie sich bei der Vorschau an.

1. [Melden Sie sich für die Projekt Firefly-Vorschau](https://adobeio.typeform.com/to/obqgRm) an.
1. Warten Sie etwa 2-10 Tage, bis Sie per E-Mail benachrichtigt werden, dass Sie freigeschaltet sind, bevor Sie mit dem Tutorial fortfahren.
   + Wenn Sie sich nicht sicher sind, ob Sie bereitgestellt wurden, fahren Sie mit den nächsten Schritten fort und wenn Sie in [Adobe Developer Console](https://console.adobe.io) kein __Projekt Firefly__-Projekt erstellen können, wurden Sie noch nicht bereitgestellt.

## Cloud-Datenspeicherung

Die Cloud-Datenspeicherung ist für die lokale Entwicklung von Asset compute-Projekten erforderlich.

Wenn Asset compute-Mitarbeiter für die direkte Verwendung durch AEM als Cloud Service auf dem Adobe I/O Runtime bereitgestellt werden, ist diese Cloud-Datenspeicherung nicht unbedingt erforderlich, da AEM die Cloud-Datenspeicherung bietet, von der aus das Asset gelesen und in die Darstellung geschrieben wird.

### Microsoft Azure Blob Storage{#azure-blob-storage}

Wenn Sie noch keinen Zugriff auf die Datenspeicherung von Microsoft Blue Blob haben, melden Sie sich für ein [kostenloses 12-Monats-Konto](https://azure.microsoft.com/en-us/free/) an.

Dieses Tutorial wird die Datenspeicherung von Azurblut verwenden, aber [Amazon S3](#amazon-s3) kann ebenso verwendet werden wie nur eine geringfügige Variante des Tutorials.

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_Clickthrough der Bereitstellung der Azurblase-Datenspeicherung (kein Ton)_


1. Melden Sie sich bei Ihrem [Microsoft Blue-Konto](https://azure.microsoft.com/en-us/account/) an.
1. Navigieren Sie zum Abschnitt __Datenspeicherung Accounts__ Azurblaue Dienste
1. Tippen Sie auf __+ Hinzufügen__, um ein neues Blob-Datenspeicherung-Konto zu erstellen.
1. Erstellen Sie nach Bedarf eine neue __Ressourcengruppe__, z. B.: `aem-as-a-cloud-service`
1. Geben Sie einen __Datenspeicherung-Kontonamen__ ein, z. B.: `aemguideswkndassetcomput`
   + Der __Datenspeicherung-Kontoname__ wird für [Konfigurieren der Cloud-Datenspeicherung](../develop/environment-variables.md) für das lokale Asset compute Development Tool verwendet
   + Die mit dem Benutzerkonto verknüpften __Zugriffstasten__ sind auch erforderlich, wenn [die Cloud-Datenspeicherung](../develop/environment-variables.md) konfiguriert wird.
1. Belassen Sie alle anderen Standardeinstellungen und tippen Sie auf die Schaltfläche __Überprüfen + Erstellen__
   + Wählen Sie optional den __Ort__ in Ihrer Nähe aus.
1. Überprüfen Sie die Bereitstellungsanforderung auf Richtigkeit und tippen Sie auf die Schaltfläche __Erstellen__, falls zufrieden gestellt.

### Amazon S3{#amazon-s3}

Die Verwendung von [Microsoft Azurblase-Datenspeicherung](#azure-blob-storage) wird zum Abschluss dieses Lernprogramms empfohlen. Es kann jedoch auch [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) verwendet werden.

Wenn Sie die Amazon S3-Datenspeicherung verwenden, geben Sie bei der Konfiguration der Projektvariablen [die Anmeldeinformationen für die Amazon S3-Cloud-Datenspeicherung an.](../develop/environment-variables.md#amazon-s3)

Wenn Sie speziell für dieses Lernprogramm Cloud-Datenspeicherung bereitstellen müssen, empfehlen wir die Verwendung der [Azurblase-Datenspeicherung](#azure-blob-storage).
