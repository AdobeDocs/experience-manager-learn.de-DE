---
title: Festlegen der Abstände zwischen den Schaltflächen „Weiter“ und „Zurück“ der Symbolleiste
description: Festlegen der Abstände zwischen den Schaltflächen in der Symbolleiste
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9291
exl-id: 1b55b6d2-3bab-4907-af89-c81a3b1a44cb
last-substantial-update: 2019-07-07T00:00:00Z
duration: 59
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 100%

---

# Festlegen des Abstands der Schaltfläche in der Symbolleiste

Wenn Sie der Symbolleiste in AEM Forms die Schaltflächen „Weiter“ und „Zurück“ hinzufügen, werden die Schaltflächen standardmäßig direkt nebeneinander angeordnet. Sie können die Schaltfläche „Weiter“ in der Symbolleiste ganz nach rechts schieben, während Sie die Schaltfläche „Zurück“ links beibehalten

![toolbar-spacing](assets/toolbar-spacing.png)


## Festlegen des Stils der Symbolleiste

Der obige Anwendungsfall kann mithilfe des Stileditors einfach durchgeführt werden. Nachdem Sie die Schaltfläche „Zurück“/„Weiter“ zur Symbolleiste hinzugefügt haben, stellen Sie sicher, dass Sie die Stilebene im Bearbeitungsmenü ausgewählt haben. Wählen Sie bei ausgewähltem Stilmodus die Symbolleiste aus, um das Stileigenschaftenblatt zu öffnen. Erweitern Sie den Abschnitt „Dimensionen und Position“ und stellen Sie sicher, dass alle Eigenschaften angezeigt werden. Legen Sie die folgenden Eigenschaften fest
* Dimensionen und Position
   * Breite: 100 %
   * Position: relativ

Speichern Sie Ihre Änderungen

## Festlegen des Stils der Schaltfläche „Weiter“

Wählen Sie die Schaltfläche „Weiter“ aus und stellen Sie sicher, dass Sie das Stileigenschaftenblatt der nächsten Schaltfläche öffnen (nicht den Text der nächsten Schaltfläche). Legen Sie die folgenden Eigenschaften fest
* Dimensionen und Position
   * Position: absolut oben 1 px, rechts 1 px
* Rahmen
   * Rahmenradius: 4 px (oben, rechts, unten, links)

Speichern Sie Ihre Änderungen
