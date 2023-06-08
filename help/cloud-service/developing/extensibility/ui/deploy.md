---
title: Bereitstellen einer AEM UI-Erweiterung
description: Erfahren Sie, wie Sie eine AEM UI-Erweiterung bereitstellen.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 2e37165d-c003-4206-8133-54e37ca35b8e
source-git-commit: 6b5c755bd8fe6bbf497895453b95eb236f69d5f6
workflow-type: tm+mt
source-wordcount: '777'
ht-degree: 1%

---

# Bereitstellen einer Erweiterung

Für die Verwendung in AEM as a Cloud Service Umgebungen muss die App Builder-Erweiterung bereitgestellt und genehmigt werden.

Bei der Bereitstellung von App Builder-Apps für Erweiterungen sind einige Aspekte zu beachten:

+ Erweiterungen werden im Adobe Developer Console-Projektarbeitsbereich bereitgestellt. Die standardmäßigen Arbeitsbereiche sind:
   + __Produktion__ Workspace enthält Erweiterungsbereitstellungen, die in allen AEM as a Cloud Service verfügbar sind.
   + __Staging__ Workspace fungiert als Entwicklerarbeitsbereich. Erweiterungen, die im Staging-Arbeitsbereich bereitgestellt werden, sind in AEM as a Cloud Service nicht verfügbar.
Arbeitsbereiche der Adobe Developer-Konsole weisen keine direkte Korrelation mit AEM as a Cloud Service Umgebungstypen auf.
+ Eine im Produktionsarbeitsbereich bereitgestellte Erweiterung wird in allen AEM as a Cloud Service Umgebungen in der Adobe Org angezeigt, in denen die Erweiterung vorhanden ist.
Eine Erweiterung kann nicht auf die Umgebungen beschränkt sein, in denen sie registriert ist, indem sie [Bedingungslogik, die den AEM as a Cloud Service Hostnamen überprüft](https://developer.adobe.com/uix/docs/guides/publication/#enabling-extension-only-on-specific-aem-environments).
+ Auf AEM as a Cloud Service können mehrere Erweiterungen verwendet werden. Adobe empfiehlt, dass jede App Builder-Erweiterung verwendet wird, um ein einzelnes Geschäftsziel zu lösen. Eine App Builder-App mit einer Erweiterung kann jedoch mehrere Erweiterungspunkte implementieren, die ein gemeinsames Geschäftsziel unterstützen.

## Erste Bereitstellung

Damit eine Erweiterung in AEM as a Cloud Service Umgebungen verfügbar ist, muss sie in der Adobe Developer Console bereitgestellt werden.

Der Implementierungsprozess gliedert sich in zwei logische Schritte:

1. Bereitstellung der App Builder-Erweiterung-App für die Adobe Developer Console durch einen Entwickler.
1. Genehmigung der Erweiterung durch einen Implementierungsmanager oder Business Owner.

### Bereitstellen der Erweiterung

Stellen Sie die Erweiterung im Produktionsarbeitsbereich bereit. Erweiterungen, die im Produktionsarbeitsbereich bereitgestellt werden, werden automatisch allen AEM as a Cloud Service Autorendiensten in der Adobe Org hinzugefügt, in der die Erweiterung bereitgestellt wird.

1. Öffnen Sie eine Befehlszeile im Stammverzeichnis der aktualisierten App Builder-App der Erweiterung.
1. Stellen Sie sicher, dass der Produktionsarbeitsbereich aktiv ist.

   ```shell
   $ aio app use -w Production
   ```

   Zusammenführen aller Änderungen zu `.env` und `.aio`.

1. Stellen Sie die aktualisierte App Builder-App der Erweiterung bereit.

   ```shell
   $ aio app deploy
   ```

#### Genehmigung der Bereitstellung anfordern

![Erweiterung zur Genehmigung einreichen](./assets/deploy/submit-for-approval.png){align="center"}

1. Anmelden bei [Adobe Developer-Konsole](https://developer.adobe.com)
1. Auswählen __Konsole__
1. Navigieren Sie zu __Projekte__
1. Wählen Sie das Projekt aus, das mit der Erweiterung verknüpft ist
1. Wählen Sie die __Produktion__ Arbeitsbereich
1. Auswählen __Zur Genehmigung einreichen__
1. Füllen Sie das Formular aus und senden Sie es ab und aktualisieren Sie die Felder nach Bedarf.

### Freigabe

![Genehmigung der Erweiterung](./assets/deploy/adobe-exchange.png){align="center"}

1. Anmelden bei [Adobe Exchange](https://exchange.adobe.com/)
1. Navigieren Sie zu __Verwalten__ > __Apps steht noch aus Überprüfung__
1. __Überprüfen__ die App Builder-App der Erweiterung
1. Wenn die Erweiterungsänderungen akzeptabel sind __Accept__ die Überprüfung. Dadurch wird die Erweiterung sofort in alle AEM as a Cloud Service Autorendienste in der Adobe-Organisation eingefügt.

Sobald die Erweiterungsanforderung genehmigt wurde, wird die Erweiterung sofort in den AEM as a Cloud Service Autorendiensten aktiviert.

## Aktualisieren einer Erweiterung

Das Aktualisieren und Erweitern der App Builder-App folgt demselben Prozess wie das [Erstbereitstellung](#initial-deployment)mit der Abweichung, dass die vorhandene Erweiterungsbereitstellung zuerst widerrufen werden muss.

### Die Erweiterung sperren

Um eine neue Version einer Erweiterung bereitzustellen, muss sie zunächst gesperrt (oder entfernt) werden. Die Erweiterung ist zwar &quot;Revoziert&quot;, aber nicht in AEM Konsolen verfügbar.

1. Anmelden bei [Adobe Exchange](https://exchange.adobe.com/)
1. Navigieren Sie zu __Verwalten__ > __App Builder-Apps__
1. __Sperren__ Zu aktualisierende Erweiterung

### Bereitstellen der Erweiterung

Stellen Sie die Erweiterung im Produktionsarbeitsbereich bereit. Erweiterungen, die im Produktionsarbeitsbereich bereitgestellt werden, werden automatisch allen AEM as a Cloud Service Autorendiensten in der Adobe Org hinzugefügt, in der die Erweiterung bereitgestellt wird.

1. Öffnen Sie eine Befehlszeile im Stammverzeichnis der aktualisierten App Builder-App der Erweiterung.
1. Stellen Sie sicher, dass der Produktionsarbeitsbereich aktiv ist.

   ```shell
   $ aio app use -w Production
   ```

   Zusammenführen aller Änderungen zu `.env` und `.aio`.

1. Stellen Sie die aktualisierte App Builder-App der Erweiterung bereit.

   ```shell
   $ aio app deploy
   ```

#### Genehmigung der Bereitstellung anfordern

![Erweiterung zur Genehmigung einreichen](./assets/deploy/submit-for-approval.png){align="center"}

1. Anmelden bei [Adobe Developer-Konsole](https://developer.adobe.com)
1. Auswählen __Konsole__
1. Navigieren Sie zu __Projekte__
1. Wählen Sie das Projekt aus, das mit der Erweiterung verknüpft ist
1. Wählen Sie die __Produktion__ Arbeitsbereich
1. Auswählen __Zur Genehmigung einreichen__
1. Füllen Sie das Formular aus und senden Sie es ab und aktualisieren Sie die Felder nach Bedarf.

#### Genehmigen der Bereitstellungsanforderung

![Genehmigung der Erweiterung](./assets/deploy/adobe-exchange.png){align="center"}

1. Anmelden bei [Adobe Exchange](https://exchange.adobe.com/)
1. Navigieren Sie zu __Verwalten__ > __Apps steht noch aus Überprüfung__
1. __Überprüfen__ die App Builder-App der Erweiterung
1. Wenn die Erweiterungsänderungen akzeptabel sind __Accept__ die Überprüfung. Dadurch wird die Erweiterung sofort in alle AEM as a Cloud Service Autorendienste in der Adobe-Organisation eingefügt.

Sobald die Erweiterungsanforderung genehmigt wurde, wird die Erweiterung sofort in den AEM as a Cloud Service Autorendiensten aktiviert.

## Entfernen einer Erweiterung

![Entfernen einer Erweiterung](./assets/deploy/revoke.png)

Um eine Erweiterung zu entfernen, sperren (oder entfernen) Sie sie aus Adobe Exchange. Wenn die Erweiterung gesperrt wird, wird sie aus allen AEM as a Cloud Service Autorendiensten entfernt.

1. Anmelden bei [Adobe Exchange](https://exchange.adobe.com/)
1. Navigieren Sie zu __Verwalten__ > __App Builder-Apps__
1. __Sperren__ Die zu entfernende Erweiterung
