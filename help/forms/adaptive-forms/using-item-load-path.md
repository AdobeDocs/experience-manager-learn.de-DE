---
title: Verwenden des Element-Ladepfads zum Füllen einer Dropdown-Liste
description: Konfigurieren und Füllen einer Dropdown-Liste zum Lesen von Werten aus einem CRX-Knoten
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-10961
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-20T00:00:00Z
thumbnail: item-load.jpg
exl-id: 89c486c8-95c3-4cd4-bf8e-a1b3558f17d6
duration: 34
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '180'
ht-degree: 100%

---

# Element-Ladeeigenschaft in AEM Forms

Konfigurieren und füllen Sie eine Dropdown-Liste mit der Eigenschaft „Element-Ladepfad“.
Das Feld „Element-Ladepfad“ ermöglicht es einer Autorin oder einem Autor, eine URL bereitzustellen, aus der die in einer Dropdown-Liste verfügbaren Optionen geladen werden.
Gehen Sie wie folgt vor, um einen solchen Knoten in crx zu erstellen:
* Melden Sie sich bei crx an.
* Erstellen Sie einen Knoten mit dem Namen „assets“ (Sie können diesen Knoten gemäß Ihren Anforderungen benennen) und geben Sie „sling:folder“ unter „content“ ein.
* Speichern Sie.
* Klicken Sie auf den neu erstellten Asset-Knoten und legen Sie seine Eigenschaften fest, wie unten dargestellt.
* Sie müssen eine Eigenschaft vom Typ „Zeichenfolge“ mit dem Namen „assettypes“ erstellen (Sie können diesen gemäß Ihren Anforderungen wählen). Stellen Sie sicher, dass die Eigenschaft mehrwertig ist. Geben Sie die gewünschten Werte an und speichern Sie diese.
  ![item-load-path](assets/item-load-path-crx.png)

Um diese Werte in Ihre Dropdown-Liste zu laden, geben Sie den folgenden Pfad in der Eigenschaft „Element-Ladepfad“: **/content/assets/assettypes** an.

Das Beispielpaket kann [hier heruntergeladen werden](assets/item-load-path-package.zip).
