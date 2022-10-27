---
title: Die nächsten und vorherigen Schaltflächen der Symbolleiste platzieren
description: Platzieren Sie die Schaltflächen der Symbolleiste
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9291
exl-id: 1b55b6d2-3bab-4907-af89-c81a3b1a44cb
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 11%

---

# Platzieren Sie die Symbolleistenschaltfläche

Wenn Sie der Symbolleiste in AEM Forms die Schaltflächen Weiter und Zurück hinzufügen, werden die Schaltflächen standardmäßig nebeneinander angeordnet. Sie können die Schaltfläche Weiter in der Symbolleiste ganz rechts nach links drücken, während Sie die Schaltfläche Zurück/Vorwärts links beibehalten

![toolbar-spacing](assets/toolbar-spacing.png)


## Symbolleiste formatieren

Der obige Anwendungsfall kann mithilfe des Stileditors einfach durchgeführt werden. Nachdem Sie die Schaltfläche &quot;Zurück/Weiter&quot;zur Symbolleiste hinzugefügt haben, stellen Sie sicher, dass Sie die Ebene &quot;Stil&quot;im Bearbeitungsmenü ausgewählt haben. Wählen Sie bei ausgewähltem Stilmodus die Symbolleiste aus, um das Styling-Eigenschaftenblatt zu öffnen. Erweitern Sie den Abschnitt Dimensionen und Position und stellen Sie sicher, dass alle Eigenschaften angezeigt werden. Legen Sie die folgenden Eigenschaften fest
* Abmessungen und Position
   * Breite: 100%
   * Funktion: relative

Speichern Sie Ihre Änderungen

## Stil der Schaltfläche &quot;Weiter&quot;

Wählen Sie die Schaltfläche Weiter aus und stellen Sie sicher, dass Sie das Stylesheet der nächsten Schaltfläche öffnen (nicht den nächsten Schaltflächentext). Legen Sie die folgenden Eigenschaften fest
* Abmessungen und Position
   * position: Absoluter 1 px, rechts 1px
* Rahmen
   * Rahmenradius: 4px(Oben, Rechts, Unten, Links)

Speichern Sie Ihre Änderungen
