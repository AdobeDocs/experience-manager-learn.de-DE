---
title: Erste Schritte mit AEM Kopflos - Kapitel 6 - Bereitstellen des Inhalts auf AEM Publish als JSON
description: Kapitel 6 des AEM Headless-Lernprogramms umfasst die Sicherstellung, dass alle erforderlichen Pakete, Konfigurationen und Inhalte auf AEM Publish verfügbar sind, damit der Verbrauch über die mobile App möglich ist.
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 2%

---


# Kapitel 6 - Bereitstellen der Inhalte auf AEM Publish for Versand

Kapitel 6 des AEM Headless-Lernprogramms umfasst die Sicherstellung, dass alle erforderlichen Pakete, Konfigurationen und Inhalte auf AEM Publish verfügbar sind, damit die Mobile App genutzt werden kann.

## Veröffentlichen von Inhalten für AEM Content Services

Die Konfiguration und die Inhalte, die erstellt wurden, um die Ereignis über AEM Content Services zu leiten, müssen in AEM Publish veröffentlicht werden, damit die Mobile App darauf zugreifen kann.

Da AEM Content Services auf der Grundlage von Configuration (Inhaltsfragmentmodelle, bearbeitbare Vorlagen), Assets (Inhaltsfragmente, Bilder) und Seiten erstellt wurde, verfügen alle diese Elemente automatisch über AEM Content-Management-Funktionen, einschließlich:

* Arbeitsablauf für Review und Verarbeitung
* und Aktivierung/Deaktivierung zum Verschieben und Abrufen von Inhalten aus den AEM Content Services-Endpunkten von AEM Publish

1. Vergewissern Sie sich, dass die **[!DNL WKND Mobile]Anwendungspakete**, die in [Kapitel 1](./chapter-1.md#wknd-mobile-application-packages)aufgeführt sind, auf **AEM Publish** mit [!UICONTROL Package Manager]installiert sind.
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Veröffentlichen der **[!DNL WKND Mobile Events API]bearbeitbaren Vorlage**
   1. Navigate to **[!UICONTROL AEM]>[!UICONTROL Tools]>[!UICONTROL General]>[!UICONTROL Templates]>[!DNL WKND Mobile]**
   1. Select the **[!DNL Event API]** template
   1. Tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Veröffentlichen]** .
   1. Veröffentlichen Sie die **Vorlage** und **alle Verweise** (Inhaltsrichtlinien, Zuordnungen von Inhaltsrichtlinien und Vorlagen).

1. Veröffentlichen Sie die **[!DNL WKND Mobile Events]Inhaltsfragmente**.

Beachten Sie, dass dies erforderlich ist, da die Ereignisses-API die Komponente &quot;Inhaltsfragment-Liste&quot;verwendet, die nicht speziell auf Inhaltsfragmente verweist.
1. Navigieren Sie zu **[!UICONTROL AEM]>[!UICONTROL Assets]>[!UICONTROL Dateien]>[!DNL WKND Mobile]>[!DNL English]>[!DNL Events]** 1. Wählen Sie alle **[!DNL Event]** Inhaltsfragmente aus1. Tippen Sie in der oberen Aktionsleiste auf die Schaltfläche Veröffentlichung **[!UICONTROL verwalten]** 1. Wenn Sie die standardmäßige **Veröffentlichungsaktion** unverändert beibehalten, tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Weiter]** . Wählen Sie **alle** Inhaltsfragmente aus1. Tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Veröffentlichen]** * *Das[!DNL Events]Inhaltsfragmentmodell und die Referenzen Ereignis Images werden automatisch zusammen mit den Inhaltsfragmenten veröffentlicht.*

1. Publish the **[!DNL Events API]page**.
   1. Navigieren Sie zu **[!UICONTROL AEM]>[!UICONTROL Sites]>[!DNL WKND Mobile]>[!DNL English]>[!DNL API]**
   1. Wählen Sie die **[!DNL Events]** Seite
   1. Tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Veröffentlichung]** verwalten
   1. Wenn Sie die standardmäßige **Veröffentlichungsaktion** unverändert beibehalten, tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Weiter]** .
   1. Wählen Sie die **[!DNL Events]** Seite
   1. Tippen Sie **[!DNL Publish]** in der oberen Aktionsleiste

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## AEM-Veröffentlichung überprüfen

1. Stellen Sie in einem neuen Webbrowser sicher, dass Sie bei AEM Publish abgemeldet sind, und fordern Sie die folgenden URLs an (wobei &quot;host:port&quot; `http://localhost:4503` für AEM Publish aktiviert ist).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   Diese Anforderungen sollten dieselbe JSON-Antwort zurückgeben wie bei der Überprüfung der entsprechenden AEM Author-Endpunkte. Wenn dies nicht der Fall ist, stellen Sie sicher, dass alle Veröffentlichungen erfolgreich sind (überprüfen Sie die Replikationswarteschlangen), das [!DNL WKND Mobile] Paket auf AEM Publish installiert ist, und überprüfen Sie den Abschnitt `ui.apps` `error.log` für AEM Publish.

## Nächster Schritt

Es sind keine zusätzlichen Pakete zu installieren. Vergewissern Sie sich, dass der Inhalt und die Konfiguration, die in diesem Abschnitt erläutert werden, in AEM Publish veröffentlicht sind. Andernfalls funktionieren nachfolgende Kapitel nicht.

* [Kapitel 7 - Verwenden AEM Content Services von einer mobilen App](./chapter-7.md)
