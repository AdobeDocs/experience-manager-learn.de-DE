---
title: Erstellen von Dokument-Fragment
description: 'Dies ist Teil 5 eines mehrstufigen Lernprogramms zur Erstellung Ihres ersten interaktiven Kommunikations-Dokuments. In diesem Teil erstellen wir ein Dokument-Fragment, das den Namen und die Adresse des Empfängers enthält. '
seo-description: 'Dies ist Teil 5 eines mehrstufigen Lernprogramms zur Erstellung Ihres ersten interaktiven Kommunikations-Dokuments. In diesem Teil erstellen wir ein Dokument-Fragment, das den Namen und die Adresse des Empfängers enthält. '
uuid: 7fd8a0f2-a921-4e70-91c9-908dae9aeab2
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 47d3aa97-0bff-48e0-8a65-55e5332f811b
kt: 5958
thumbnail: 22350.jpg
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 0%

---


# Erstellen von Dokument-Fragment

In diesem Teil erstellen wir ein Dokument-Fragment, das den Namen und die Adresse des Empfängers enthält.

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

Dokument-Fragmente enthalten den Textinhalt von interaktiven Dokumenten. Dieser Textinhalt kann statischer Text sein oder aus den Werten der zugrunde liegenden Datenmodellelemente eingefügt werden. Beispiel: **Sehr geehrte _{name}_**, wobei Liebe statischer Text und Name der Elementname des Formulardatenmodells ist. Zur Laufzeit wird dies auf **Sehr geehrte Gloria Rios**oder **Sehr geehrte John Jacobs**je nach Wert des name-Elements aufgelöst.

Rich-Text-Editor ist intuitiv genug, damit ein Geschäftsbenutzer Text verfassen und Formulardaten-Elemente einfügen kann. Der Dokument-Fragment-Editor kann Text formatieren, Schriftarten und Stile angeben, Sonderzeichen einfügen und Hyperlinks erstellen.

Der Dokument-Fragment-Editor hat auch die Möglichkeit, Inline-Bedingungen in Ihren Text einzufügen, wie in diesem [Video](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>Stellen Sie sicher, dass die Formulardatenmodellelemente, die Sie in ein Dokument-Fragment einfügen, untergeordnete Elemente des Stammelements sind. In diesem Verwendungsfall müssen Sie beispielsweise sicherstellen, dass die Elemente des Benutzerobjekts, die Sie auswählen, das untergeordnete Element des gleichmäßigen Objekts sind

