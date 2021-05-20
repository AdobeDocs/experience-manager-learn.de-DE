---
title: Erstellen von Dokumentfragmenten für den Empfängernamen und die Adresse
seo-title: Erstellen von Dokumentfragmenten für den Empfängernamen und die Adresse
description: 'Dies ist Teil 5 eines mehrstufigen Tutorials zum Erstellen Ihres ersten interaktiven Kommunikationsdokuments. In diesem Teil erstellen wir ein Dokumentfragment, das den Namen und die Adresse des Empfängers enthält. '
seo-description: 'Dies ist Teil 5 eines mehrstufigen Tutorials zum Erstellen Ihres ersten interaktiven Kommunikationsdokuments. In diesem Teil erstellen wir ein Dokumentfragment, das den Namen und die Adresse des Empfängers enthält. '
uuid: 689931e4-a026-4e62-9acd-552918180819
feature: Interaktive Kommunikation
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 404eed65-ec55-492a-85b5-59773896b217
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 1%

---


# Erstellen von Dokumentfragmenten für den Empfängernamen und die Adresse {#creating-document-fragments-to-hold-the-recipient-name-and-address}

In diesem Teil erstellen wir ein Dokumentfragment, das den Namen und die Adresse des Empfängers enthält.

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

Dokumentfragmente enthalten den Textinhalt interaktiver Kommunikationsdokumente. Dieser Textinhalt kann statischer Text sein oder aus den zugrunde liegenden Datenmodell-Elementwerten eingefügt werden. Beispiel: Lieber {name}, wobei &quot;Lieber&quot;statischer Text und {name} der Name des Formulardaten-Elements ist. Zur Laufzeit wird dies abhängig vom Wert des name -Elements auf &quot;Lieber Gloria Rios&quot;oder &quot;Lieber John Jacobs&quot;aufgelöst.

Der Rich-Text-Editor ist intuitiv genug, damit ein Geschäftsbenutzer Text erstellen und Formulardaten-Elemente einfügen kann. Der Dokumentfragment-Editor bietet die Möglichkeit, Text zu formatieren, Schriftarten und -stile anzugeben, Sonderzeichen einzufügen und Hyperlinks zu erstellen.

Der Dokumentfragmente-Editor kann auch Inline-Bedingungen in Ihren Text einfügen, wie in diesem [Video](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>Stellen Sie sicher, dass die Formulardatenmodellelemente, die Sie in Dokumentfragmente einfügen, untergeordnete Elemente des Stammelements sind. Stellen Sie beispielsweise in diesem Anwendungsfall sicher, dass die von Ihnen ausgewählten Elemente des User-Objekts das untergeordnete Element des Balanceobjekts sind.

