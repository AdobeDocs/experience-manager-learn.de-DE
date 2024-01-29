---
title: Einrichten von Übersetzungsregeln in AEM
description: Über die Benutzeroberfläche für die Übersetzungskonfiguration können Benutzende Regeln für die Übersetzung von Inhalten in AEM Sites verwalten. In diesem Video wird die Erstellung einer neuen Übersetzungsregel für eine benutzerdefinierte Komponente beschrieben.
feature: Language Copy
version: 6.4, 6.5
topic: Localization
role: User
level: Beginner
doc-type: Technical Video
exl-id: 359da531-839c-4680-abf9-c880cc700159
duration: 558
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '283'
ht-degree: 100%

---

# Einrichten von Übersetzungsregeln {#set-up-translation-rules-in-aem}

Über die Benutzeroberfläche für die Übersetzungskonfiguration können Benutzende Regeln für die Übersetzung von Inhalten in AEM Sites verwalten. In diesem Video wird die Erstellung einer neuen Übersetzungsregel für eine benutzerdefinierte Komponente beschrieben.

>[!NOTE]
>
> Das folgende Video wurde in AEM 6.3 aufgenommen. Ab AEM 6.4 wird eine neue Repository-Struktur zum Speichern der XML-Datei für Übersetzungsregeln eingeführt. Bei Verwendung der Benutzeroberfläche für die Übersetzungskonfiguration ab AEM 6.4 werden die Regeln am Speicherort `/conf/global/settings/translation/rules/translation_rules.xml` gespeichert. Weitere Details finden Sie unter [Identifizieren zu übersetzender Inhalte](https://helpx.adobe.com/de/experience-manager/6-5/sites/administering/using/tc-rules.html).

>[!VIDEO](https://video.tv.adobe.com/v/18135?quality=12&learn=on)

Übersetzungsregeln identifizieren Inhalte in AEM, die für die Übersetzung extrahiert werden sollen. Vordefinierte Übersetzungsregeln decken häufige Anwendungsfälle ab, wie Textkomponenten und Alternativtext für Bildkomponenten. Je nach den Anforderungen an die Übersetzungsprojekte können zusätzliche Regeln erforderlich sein. Im Allgemeinen ermöglichen es Übersetzungsregeln Benutzenden, Folgendes anzugeben:

1. Eigenschaften, die basierend auf Pfad- und/oder Ressourcentyp übersetzt werden sollen
2. Filter für Eigenschaften, die NICHT übersetzt werden sollen
3. Referenzierte Inhalte, die übersetzt werden sollen (d. h. Bilder oder Inhaltsfragmente)

Editor für Übersetzungsregeln, der die XML-Übersetzungsdatei aktualisiert. Die Benutzeroberfläche für die Übersetzungskonfiguration erleichtert die Verwaltung verschiedener Übersetzungsregeln und schützt vor Schreibfehlern bei der direkten Bearbeitung von XML.

Zugriff auf die Benutzeroberfläche der Übersetzungskonfiguration:

* **[!UICONTROL AEM Startmenü] > [!UICONTROL Tools] > [!UICONTROL Allgemein] > [[!UICONTROL Übersetzungskonfiguration]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## Vor AEM 6.3 {#prior-to-aem}

In früheren AEM-Versionen wurden die Übersetzungsregeln manuell aktualisiert, indem eine XML-Datei bearbeitet wurde, die sich im Übersetzungs-Workflow befindet: `/etc/workflow/models/translation/translation_rules.xml`.

## Zusätzliche Ressourcen {#additional-resources}

* [Identifizieren zu übersetzender Inhalte](https://helpx.adobe.com/de/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [Übersetzen von Inhalten für mehrsprachige Sites](https://helpx.adobe.com/de/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/de/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/de/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [Best Practices für die Übersetzung](https://helpx.adobe.com/de/experience-manager/6-5/sites/administering/using/tc-bp.html)
