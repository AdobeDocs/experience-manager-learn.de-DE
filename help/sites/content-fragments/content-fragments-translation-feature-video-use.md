---
title: Übersetzungsunterstützung für AEM Inhaltsfragmente
description: Erfahren Sie, wie Inhaltsfragmente mit Adobe Experience Manager lokalisiert und übersetzt werden können. Mit einem Inhaltsfragment verknüpfte gemischte Medien-Assets können ebenfalls extrahiert und übersetzt werden.
feature: Content Fragments, Multi Site Manager
topic: Localization
role: User
level: Intermediate
version: 6.4, 6.5, Cloud Service
kt: 201
thumbnail: 18131.jpg
exl-id: cc4ffbd0-207a-42e4-bfcb-d6c83fb97237
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 2%

---

# Übersetzungsunterstützung für AEM Inhaltsfragmente {#translation-support-content-fragments}

Erfahren Sie, wie Inhaltsfragmente mit Adobe Experience Manager lokalisiert und übersetzt werden können. Mit einem Inhaltsfragment verknüpfte gemischte Medien-Assets können ebenfalls extrahiert und übersetzt werden.

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=12&learn=on)

## Anwendungsfälle für die Übersetzung von Inhaltsfragmenten {#content-fragment-translation-use-cases}

Inhaltsfragmente sind ein erkannter Inhaltstyp, der AEM Extrakte an einen externen Übersetzungsdienst sendet. Mehrere Anwendungsfälle werden standardmäßig unterstützt:

1. Ein Inhaltsfragment kann [direkt in der Assets-Konsole für Sprachkopie und Übersetzung ausgewählt](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html).
2. Inhaltsfragmente, auf die auf einer Sites-Seite verwiesen wird, werden in den entsprechenden Sprachordner kopiert und zur Übersetzung extrahiert, wenn die Sites-Seite für die Sprachkopie ausgewählt wird.
3. Inline-Medien-Assets, die in ein Inhaltsfragment eingebettet sind, können extrahiert und übersetzt werden.
4. Mit einem Inhaltsfragment verknüpfte Asset-Sammlungen können extrahiert und übersetzt werden.

## Editor für Übersetzungsregeln {#translation-rules-editor}

Das Übersetzungsverhalten von Experience Managern kann mithilfe des **Editor für Übersetzungsregeln**. Um die Übersetzung zu aktualisieren, navigieren Sie zu **Instrumente** > **Allgemein** > **Übersetzungskonfiguration** at [http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html).

Vordefinierte Konfigurationen referenzieren Inhaltsfragmente unter `fragmentPath` mit dem Ressourcentyp `core/wcm/components/contentfragment/v1/contentfragment`. Alle Komponenten, die von der `v1/contentfragment` werden durch die Standardkonfiguration erkannt.

![Editor für Übersetzungsregeln](assets/translation-configuration.png)
