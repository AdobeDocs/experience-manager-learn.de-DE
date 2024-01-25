---
title: Kapitel 6 – Bereitstellung des Inhalts auf AEM Publish als JSON – Content Services
description: In Kapitel 6 des AEM Headless-Tutorial wird sichergestellt, dass alle erforderlichen Pakete, Konfigurationen und Inhalte auf AEM Publish verfügbar sind, damit sie über die mobile App genutzt werden können.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
duration: 210
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 100%

---

# Kapitel 6 – Bereitstellung des Inhalts auf AEM Publish zum Versand

Kapitel 6 des AEM Headless Tutorial stellt sicher, dass alle erforderlichen Pakete, Konfigurationen und Inhalte auf AEM Publish verfügbar sind, damit sie von der mobilen App genutzt werden können.

## Veröffentlichen des Inhalts für AEM Content Services

Die Konfiguration und der Inhalt, die erstellt wurden, um die Ereignisse über AEM Content Services zu leiten, müssen in AEM Publish veröffentlicht werden, damit die mobile App darauf zugreifen kann.

Da AEM Content Services aus der Konfiguration (Inhaltsfragmentmodelle, bearbeitbare Vorlagen), Assets (Inhaltsfragmente, Bilder) und Seiten erstellt wurde, verfügen alle diese Elemente automatisch über AEMs Content-Management, einschließlich:

* Workflow für Überprüfung und Verarbeitung
* und Aktivierung/Deaktivierung für das Pushen und Abrufen von Inhalten von den Endpunkten des AEM Content Services von AEM Publish.

1. Stellen Sie sicher, dass die **[!DNL WKND Mobile]Anwendungspakete**, die in [Kapitel 1](./chapter-1.md#wknd-mobile-application-packages) aufgeführt sind, mit dem [!UICONTROL Package Manager] auf **AEM Publish** installiert werden.
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Veröffentlichen Sie die **[!DNL WKND Mobile Events API]Bearbeitbare Vorlage**
   1. Navigieren Sie zu **[!UICONTROL AEM] > [!UICONTROL Tools] > [!UICONTROL Allgemein] > [!UICONTROL Vorlagen] >[!DNL WKND Mobile]** 
   1. Wählen Sie die Vorlage **[!DNL Event API]**
   1. Tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Veröffentlichen]**
   1. Veröffentlichen Sie die **Vorlage** und **alle Verweise** (Inhaltsrichtlinien, Inhaltsrichtlinienzuordnungen und Vorlagen)

1. Veröffentlichen Sie die **[!DNL WKND Mobile Events]-Inhaltsfragmente**.

   Beachten Sie, dass dies erforderlich ist, da die Ereignis-API die Komponente Inhaltsfragmentliste verwendet, die nicht speziell auf Inhaltsfragmente verweist.

   1. Navigieren Sie zu **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Dateien] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. Wählen Sie alle **[!DNL Event]**-Inhaltsfragmente aus
   1. Tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Veröffentlichung verwalten]**
   1. Wenn Sie die Standardaktion **Veröffentlichen** beibehalten möchten, tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Weiter]**
   1. Wählen Sie **alle** Inhaltsfragmente aus
   1. Tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Veröffentlichen]**
      * *Das [!DNL Events]-Inhaltsfragmentmodell und die Verweise auf Event-Bilder werden automatisch zusammen mit den Inhaltsfragmenten veröffentlicht.*

1. Veröffentlichen Sie die **[!DNL Events API]Seite**.
   1. Navigieren Sie zu **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. Wählen Sie die Seite **[!DNL Events]**
   1. Tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Veröffentlichung verwalten]**
   1. Wenn Sie die Standardaktion **Veröffentlichen** beibehalten möchten, tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Weiter]**
   1. Wählen Sie die Seite **[!DNL Events]**
   1. Tippen Sie in der oberen Aktionsleiste auf **[!DNL Publish]**

>[!VIDEO](https://video.tv.adobe.com/v/28343?quality=12&learn=on)

## Überprüfen von AEM Publish

1. Stellen Sie in einem neuen Webbrowser sicher, dass Sie von der AEM Publish abgemeldet sind und fordern Sie die folgenden URLs an (`http://localhost:4503` durch irgendeinen Host:Port ersetzen, auf dem AEM Publish ausgeführt wird).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)

   Diese Anfragen sollten dieselbe JSON-Antwort zurückgeben wie bei der Überprüfung der entsprechenden AEM Author-Endpunkte. Wenn dies nicht der Fall ist, stellen Sie sicher, dass alle Veröffentlichungen erfolgreich waren (überprüfen Sie die Replikations-Warteschlangen), das [!DNL WKND Mobile] `ui.apps`-Paket auf der AEM Publish installiert ist und überprüfen Sie das `error.log` für AEM Publish.

## Nächster Schritt

Es müssen keine zusätzlichen Pakete installiert werden. Stellen Sie sicher, dass der in diesem Abschnitt beschriebene Inhalt und die Konfiguration in AEM Publish veröffentlicht wurden. Ansonsten funktionieren nachfolgende Kapitel nicht.

* [Kapitel 7 – Verwenden von AEM Content Services über eine Mobile App](./chapter-7.md)
