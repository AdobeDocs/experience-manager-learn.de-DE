---
title: Kapitel 6 - Veröffentlichung des Inhalts in AEM Publish as JSON - Content Services
description: In Kapitel 6 des Tutorials AEM Headless wird sichergestellt, dass alle erforderlichen Pakete, Konfigurationen und Inhalte auf AEM Publish verfügbar sind, damit die App genutzt werden kann.
feature: Inhaltsfragmente, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 2%

---


# Kapitel 6 - Bereitstellen des Inhalts auf AEM Publish for Delivery

In Kapitel 6 des Tutorials AEM Headless wird sichergestellt, dass alle erforderlichen Pakete, Konfigurationen und Inhalte auf AEM Publish verfügbar sind, damit sie von der Mobile App genutzt werden können.

## Veröffentlichen des Inhalts für AEM Content Services

Die Konfiguration und der Inhalt, die erstellt wurden, um die Ereignisse über AEM Content Services zu leiten, müssen in AEM Publish veröffentlicht werden, damit die Mobile App darauf zugreifen kann.

Da AEM Content Services aus der Konfiguration (Inhaltsfragmentmodelle, bearbeitbare Vorlagen), Assets (Inhaltsfragmente, Bilder) und Seiten erstellt wurde, verfügen alle diese Elemente automatisch über AEM Inhaltsverwaltungsfunktionen, einschließlich:

* Workflow für Überprüfung und Verarbeitung
* und Aktivierung/Deaktivierung für das Pushen und Abrufen von Inhalten von den Endpunkten der AEM-Veröffentlichungs-AEM Content Services

1. Stellen Sie sicher, dass die **[!DNL WKND Mobile]Anwendungspakete**, die in [Kapitel 1](./chapter-1.md#wknd-mobile-application-packages) aufgelistet sind, auf **AEM Publish** mit [!UICONTROL Package Manager] installiert sind.
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Veröffentlichen Sie die **[!DNL WKND Mobile Events API]bearbeitbare Vorlage**
   1. Navigieren Sie zu **[!UICONTROL AEM] > [!UICONTROL Tools] > [!UICONTROL Allgemein] > [!UICONTROL Vorlagen] >[!DNL WKND Mobile]**
   1. Wählen Sie die Vorlage **[!DNL Event API]** aus.
   1. Tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Veröffentlichen]** .
   1. Veröffentlichen Sie die **Vorlage** und **alle Verweise** (Inhaltsrichtlinien, Inhaltsrichtlinienzuordnungen und Vorlagen).

1. Veröffentlichen Sie die **[!DNL WKND Mobile Events]Inhaltsfragmente**.

Beachten Sie, dass dies erforderlich ist, da die Ereignis-API die Komponente Inhaltsfragmentliste verwendet, die nicht speziell auf Inhaltsfragmente verweist.
1. Navigieren Sie zu **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Dateien] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
1. Wählen Sie alle **[!DNL Event]** Inhaltsfragmente aus.
1. Tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Veröffentlichung verwalten]** .
1. Wenn Sie die standardmäßige Aktion **Veröffentlichen** unverändert lassen, tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Weiter]** .
1. Wählen Sie **alle** Inhaltsfragmente aus.
1. Tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Veröffentlichen]** .
* *Das [!DNL Events] Inhaltsfragmentmodell und die Verweise auf Ereignisbilder werden automatisch zusammen mit den Inhaltsfragmenten veröffentlicht.*

1. Veröffentlichen Sie die **[!DNL Events API]-Seite**.
   1. Navigieren Sie zu **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. Wählen Sie die Seite **[!DNL Events]** aus.
   1. Tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Veröffentlichung verwalten]** .
   1. Wenn Sie die Standardaktion **Veröffentlichen** unverändert lassen, tippen Sie in der oberen Aktionsleiste auf **[!UICONTROL Weiter]** .
   1. Wählen Sie die Seite **[!DNL Events]** aus.
   1. Tippen Sie in der oberen Aktionsleiste auf **[!DNL Publish]** .

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## Überprüfen der AEM-Veröffentlichung

1. Stellen Sie in einem neuen Webbrowser sicher, dass Sie von der AEM-Veröffentlichungsinstanz abgemeldet sind und fordern Sie die folgenden URLs an (ersetzen Sie `http://localhost:4503` für Host:Port, auf dem AEM Publish ausgeführt wird).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   Diese Anfragen sollten dieselbe JSON-Antwort zurückgeben wie bei der Überprüfung der entsprechenden AEM-Autoren-Endpunkte. Ist dies nicht der Fall, stellen Sie sicher, dass alle Veröffentlichungen erfolgreich waren (überprüfen Sie die Replikations-Warteschlangen), das Paket [!DNL WKND Mobile] `ui.apps` wird auf AEM Publish installiert und überprüfen Sie das Paket `error.log` für AEM Publish.

## Nächster Schritt

Es sind keine zusätzlichen Pakete zu installieren. Stellen Sie sicher, dass der in diesem Abschnitt beschriebene Inhalt und die Konfiguration in AEM Publish veröffentlicht wurden. Ansonsten funktionieren nachfolgende Kapitel nicht.

* [Kapitel 7 - AEM von Content Services aus einer Mobile App](./chapter-7.md)
