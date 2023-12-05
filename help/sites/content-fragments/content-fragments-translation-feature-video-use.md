---
title: Übersetzungsunterstützung für AEM-Inhaltsfragmente
description: Erfahren Sie, wie Inhaltsfragmente mit Adobe Experience Manager lokalisiert und übersetzt werden können. Mit einem Inhaltsfragment verknüpfte gemischte Medien-Assets können ebenfalls extrahiert und übersetzt werden.
feature: Content Fragments, Multi Site Manager
topic: Localization
role: User
level: Intermediate
version: 6.4, 6.5, Cloud Service
jira: KT-201
thumbnail: 18131.jpg
doc-type: Feature Video
exl-id: cc4ffbd0-207a-42e4-bfcb-d6c83fb97237
duration: 245
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 100%

---

# Übersetzungsunterstützung für AEM-Inhaltsfragmente {#translation-support-content-fragments}

Erfahren Sie, wie Inhaltsfragmente mit Adobe Experience Manager lokalisiert und übersetzt werden können. Mit einem Inhaltsfragment verknüpfte gemischte Medien-Assets können ebenfalls extrahiert und übersetzt werden.

>[!VIDEO](https://video.tv.adobe.com/v/18131?quality=12&learn=on)

## Anwendungsfälle für die Übersetzung von Inhaltsfragmenten {#content-fragment-translation-use-cases}

Inhaltsfragmente sind ein anerkannter Content-Typ, den AEM extrahiert, um ihn an einen externen Übersetzungsdienst zu senden. Mehrere Anwendungsfälle werden standardmäßig unterstützt:

1. Ein Inhaltsfragment kann [direkt in der Assets-Konsole für Sprachkopie und Übersetzung](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html?lang=de) ausgewählt werden.
2. Inhaltsfragmente, auf die auf einer Sites-Seite verwiesen wird, werden in den entsprechenden Sprachordner kopiert und zur Übersetzung extrahiert, wenn die Sites-Seite für die Sprachkopie ausgewählt wird.
3. Inline-Medien-Assets, die in ein Inhaltsfragment eingebettet sind, können extrahiert und übersetzt werden.
4. Mit einem Inhaltsfragment verknüpfte Asset-Sammlungen können extrahiert und übersetzt werden.

## Editor für Übersetzungsregeln {#translation-rules-editor}

Das Übersetzungsverhalten von Experience Manager kann mithilfe des **Editors für Übersetzungsregeln** aktualisiert werden. Um die Übersetzung zu aktualisieren, navigieren Sie zu **Tools** > **Allgemein** > **Übersetzungskonfiguration** unter [http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html).

Standardkonfigurationen verweisen auf Inhaltsfragmente bei `fragmentPath` mit einem Ressourcentyp von `core/wcm/components/contentfragment/v1/contentfragment`. Alle Komponenten, die von `v1/contentfragment` erben, werden von der Standardkonfiguration erkannt.

![Editor für Übersetzungsregeln](assets/translation-configuration.png)
