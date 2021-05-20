---
title: Erstellen von Dokumentfragmenten
description: 'Dies ist Teil 5 eines mehrstufigen Tutorials zum Erstellen Ihres ersten interaktiven Kommunikationsdokuments. In diesem Teil erstellen wir ein Dokumentfragment, das den Namen und die Adresse des Empfängers enthält. '
seo-description: 'Dies ist Teil 5 eines mehrstufigen Tutorials zum Erstellen Ihres ersten interaktiven Kommunikationsdokuments. In diesem Teil erstellen wir ein Dokumentfragment, das den Namen und die Adresse des Empfängers enthält. '
uuid: 7fd8a0f2-a921-4e70-91c9-908dae9aeab2
feature: Interaktive Kommunikation
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 47d3aa97-0bff-48e0-8a65-55e5332f811b
kt: 5958
thumbnail: 22350.jpg
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 1%

---


# Erstellen von Dokumentfragmenten

In diesem Teil erstellen wir ein Dokumentfragment, das den Namen und die Adresse des Empfängers enthält.

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

Dokumentfragmente enthalten den Textinhalt interaktiver Kommunikationsdokumente. Dieser Textinhalt kann statischer Text sein oder aus den zugrunde liegenden Datenmodell-Elementwerten eingefügt werden. Beispiel: **Lieber _{name}_**, wobei &quot;Liebe&quot;statischer Text und &quot;Name&quot;der Elementname des Formulardatenmodells ist. Zur Laufzeit wird dies in **Lieber Gloria Rios**oder **Lieber John Jacobs**aufgelöst, je nach Wert des name-Elements.

Der Rich-Text-Editor ist intuitiv genug, damit ein Geschäftsbenutzer Text erstellen und Formulardaten-Elemente einfügen kann. Der Dokumentfragment-Editor bietet die Möglichkeit, Text zu formatieren, Schriftarten und -stile anzugeben, Sonderzeichen einzufügen und Hyperlinks zu erstellen.

Der Dokumentfragment-Editor kann auch Inline-Bedingungen in Ihren Text einfügen, wie in diesem [Video](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>Stellen Sie sicher, dass die Formulardatenmodellelemente, die Sie in Dokumentfragmente einfügen, untergeordnete Elemente des Stammelements sind. Stellen Sie beispielsweise in diesem Anwendungsfall sicher, dass die von Ihnen ausgewählten Elemente des User-Objekts das untergeordnete Element des Balanceobjekts sind.

