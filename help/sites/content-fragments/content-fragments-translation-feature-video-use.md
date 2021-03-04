---
title: Übersetzungsunterstützung für AEM Inhaltsfragmente
description: Erfahren Sie, wie Inhaltsfragmente mit Adobe Experience Manager lokalisiert und übersetzt werden können. Assets mit gemischten Medien, die mit einem Inhaltsfragment verknüpft sind, können ebenfalls extrahiert und übersetzt werden.
feature: Inhaltsfragmente, Multi-Site-Manager
topic: Lokalisierung
role: Geschäftspraktiker
level: Zwischenschaltung
version: 6.3, 6.4, 6.5, cloud-service
kt: 201
thumbnail: 18131.jpg
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 2%

---


# Übersetzungsunterstützung für AEM Inhaltsfragmente {#translation-support-content-fragments}

Erfahren Sie, wie Inhaltsfragmente mit Adobe Experience Manager lokalisiert und übersetzt werden können. Assets mit gemischten Medien, die mit einem Inhaltsfragment verknüpft sind, können ebenfalls extrahiert und übersetzt werden.

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=12&learn=on)

## Übersetzung von Inhaltsfragmenten - Verwendungsfälle {#content-fragment-translation-use-cases}

Inhaltsfragmente sind ein erkannter Inhaltstyp, AEM Auszüge an einen externen Übersetzungsdienst gesendet werden. Einige Anwendungsfälle werden standardmäßig unterstützt:

1. Ein Inhaltsfragment kann direkt in der Assets-Konsole für Sprachkopie und Übersetzung ausgewählt werden
2. Inhaltsfragmente, auf die auf einer Seite &quot;Sites&quot;verwiesen wird, werden in den entsprechenden Sprachordner kopiert und zur Übersetzung extrahiert, wenn die Seite &quot;Sites&quot;für die Sprachkopie ausgewählt wird
3. Inline-Medien-Assets, die in ein Inhaltsfragment eingebettet sind, können extrahiert und übersetzt werden.
4. Mit einem Inhaltsfragment verknüpfte Asset-Sammlungen können extrahiert und übersetzt werden.

## Editor für Übersetzungsregeln {#translation-rules-editor}

Das Übersetzungsverhalten von Experience Managern kann mit dem **Übersetzungsregel-Editor** aktualisiert werden. Um die Übersetzung zu aktualisieren, navigieren Sie zu **Tools** > **Allgemein** > **Übersetzungskonfiguration** unter [http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html).

Standardmäßige Konfigurationen verweisen auf Inhaltsfragmente bei `fragmentPath` mit einem Ressourcentyp von `core/wcm/components/contentfragment/v1/contentfragment`. Alle Komponenten, die von `v1/contentfragment` erben, werden von der Standardkonfiguration erkannt.

![Editor für Übersetzungsregeln](assets/translation-configuration.png)
