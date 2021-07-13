---
title: Einrichten von Übersetzungsregeln in AEM
description: Über die Benutzeroberfläche für die Übersetzungskonfiguration können Benutzer Regeln für die Übersetzung von Inhalten in AEM Sites verwalten. In diesem Video wird die Erstellung einer neuen Übersetzungsregel für eine benutzerdefinierte Komponente beschrieben.
feature: Sprachkopie
topics: localization, content-architecture
audience: developer, administrator
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Lokalisierung
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 7%

---


# Einrichten von Übersetzungsregeln {#set-up-translation-rules-in-aem}

Über die Benutzeroberfläche für die Übersetzungskonfiguration können Benutzer Regeln für die Übersetzung von Inhalten in AEM Sites verwalten. In diesem Video wird die Erstellung einer neuen Übersetzungsregel für eine benutzerdefinierte Komponente beschrieben.

>[!NOTE]
>
> Das folgende Video wurde in AEM 6.3 aufgezeichnet. AEM 6.4+ führt eine neue Repository-Struktur zum Speichern der XML-Datei für Übersetzungsregeln ein. Bei Verwendung der Benutzeroberfläche für die Übersetzungskonfiguration in AEM 6.4+ werden die Regeln am Speicherort `/conf/global/settings/translation/rules/translation_rules.xml` gespeichert. Weitere Informationen finden Sie unter [Identifizieren von zu übersetzenden Inhalten](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html) .

>[!VIDEO](https://video.tv.adobe.com/v/18135/?quality=9&learn=on)

Übersetzungsregeln identifizieren Inhalte in AEM, die zur Übersetzung extrahiert werden sollen. Vordefinierte Übersetzungsregeln decken gängige Anwendungsfälle ab, wie Textkomponenten und Alternativtext für Bildkomponenten. Abhängig von den Anforderungen an die Übersetzung von Projekten können zusätzliche Regeln erforderlich sein. Im Allgemeinen ermöglichen es Übersetzungsregeln Benutzern, Folgendes anzugeben:

1. Eigenschaften, die basierend auf Pfad und/oder Ressourcentyp übersetzt werden sollen
2. Filter für Eigenschaften, die NICHT übersetzt werden sollen
3. Referenzierte Inhalte, die übersetzt werden sollen (d. h. Bilder oder Inhaltsfragmente)

Der Übersetzungsregel-Editor, der die XML-Übersetzungsdatei aktualisiert. Die Benutzeroberfläche für die Übersetzungskonfiguration erleichtert die Verwaltung verschiedener Übersetzungsregeln und schützt vor Typos bei der direkten Bearbeitung von XML.

Zugriff auf die Benutzeroberfläche der Übersetzungskonfiguration:

* **[!UICONTROL AEM Startmenü]  >  [!UICONTROL Tools]  >  [!UICONTROL Allgemein]  >  [[!UICONTROL Übersetzungskonfiguration]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## Vor AEM 6.3 {#prior-to-aem}

In früheren AEM wurden die Übersetzungsregeln der Version manuell aktualisiert, indem eine XML-Datei bearbeitet wurde, die sich im Übersetzungs-Workflow befindet: `/etc/workflow/models/translation/translation_rules.xml`.

## Zusätzliche Ressourcen {#additional-resources}

* [Identifizieren zu übersetzender Inhalte](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [Übersetzen von Inhalten für mehrsprachige Sites](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [Best Practices für die Übersetzung](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)
