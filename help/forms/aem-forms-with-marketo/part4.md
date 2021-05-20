---
title: AEM Forms mit Marketo (Teil 4)
seo-title: AEM Forms mit Marketo (Teil 4)
description: Tutorial zur Integration von AEM Forms mit Marketo mithilfe des AEM Forms-Formulardatenmodells.
seo-description: Tutorial zur Integration von AEM Forms mit Marketo mithilfe des AEM Forms-Formulardatenmodells.
feature: Adaptives Forms, Formulardatenmodell
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---


# Erstellen eines adaptiven Formulars mit dem Formulardatenmodell

Der nächste Schritt besteht darin, ein adaptives Formular zu erstellen und es auf dem Formulardatenmodell zu basieren, das im vorherigen Schritt erstellt wurde.
Der Benutzer gibt die Lead-ID ein und beim Abmelden des Marketo-Dienstes, um die Leads nach ID zu erhalten, wird aufgerufen. Die Ergebnisse des Dienstvorgangs werden dann den entsprechenden Feldern des adaptiven Forms zugeordnet.

1. Erstellen Sie ein adaptives Formular und verknüpfen Sie es auf der Grundlage von &quot;Leere Formularvorlage&quot;mit dem Formulardatenmodell, das Sie im vorherigen Schritt erstellt haben.
1. Öffnen Sie das Formular im Bearbeitungsmodus
1. Ziehen Sie eine TextField -Komponente und eine Bedienfeldkomponente per Drag-and-Drop in das adaptive Formular. Legen Sie den Titel der TextField-Komponente &quot;Enter Lead Id&quot;fest und setzen Sie ihren Namen auf &quot;LeadId&quot;
1. Ziehen Sie zwei TextField-Komponenten per Drag-and-Drop in die Bedienfeldkomponente
1. Legen Sie den Namen und Titel der 2 Textfield-Komponenten auf Vorname und Nachname fest.
1. Konfigurieren Sie die Bedienfeldkomponente so, dass sie eine wiederholbare Komponente ist, indem Sie den Wert Minimum auf 1 und Maximum auf -1 setzen. Dies ist erforderlich, da der Marketo-Dienst ein Array von Lead-Objekten zurückgibt und Sie eine wiederholbare Komponente benötigen, um die Ergebnisse anzuzeigen. In diesem Fall erhalten wir jedoch nur ein Lead-Objekt zurück, da wir nach Lead-Objekten anhand ihrer Kennung suchen.
1. Erstellen Sie eine Regel für das Feld LeadId , wie in der Abbildung unten dargestellt
1. Zeigen Sie eine Vorschau des Formulars an und geben Sie eine gültige Lead-ID in das Feld LeadID ein. Die Felder Vorname und Nachname sollten mit den Ergebnissen des Dienstaufrufs gefüllt werden.

Im folgenden Screenshot werden die Einstellungen des Regeleditors erläutert

![ruleeditor](assets/ruleeditor.jfif)
