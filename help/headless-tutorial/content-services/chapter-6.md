---
title: Kapitel 6 - Veröffentlichung des Inhalts in AEM Publish as JSON - Content Services
description: In Kapitel 6 des Tutorials AEM Headless wird sichergestellt, dass alle erforderlichen Pakete, Konfigurationen und Inhalte auf AEM Publish verfügbar sind, damit die App genutzt werden kann.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
source-git-commit: 25a1a40f42d37443db9edc0e09b1691b1c19e848
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 2%

---

# Kapitel 6 - Bereitstellen des Inhalts auf AEM Publish for Delivery

In Kapitel 6 des Tutorials AEM Headless wird sichergestellt, dass alle erforderlichen Pakete, Konfigurationen und Inhalte auf AEM Publish verfügbar sind, damit sie von der Mobile App genutzt werden können.

## Veröffentlichen des Inhalts für AEM Content Services

Die Konfiguration und der Inhalt, die erstellt wurden, um die Ereignisse über AEM Content Services zu leiten, müssen in AEM Publish veröffentlicht werden, damit die Mobile App darauf zugreifen kann.

Da AEM Content Services aus der Konfiguration (Inhaltsfragmentmodelle, bearbeitbare Vorlagen), Assets (Inhaltsfragmente, Bilder) und Seiten erstellt wurde, verfügen alle diese Elemente automatisch über AEM Inhaltsverwaltungsfunktionen, einschließlich:

* Workflow für Überprüfung und Verarbeitung
* und Aktivierung/Deaktivierung für das Pushen und Abrufen von Inhalten von den Endpunkten der AEM-Veröffentlichungs-AEM Content Services

1. Stellen Sie sicher, dass **[!DNL WKND Mobile]Anwendungspakete**, aufgeführt in [Kapitel 1](./chapter-1.md#wknd-mobile-application-packages), installiert auf **AEM-Veröffentlichung** using [!UICONTROL Package Manager].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Veröffentlichen Sie die **[!DNL WKND Mobile Events API]Bearbeitbare Vorlage**
   1. Navigieren Sie zu **[!UICONTROL AEM] > [!UICONTROL Instrumente] > [!UICONTROL Allgemein] > [!UICONTROL Vorlagen] >[!DNL WKND Mobile]**
   1. Wählen Sie die **[!DNL Event API]** template
   1. Tippen **[!UICONTROL Veröffentlichen]** in der oberen Aktionsleiste
   1. Veröffentlichen Sie die **template** und **alle Verweise** (Inhaltsrichtlinien, Inhaltsrichtlinienzuordnungen und Vorlagen)

1. Veröffentlichen Sie die **[!DNL WKND Mobile Events]Inhaltsfragmente**.

   Beachten Sie, dass dies erforderlich ist, da die Ereignis-API die Komponente Inhaltsfragmentliste verwendet, die nicht speziell auf Inhaltsfragmente verweist.

   1. Navigieren Sie zu **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Dateien] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. Wählen Sie alle **[!DNL Event]** Inhaltsfragmente
   1. Tippen Sie auf **[!UICONTROL Veröffentlichung verwalten]** in der oberen Aktionsleiste
   1. Behalten der Standardeinstellung bei **Veröffentlichen** Aktion unverändert, tippen Sie auf **[!UICONTROL Nächste]** in der oberen Aktionsleiste
   1. Auswählen **all** Inhaltsfragmente
   1. Tippen **[!UICONTROL Veröffentlichen]** in der oberen Aktionsleiste
      * *Die [!DNL Events] Inhaltsfragmentmodell und Verweise Ereignisbilder werden automatisch zusammen mit den Inhaltsfragmenten veröffentlicht.*

1. Veröffentlichen Sie die **[!DNL Events API]page**.
   1. Navigieren Sie zu **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. Wählen Sie die **[!DNL Events]** page
   1. Tippen Sie auf **[!UICONTROL Veröffentlichung verwalten]** in der oberen Aktionsleiste
   1. Behalten der Standardeinstellung bei **Veröffentlichen** Aktion unverändert, tippen Sie auf **[!UICONTROL Nächste]** in der oberen Aktionsleiste
   1. Wählen Sie die **[!DNL Events]** page
   1. Tippen **[!DNL Publish]** in der oberen Aktionsleiste

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## Überprüfen der AEM-Veröffentlichung

1. Stellen Sie in einem neuen Webbrowser sicher, dass Sie von der AEM-Veröffentlichung abgemeldet sind und die folgenden URLs anfordern (ersetzen durch `http://localhost:4503` für welchen Host: Port AEM Publish ausgeführt wird).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   Diese Anfragen sollten dieselbe JSON-Antwort zurückgeben wie bei der Überprüfung der entsprechenden AEM-Autoren-Endpunkte. Wenn dies nicht der Fall ist, stellen Sie sicher, dass alle Veröffentlichungen erfolgreich sind (überprüfen Sie die Replikations-Warteschlangen), [!DNL WKND Mobile] `ui.apps` Das Paket wird auf der AEM-Veröffentlichungsinstanz installiert und überprüfen Sie die `error.log` für AEM Publish.

## Nächster Schritt

Es sind keine zusätzlichen Pakete zu installieren. Stellen Sie sicher, dass der in diesem Abschnitt beschriebene Inhalt und die Konfiguration in AEM Publish veröffentlicht wurden. Ansonsten funktionieren nachfolgende Kapitel nicht.

* [Kapitel 7 - AEM von Content Services aus einer Mobile App](./chapter-7.md)
