---
title: Erstellen eines Dokumentfragments
description: Dies ist Teil 5 eines mehrstufigen Tutorials zum Erstellen Ihres ersten interaktiven Kommunikationsdokuments. In diesem Teil erstellen wir ein Dokumentfragment, das den Namen und die Adresse der Empfängerin bzw. des Empfängers enthält.
seo-description: This is part 5 of a multi-step tutorial for creating your first interactive communications document. In this part, we will create document fragment to hold the recipient name and address.
uuid: 7fd8a0f2-a921-4e70-91c9-908dae9aeab2
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 47d3aa97-0bff-48e0-8a65-55e5332f811b
jira: KT-5958
thumbnail: 22350.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 2fe3f950-bc2a-4e91-8d91-00438691727a
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 100%

---

# Erstellen eines Dokumentfragments

In diesem Teil erstellen wir ein Dokumentfragment, das den Namen und die Adresse der Empfängerin bzw. des Empfängers enthält.

>[!VIDEO](https://video.tv.adobe.com/v/22350?quality=12&learn=on)

Dokumentfragmente enthalten den Textinhalt interaktiver Kommunikationsdokumente. Dieser Textinhalt kann statischer Text sein oder aus den zugrunde liegenden Datenmodell-Elementwerten eingefügt werden. Zum Beispiel: **Liebe/Lieber _{name}_**, wobei „Liebe/Lieber“ statischer Text und „name“ der Name des Formulardaten-Elements ist. Zur Laufzeit wird dies je nach dem Wert des name-Elements in **Liebe Beate Bauer**oder **Lieber Stefan Müller**aufgelöst.

Der Rich-Text-Editor ist intuitiv genug, damit Business-Anwenderinnen und -Anwender Text erstellen und Formulardaten-Elemente einfügen können. Der Dokumentfragmenteditor bietet die Möglichkeit, Text zu formatieren, Schriftarten und -stile anzugeben, Sonderzeichen einzufügen und Hyperlinks zu erstellen.

Der Dokumentfragmenteditor kann auch Inline-Bedingungen in Ihren Text einfügen, wie in diesem [Video](https://helpx.adobe.com/de/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html) zu sehen.

>[!NOTE]
>
>Stellen Sie sicher, dass die Formulardatenmodellelemente, die Sie in Dokumentfragmente einfügen, untergeordnete Elemente des Stammelements sind. Stellen Sie beispielsweise in diesem Anwendungsfall sicher, dass die von Ihnen ausgewählten Elemente des Benutzerobjekts das untergeordnete Element des Balances-Objekts sind.

## Nächste Schritte

[Erstellen eines Druckkanaldokuments](./create-print-channel-document.md)
