---
title: Erstellen von Dokument-Fragmenten, die den Namen und die Adresse des Empfängers enthalten
seo-title: Erstellen von Dokument-Fragmenten, die den Namen und die Adresse des Empfängers enthalten
description: 'Dies ist Teil 5 eines mehrstufigen Lernprogramms zur Erstellung Ihres ersten interaktiven Kommunikations-Dokuments. In diesem Teil erstellen wir ein Dokument-Fragment, das den Namen und die Adresse des Empfängers enthält. '
seo-description: 'Dies ist Teil 5 eines mehrstufigen Lernprogramms zur Erstellung Ihres ersten interaktiven Kommunikations-Dokuments. In diesem Teil erstellen wir ein Dokument-Fragment, das den Namen und die Adresse des Empfängers enthält. '
uuid: 689931e4-a026-4e62-9acd-552918180819
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 404eed65-ec55-492a-85b5-59773896b217
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 1%

---


# Erstellen von Dokument-Fragmenten, die den Empfänger und die Adresse {#creating-document-fragments-to-hold-the-recipient-name-and-address} enthalten

In diesem Teil erstellen wir ein Dokument-Fragment, das den Namen und die Adresse des Empfängers enthält.

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

Dokument-Fragmente enthalten den Textinhalt von interaktiven Dokumenten. Dieser Textinhalt kann statischer Text sein oder aus den Werten der zugrunde liegenden Datenmodellelemente eingefügt werden. Beispiel: Sehr geehrte {Name}, bei der Liebe statischer Text und {Name} der Name des Formulardaten-Elements ist. Zur Laufzeit wird dies in Abhängigkeit vom Wert des name-Elements auf &quot;Lieber Gloria Rios&quot;oder &quot;Lieber John Jacobs&quot;aufgelöst.

Der Rich-Text-Editor ist intuitiv genug, damit ein Geschäftsbenutzer Text verfassen und Formulardaten-Elemente einfügen kann. Der Dokument-Fragment-Editor kann Text formatieren, Schriftarten und Stile angeben, Sonderzeichen einfügen und Hyperlinks erstellen.

Der Dokument-Fragment-Editor hat auch die Möglichkeit, Inline-Bedingungen in Ihren Text einzufügen, wie in diesem [Video](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>Stellen Sie sicher, dass die Formulardatenmodellelemente, die Sie in ein Dokument-Fragment einfügen, untergeordnete Elemente des Stammelements sind. In diesem Verwendungsfall müssen Sie beispielsweise sicherstellen, dass die Elemente des Benutzerobjekts, die Sie auswählen, das untergeordnete Element des gleichmäßigen Objekts sind

