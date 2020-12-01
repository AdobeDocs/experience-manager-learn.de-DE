---
title: Verwenden der Übersetzung mit AEM Inhaltsfragmenten
description: AEM 6.3 bietet die Möglichkeit, Inhaltsfragmente zu übersetzen. Assets mit gemischten Medien und Asset-Sammlungen, die mit einem Inhaltsfragment verknüpft sind, können ebenfalls extrahiert und übersetzt werden.
sub-product: Sites, Assets, Content-Services
feature: content-fragments, multi-site-manager
topics: localization, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 6%

---


# Verwenden der Übersetzung mit AEM Inhaltsfragmenten{#using-translation-with-aem-content-fragments}

AEM 6.3 bietet die Möglichkeit, Inhaltsfragmente zu übersetzen. Assets mit gemischten Medien und Asset-Sammlungen, die mit einem Inhaltsfragment verknüpft sind, können ebenfalls extrahiert und übersetzt werden.

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=9&learn=on)

## Übersetzung von Inhaltsfragmenten - Verwendungsfälle {#content-fragment-translation-use-cases}

Inhaltsfragmente sind ein erkannter Inhaltstyp, der AEM extrahiert und an einen externen Übersetzungsdienst gesendet wird. Einige Anwendungsfälle werden standardmäßig unterstützt:

1. Ein Inhaltsfragment kann direkt in der Assets-Konsole für Sprachkopie und Übersetzung ausgewählt werden
2. Inhaltsfragmente, auf die auf einer Seite &quot;Sites&quot;verwiesen wird, werden in den entsprechenden Sprachordner kopiert und zur Übersetzung extrahiert, wenn die Seite &quot;Sites&quot;für die Sprachkopie ausgewählt wird
3. Inline-Medien-Assets, die in ein Inhaltsfragment eingebettet sind, können extrahiert und übersetzt werden.
4. Mit einem Inhaltsfragment verknüpfte Asset-Sammlungen können extrahiert und übersetzt werden.

## Übersetzungskonfigurationsoptionen {#translation-config-options}

Die standardmäßige Übersetzungskonfiguration unterstützt mehrere Optionen zum Übersetzen von Inhaltsfragmenten. Standardmäßig werden Inline-Medien-Assets und zugehörige Asset-Sammlungen NICHT übersetzt. Um die Übersetzungskonfiguration zu aktualisieren, navigieren Sie zu [http://localhost:4502/etc/cloudservices/translation/default_translation.html](http://localhost:4502/etc/cloudservices/translation/default_translation.html).

Es gibt vier Optionen zum Übersetzen von Inhaltsfragment-Assets:

1. **Nicht übersetzen (Standard)**
2. **Nur Inline-Medienassets**
3. **Nur verknüpfte Asset-Sammlungen**
4. **Eingebundene Medien-Assets und zugeordnete Sammlungen**

![Übersetzungskonfiguration](assets/classic-ui-dialog.png)
