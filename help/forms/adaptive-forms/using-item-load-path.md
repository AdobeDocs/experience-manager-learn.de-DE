---
title: Verwenden des Ladepfads für Elemente zum Ausfüllen der Dropdown-Liste
description: Konfigurieren und Ausfüllen einer Dropdown-Liste zum Lesen von Werten aus einem CRX-Knoten
feature: Adaptive Forms
version: 6.4,6.5
kt: 10961
topic: Development
role: Developer
level: Beginner
source-git-commit: 614db8b03a823b60846ab8ccfa8fbc29a41f7791
workflow-type: tm+mt
source-wordcount: '175'
ht-degree: 0%

---

# Eigenschaft &quot;Elementladevorgang&quot;in AEM Forms

Konfigurieren und füllen Sie die Dropdown-Liste mit der Eigenschaft &quot;item load path&quot;.
Das Feld &quot;Element-Ladepfad&quot;ermöglicht es einem Autor, eine URL bereitzustellen, aus der die in einer Dropdown-Liste verfügbaren Optionen geladen werden.
Um einen solchen Knoten in crx zu erstellen, führen Sie die folgenden Schritte aus

* Bei crx anmelden
* Erstellen Sie einen Knoten mit dem Namen assets (Sie können diesen Knoten gemäß Ihren Anforderungen benennen) und geben Sie sling:folder unter content ein.
* Speichern
* Klicken Sie auf den neu erstellten Asset-Knoten und legen Sie seine Eigenschaften wie unten gezeigt fest.
* Sie müssen eine Eigenschaft des Typs String mit dem Namen Assettypes erstellen (Sie können sie gemäß Ihren Anforderungen benennen). Geben Sie die gewünschten Werte an und speichern Sie sie.

![item-load-path](assets/item-load-path-crx.png)

Um diese Werte in Ihre Dropdown-Liste zu laden, geben Sie den folgenden Pfad in die Eigenschaft &quot;item load path&quot;ein  **/content/assets/assettypes**

Das Beispielpaket kann [heruntergeladen von hier](assets/item-load-path-package.zip)
