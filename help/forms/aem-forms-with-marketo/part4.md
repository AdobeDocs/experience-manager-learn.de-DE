---
title: AEM Forms mit Marketo (Teil 4)
description: Tutorial zur Integration von AEM Forms in Marketo mithilfe des AEM Forms-Formulardatenmodells.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 6b44e6b2-15f7-45b2-8d21-d47f122c809d
duration: 84
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 100%

---

# Erstellen eines adaptiven Formulars mithilfe des Formulardatenmodells

Der nächste Schritt besteht darin, ein adaptives Formular zu erstellen und am Formulardatenmodell auszurichten, das im vorherigen Schritt erstellt wurde.
Die Benutzenden geben die Lead-ID ein und beim Verlassen des Marketo-Dienstes mithilfe der Tabulatortaste werden die Leads je nach ID abgerufen. Die Ergebnisse des Dienstvorgangs werden dann den entsprechenden Feldern des adaptiven Formulars zugeordnet.

1. Erstellen Sie ein adaptives Formular auf der Grundlage der „Leeren Formularvorlage“ und verknüpfen Sie es mit dem Formulardatenmodell, das Sie im vorherigen Schritt erstellt haben.
1. Öffnen Sie das Formular im Bearbeitungsmodus
1. Ziehen Sie eine TextField-Komponente und eine Bedienfeldkomponente per Drag-and-Drop in das adaptive Formular. Legen Sie den Titel der TextField-Komponente auf „Lead-ID eingeben“ und ihren Namen auf „LeadId“ fest
1. Ziehen Sie zwei TextField-Komponenten per Drag-and-Drop in die Bedienfeldkomponente
1. Legen Sie den Namen und Titel der zwei TextField-Komponenten auf „Vorname“ und „Nachname“ fest.
1. Konfigurieren Sie die Bedienfeldkomponente so, dass sie eine wiederholbare Komponente ist, indem Sie das Minimum auf 1 und das Maximum auf -1 setzen. Dies ist erforderlich, da der Marketo-Dienst ein Array von Lead-Objekten zurückgibt und Sie eine wiederholbare Komponente benötigen, um die Ergebnisse anzuzeigen. In diesem Fall erhalten wir jedoch nur ein Lead-Objekt zurück, da wir nach Lead-Objekten anhand ihrer ID suchen.
1. Erstellen Sie eine Regel für das LeadId-Feld, wie in der Abbildung unten dargestellt
1. Zeigen Sie eine Vorschau des Formulars an, geben Sie eine gültige Lead-ID in das LeadID-Feld ein, und verlassen Sie das Fenster per Tabulatortaste. Die Felder „Vorname“ und „Nachname“ sollten mit den Ergebnissen des Dienstaufrufs gefüllt werden.

Im folgenden Screenshot werden die Einstellungen des Regeleditors erläutert

![Regeleditor](assets/ruleeditor.jfif)

## Debugging

Wenn Sie die in diesem Artikel bereitgestellten Bundles verwenden, sollten Sie evtl. [Debugging-Protokolle](http://localhost:4502/system/console/slinglog) für die folgenden Klassen aktivieren:

+ `com.marketoandforms.core.impl.MarketoServiceImpl`
+ `com.marketoandforms.core.MarketoConfigurationService`

## Herzlichen Glückwunsch!

Sie haben AEM Forms mithilfe des AEM Forms-Formulardatenmodells in Marketo integriert.