---
title: Bereitstellen einer AEM-Benutzeroberflächen-Erweiterung
description: Erfahren Sie, wie Sie eine AEM-Benutzeroberflächen-Erweiterung bereitstellen.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 2e37165d-c003-4206-8133-54e37ca35b8e
duration: 228
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '765'
ht-degree: 100%

---

# Bereitstellen einer Erweiterung

Für die Verwendung in AEM as a Cloud Service-Umgebungen muss die App-Entwicklungs-Erweiterungs-App bereitgestellt und genehmigt werden.

Bei der Bereitstellung von App-Entwicklungs-Erweiterungs-Apps sind einige Aspekte zu beachten:

+ Erweiterungen werden im Adobe Developer Console-Projektarbeitsbereich bereitgestellt. Die standardmäßigen Arbeitsbereiche sind:
   + Der Arbeitsbereich __Produktion__ enthält Erweiterungsbereitstellungen, die immer in AEM as a Cloud Service verfügbar sind.
   + Der Arbeitsbereich __Staging__ dient als Entwicklerarbeitsbereich. Erweiterungen, die im Arbeitsbereich „Staging“ bereitgestellt werden, sind in AEM as a Cloud Service nicht verfügbar.
Adobe Developer Console-Arbeitsbereiche weisen keine direkte Verbindung zu AEM as a Cloud Service-Umgebungstypen auf.
+ Eine im Arbeitsbereich „Produktion“ bereitgestellte Erweiterung wird in allen AEM as a Cloud Service-Umgebungen in der Adobe-Organisation angezeigt, in der die Erweiterung vorhanden ist.
Die Beschränkung einer Erweiterung auf die Umgebungen, in denen sie registriert ist, kann aufgehoben werden, indem man eine [bedingte Logik hinzufügt, die den Host-Namen von AEM as a Cloud Service überprüft](https://developer.adobe.com/uix/docs/guides/publication/#enabling-extension-only-on-specific-aem-environments).
+ In AEM as a Cloud Service können mehrere Erweiterungen angewendet werden. Adobe empfiehlt, dass jede App-Entwicklungs-Erweiterungs-App verwendet wird, um ein einzelnes Geschäftsziel zu lösen. Eine einzige App-Entwicklungs-Erweiterungs-App kann jedoch mehrere Erweiterungspunkte implementieren, die ein gemeinsames Geschäftsziel unterstützen.

## Erstbereitstellung

Damit eine Erweiterung in AEM as a Cloud Service-Umgebungen verfügbar ist, muss sie in Adobe Developer Console bereitgestellt werden.

Der Bereitstellungsprozess gliedert sich in zwei logische Schritte:

1. Bereitstellung der App-Entwicklungs-Erweiterungs-App für Adobe Developer Console durch eine Entwicklungsperson
1. Genehmigung der Erweiterung durch Bereitstellungs-Managerin bzw. -Manager oder Geschäftsinhaberin bzw. -inhaber.

### Bereitstellen der Erweiterung

Stellen Sie die Erweiterung im Arbeitsbereich „Produktion“ bereit. Erweiterungen, die im Arbeitsbereich „Produktion“ bereitgestellt werden, werden automatisch allen Author-Services von AEM as a Cloud Service in der Adobe-Organisation hinzugefügt, in der die Erweiterung bereitgestellt wird.

1. Öffnen Sie eine Befehlszeile für das Stammverzeichnis der aktualisierten App-Entwicklungs-Erweiterungs-App.
1. Stellen Sie sicher, dass der Arbeitsbereich „Produktion“ aktiv ist.

   ```shell
   $ aio app use -w Production
   ```

   Führen Sie alle Änderungen in `.env` und `.aio` zusammen.

1. Stellen Sie die aktualisierte App-Entwicklungs-Erweiterungs-App bereit.

   ```shell
   $ aio app deploy
   ```

#### Anfrage zum Genehmigen der Bereitstellung

![Einreichen der Erweiterung zur Genehmigung](./assets/deploy/submit-for-approval.png){align="center"}

1. Melden Sie sich bei [Adobe Developer Console](https://developer.adobe.com) an.
1. Wählen Sie __Konsole__ aus.
1. Navigieren Sie zu __Projekte__.
1. Wählen Sie das Projekt aus, das mit der Erweiterung verknüpft ist.
1. Wählen Sie den Arbeitsbereich __Produktion__ aus.
1. Wählen Sie __Zur Genehmigung einreichen__ aus.
1. Füllen Sie das Formular aus, übermitteln Sie es und aktualisieren Sie die Felder nach Bedarf.

### Genehmigung der Bereitstellung

![Genehmigung der Erweiterung](./assets/deploy/adobe-exchange.png){align="center"}

1. Melden Sie sich bei [Adobe Exchange](https://exchange.adobe.com/) an.
1. Navigieren Sie zu __Verwalten__ > __Apps mit ausstehender Überprüfung__.
1. __Überprüfen__ Sie die App-Entwicklungs-Erweiterungs-App.
1. Wenn die Erweiterungsänderungen akzeptabel sind, __akzeptieren__ Sie die Überprüfung. Dadurch wird die Erweiterung direkt in alle Author-Services von AEM as a Cloud Service in der Adobe-Organisation eingefügt.

Sobald die Erweiterungsanfrage genehmigt wurde, wird die Erweiterung sofort in den Author-Services von AEM as a Cloud Service aktiviert.

## Aktualisieren einer Erweiterung

Die Aktualisierung einer App-Entwicklungs-Erweiterungs-App folgt dem gleichen Prozess wie die [Erstbereitstellung](#initial-deployment), außer dass die vorhandene Erweiterungsbereitstellung zuerst widerrufen werden muss.

### Widerrufen der Erweiterung

Um eine neue Version einer Erweiterung bereitzustellen, muss sie zunächst widerrufen (oder entfernt) werden. Eine widerrufene Erweiterung steht in AEM-Konsolen nicht mehr zur Verfügung.

1. Melden Sie sich bei [Adobe Exchange](https://exchange.adobe.com/) an.
1. Navigieren Sie zu __Verwalten__ > __App-Entwicklungs-Apps__.
1. __Widerrufen__ Sie die zu aktualisierende Erweiterung.

### Bereitstellen der Erweiterung

Stellen Sie die Erweiterung im Arbeitsbereich „Produktion“ bereit. Erweiterungen, die im Arbeitsbereich „Produktion“ bereitgestellt werden, werden automatisch allen Author-Services von AEM as a Cloud Service in der Adobe-Organisation hinzugefügt, in der die Erweiterung bereitgestellt wird.

1. Öffnen Sie eine Befehlszeile für das Stammverzeichnis der aktualisierten App-Entwicklungs-Erweiterungs-App.
1. Stellen Sie sicher, dass der Arbeitsbereich „Produktion“ aktiv ist.

   ```shell
   $ aio app use -w Production
   ```

   Führen Sie alle Änderungen in `.env` und `.aio` zusammen.

1. Stellen Sie die aktualisierte App-Entwicklungs-Erweiterungs-App bereit.

   ```shell
   $ aio app deploy
   ```

#### Anfrage zum Genehmigen der Bereitstellung

![Einreichen der Erweiterung zur Genehmigung](./assets/deploy/submit-for-approval.png){align="center"}

1. Melden Sie sich bei [Adobe Developer Console](https://developer.adobe.com) an.
1. Wählen Sie __Konsole__ aus.
1. Navigieren Sie zu __Projekte__.
1. Wählen Sie das Projekt aus, das mit der Erweiterung verknüpft ist.
1. Wählen Sie den Arbeitsbereich __Produktion__ aus.
1. Wählen Sie __Zur Genehmigung einreichen__ aus.
1. Füllen Sie das Formular aus, übermitteln Sie es und aktualisieren Sie die Felder nach Bedarf.

#### Genehmigen der Bereitstellungsanfrage

![Genehmigung der Erweiterung](./assets/deploy/adobe-exchange.png){align="center"}

1. Melden Sie sich bei [Adobe Exchange](https://exchange.adobe.com/) an.
1. Navigieren Sie zu __Verwalten__ > __Apps mit ausstehender Überprüfung__.
1. __Überprüfen__ Sie die App-Entwicklungs-Erweiterungs-App.
1. Wenn die Erweiterungsänderungen akzeptabel sind, __akzeptieren__ Sie die Überprüfung. Dadurch wird die Erweiterung direkt in alle Author-Services von AEM as a Cloud Service in der Adobe-Organisation eingefügt.

Sobald die Erweiterungsanfrage genehmigt wurde, wird die Erweiterung sofort in den Author-Services von AEM as a Cloud Service aktiviert.

## Entfernen einer Erweiterung

![Entfernen einer Erweiterung](./assets/deploy/revoke.png)

Um eine Erweiterung zu entfernen, müssen Sie sie für Adobe Exchange widerrufen (oder daraus entfernen). Eine widerrufene Erweiterung wird aus allen Author-Services von AEM as a Cloud Service entfernt.

1. Melden Sie sich bei [Adobe Exchange](https://exchange.adobe.com/) an.
1. Navigieren Sie zu __Verwalten__ > __App-Entwicklungs-Apps__.
1. __Widerrufen__ Sie die zu entfernende Erweiterung.
