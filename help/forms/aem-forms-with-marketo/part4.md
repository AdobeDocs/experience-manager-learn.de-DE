---
title: AEM Forms mit Marketo (Teil 4)
seo-title: AEM Forms mit Marketo (Teil 4)
description: Lernprogramm zur Integration von AEM Forms in Marketing mit dem AEM Forms-Formulardatenmodell.
seo-description: Lernprogramm zur Integration von AEM Forms in Marketing mit dem AEM Forms-Formulardatenmodell.
feature: Adaptive Forms, Form Data Model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---


# Erstellen eines adaptiven Formulars mit dem Formulardatenmodell

Der nächste Schritt besteht darin, ein adaptives Formular zu erstellen und es auf dem im vorherigen Schritt erstellten Formulardatenmodell zu basieren.
Der Benutzer gibt die Interessenten-ID ein und beim Abmelden des Dienstes &quot;Marketo&quot;werden die Interessenten nach der ID aufgerufen. Die Ergebnisse des Dienstvorgangs werden dann den entsprechenden Feldern des adaptiven Forms zugeordnet.

1. Erstellen Sie ein adaptives Formular und basieren Sie auf &quot;Leere Formularvorlage&quot;. Verknüpfen Sie es mit dem im vorherigen Schritt erstellten Formulardatenmodell.
1. Öffnen Sie das Formular im Bearbeitungsmodus
1. Ziehen Sie per Drag &amp; Drop eine TextField-Komponente und eine Panel-Komponente in das adaptive Formular. Legen Sie den Titel der TextField-Komponente &quot;Enter Lead Id&quot;und den Namen auf &quot;LeadId&quot;fest
1. Ziehen Sie 2 TextField-Komponenten auf die Panel-Komponente
1. Legen Sie den Namen und den Titel der 2 TextField-Komponenten auf &quot;Vorname&quot;und &quot;Nachname&quot;fest
1. Konfigurieren Sie die Panel-Komponente so, dass sie eine wiederholbare Komponente ist, indem Sie die Einstellung Minimum auf 1 und Maximum auf -1 festlegen. Dies ist erforderlich, da der Dienst &quot;Marketo&quot;ein Array von Lead-Objekten zurückgibt und Sie eine wiederholbare Komponente benötigen, um die Ergebnisse anzuzeigen. In diesem Fall erhalten wir jedoch nur ein Lead-Objekt zurück, weil wir nach Interessentenobjekten anhand ihrer ID suchen.
1. Erstellen Sie eine Regel für das Feld LeadId, wie in unten stehender Abbildung dargestellt
1. Vorschau des Formulars und geben Sie eine gültige Interessenten-ID in das Feld &quot;LeadID&quot;und in das Feld &quot;Tabulator-ID&quot;ein. Die Felder Vorname und Nachname sollten mit den Ergebnissen des Dienstaufrufs gefüllt werden.

Im folgenden Screenshot werden die Einstellungen des Regeleditors erläutert

![eleditor](assets/ruleeditor.jfif)
