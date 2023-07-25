---
title: Erstellen einer Tag-Eigenschaft
description: Erfahren Sie, wie Sie eine Tag-Eigenschaft mit der Mindestkonfiguration erstellen, die in AEM integriert werden kann. Benutzer werden in die Tag-Benutzeroberfläche eingeführt und erfahren mehr über Erweiterungen, Regeln und Veröffentlichungs-Workflows.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 5980
thumbnail: 38553.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: d5de62ef-a2aa-4283-b500-e1f7cb5dec3b
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '607'
ht-degree: 1%

---

# Erstellen einer Tag-Eigenschaft {#create-tag-property}

Erfahren Sie, wie Sie eine Tag-Eigenschaft mit der Mindestkonfiguration erstellen, um sie in Adobe Experience Manager zu integrieren. Benutzer werden in die Tag-Benutzeroberfläche eingeführt und erfahren mehr über Erweiterungen, Regeln und Veröffentlichungs-Workflows.

>[!VIDEO](https://video.tv.adobe.com/v/38553?quality=12&learn=on)

## Erstellung von Tag-Eigenschaften

Um eine Tag-Eigenschaft zu erstellen, führen Sie die folgenden Schritte aus.

1. Navigieren Sie im Browser zum [Adobe Experience Cloud - Startseite](https://experience.adobe.com/) und melden Sie sich mit Ihrem Adobe ID an.

1. Klicken Sie auf **Datenerfassung** der _Schnellzugriff_ auf der Adobe Experience Cloud-Startseite.

1. Klicken Sie auf **Tags** Menüelement aus der linken Navigation und klicken Sie dann auf **Neue Eigenschaft** oben rechts.

1. Benennen Sie Ihre Tag-Eigenschaft mit dem **Name** erforderliches Feld. Geben Sie für das Feld Domänen Ihren Domänennamen ein oder geben Sie bei Verwendung AEM as a Cloud Service Umgebung `adobeaemcloud.com` und klicken Sie auf **Speichern**.

   ![Tag-Eigenschaften](assets/tag-properties.png)

## Neue Regel erstellen

Öffnen Sie die neu erstellte Tag-Eigenschaft, indem Sie auf ihren Namen im **Tag-Eigenschaften** anzeigen. Auch unter _Meine letzte Aktivität_ -Überschrift angezeigt, sollte die Haupterweiterung hinzugefügt werden. Die Core Tag-Erweiterung ist die Standarderweiterung und bietet grundlegende Ereignistypen wie Seitenladevorgang, Browser, Formular und andere Ereignistypen, siehe [Übersicht über die Haupterweiterung](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/core/overview.html) für weitere Informationen.

Mit Regeln können Sie festlegen, was passieren soll, wenn der Besucher mit Ihrer AEM-Site interagiert. Um die Dinge einfach zu halten, protokollieren wir zwei Meldungen an die Browser-Konsole, um zu zeigen, wie die Datenerfassungs-Tag-Integration JavaScript-Code in Ihre AEM-Site einfügen kann, ohne AEM Projekt-Code zu aktualisieren.

Um eine Regel zu erstellen, führen Sie die folgenden Schritte aus.

1. Klicken **Regeln** von _AUTHORING_ Abschnitt der linken Navigation und klicken Sie dann auf **Neue Regel erstellen**

1. Benennen Sie Ihre Regel mithilfe des **Name** erforderliches Feld.

1. Klicken **Hinzufügen** von _EREIGNISSE_ und anschließend im _Ereigniskonfiguration_ im **Ereignistyp** Dropdown-Auswahl _Bibliothek geladen (Seitenanfang)_ und klicken Sie auf **Änderungen beibehalten**.

1. Klicken **Hinzufügen** von _AKTIONEN_ und anschließend im _Aktionskonfiguration_ im **Aktionstyp** Dropdown-Auswahl _Benutzerspezifischer Code_ und klicken Sie auf **Editor öffnen**.

1. Im _Code bearbeiten_ modal, geben Sie folgendes JavaScript-Codefragment ein und klicken Sie dann auf **Speichern** und klicken Sie schließlich auf **Änderungen beibehalten**.

   ```javascript
   console.log('Tags Property loaded, all set for...');
   console.log('capabilities such as capturing data, conversion tracking and delivering unique and personalized experiences');
   ```

1. Klicken **Speichern** , um den Regelerstellungsprozess abzuschließen.

   ![Neue Regel](assets/new-rule.png)

## Bibliothek hinzufügen und veröffentlichen

Tag-Eigenschaft _Regeln_ werden mithilfe einer Bibliothek aktiviert, stellen Sie sich die Bibliothek als Paket mit JavaScript-Code vor. Aktivieren Sie die neu erstellte Regel, indem Sie die Schritte ausführen.

1. Klicken **Veröffentlichungsfluss** von _VERÖFFENTLICHUNG_ Abschnitt der linken Navigation und klicken Sie auf **Bibliothek hinzufügen**

1. Benennen Sie Ihre Bibliothek mit **Name** Feld und wählen Sie _Entwicklung (Entwicklung)_ -Option für **Umgebung** Dropdown-Liste.

1. Um alle seit der Erstellung der Tag-Eigenschaft geänderten Ressourcen auszuwählen, klicken Sie auf **+ Alle geänderten Ressourcen hinzufügen**. Durch diese Aktion werden die neu erstellte Regel und die Ressource mit der Haupterweiterung zur Bibliothek hinzugefügt. Endlich klicken **Speichern und in Entwicklung erstellen**.

1. Sobald die Bibliothek für die **Entwicklung** Swim-Spur mit _Ellipsen_ wählen Sie die **Zur Genehmigung einreichen**

1. Dann in der **Gesendet** Swimming-Spur mit _Ellipsen_ wählen Sie die **Zur Veröffentlichung genehmigen**, ebenfalls **Erstellen und in Produktion veröffentlichen** im **Genehmigt** schwimmen.

![Veröffentlichte Bibliothek](assets/published-library.png)


Der obige Schritt schließt die einfache Erstellung der Tag-Eigenschaft ab, die über eine Regel zum Protokollieren einer Nachricht in der Browser-Konsole verfügt, wenn die Seite geladen wird. Außerdem werden die Regel und die Haupterweiterung durch Erstellen einer Bibliothek veröffentlicht.

## Nächste Schritte

[AEM mit Tag-Eigenschaft über IMS verbinden](connect-aem-tag-property-using-ims.md)


## Zusätzliche Ressourcen {#additional-resources}

* [Erstellen einer Tag-Eigenschaft](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html)
