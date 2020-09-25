---
title: Einrichten von Übersetzungsregeln in AEM
description: Die Benutzeroberfläche der Übersetzungskonfiguration ermöglicht es dem Benutzer, Regeln für die Übersetzung von Inhalten in AEM Sites zu verwalten. In diesem Video wird die Erstellung einer neuen Übersetzungsregel für eine benutzerdefinierte Komponente erläutert.
feature: language-copy
topics: localization, content-architecture
audience: developer, administrator
doc-type: technical video
activity: setup
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '321'
ht-degree: 4%

---


# Einrichten von Übersetzungsregeln {#set-up-translation-rules-in-aem}

Die Benutzeroberfläche der Übersetzungskonfiguration ermöglicht es dem Benutzer, Regeln für die Übersetzung von Inhalten in AEM Sites zu verwalten. In diesem Video wird die Erstellung einer neuen Übersetzungsregel für eine benutzerdefinierte Komponente erläutert.

>[!NOTE]
>
> Das folgende Video wurde am AEM 6.3 aufgenommen. AEM 6.4+ führt eine neue Repository-Struktur zum Speichern der XML-Datei mit den Übersetzungsregeln ein. Bei Verwendung der Benutzeroberfläche der Übersetzungskonfiguration in AEM 6.4 und höher werden die Regeln am Speicherort gespeichert `/conf/global/settings/translation/rules/translation_rules.xml`. Weitere Informationen finden Sie unter [Identifizieren von zu übersetzenden](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html) Inhalten.

>[!VIDEO](https://video.tv.adobe.com/v/18135/?quality=9&learn=on)

Übersetzungsregeln identifizieren Inhalte in AEM, die zur Übersetzung extrahiert werden sollen. Vordefinierte Übersetzungsregeln decken gängige Anwendungsfälle wie Textkomponenten und Alternativtext für Bildkomponenten ab. Je nach Übersetzungsanforderungen eines Projekts sind ggf. zusätzliche Regeln erforderlich. Im Allgemeinen können Benutzer mithilfe von Übersetzungsregeln Folgendes angeben:

1. Eigenschaften, die basierend auf Pfad und/oder Ressourcentyp übersetzt werden sollten
2. Filter für Eigenschaften, die NICHT übersetzt werden sollten
3. Referenzierte Inhalte, die übersetzt werden sollen (z. B. Bilder oder Inhaltsfragmente)

Der Übersetzungsregel-Editor, der die XML-Datei für die Übersetzung aktualisiert. Die Benutzeroberfläche der Übersetzungskonfiguration erleichtert die Verwaltung verschiedener Übersetzungsregeln und schützt vor Typos, wenn XML direkt bearbeitet wird.

Zugriff auf die Benutzeroberfläche der Übersetzungskonfiguration:

* **[!UICONTROL Menü]AEM Beginn >[!UICONTROL Tools]>[!UICONTROL Allgemein]>[[!UICONTROL Übersetzungskonfiguration]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## Vor AEM 6.3 {#prior-to-aem}

In früheren AEM wurden die Übersetzungsregeln manuell aktualisiert, indem eine XML-Datei bearbeitet wurde, die sich im Arbeitsablauf für die Übersetzung befindet: `/etc/workflow/models/translation/translation_rules.xml`.

## Zusätzliche Ressourcen {#additional-resources}

* [Identifizieren zu übersetzender Inhalte](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [Übersetzen von Inhalten für mehrsprachige Sites](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [Best Practices für die Übersetzung](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)
