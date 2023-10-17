---
title: Erstellen von Dokumentfragmenten, die Namen und Adresse der Empfängerin bzw. des Empfängers enthalten
seo-title: Creating Document Fragments to hold the recipient name and address
description: Dies ist Teil 5 eines mehrstufigen Tutorials zum Erstellen Ihres ersten interaktiven Kommunikationsdokuments. In diesem Teil erstellen wir ein Dokumentfragment, das den Namen und die Adresse der Empfängerin bzw. des Empfängers enthält.
seo-description: This is part 5 of a multi-step tutorial for creating your first interactive communications document. In this part, we will create document fragment to hold the recipient name and address.
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
exl-id: 1d7093a8-3765-46ec-912a-b5a5503fd5af
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: ht
source-wordcount: '244'
ht-degree: 100%

---

# Erstellen von Dokumentfragmenten, die Namen und Adresse der Empfängerin bzw. des Empfängers enthalten {#creating-document-fragments-to-hold-the-recipient-name-and-address}

In diesem Teil erstellen wir ein Dokumentfragment, das den Namen und die Adresse der Empfängerin bzw. des Empfängers enthält.

>[!VIDEO](https://video.tv.adobe.com/v/22350?quality=12&learn=on)

Dokumentfragmente enthalten den Textinhalt interaktiver Kommunikationsdokumente. Dieser Textinhalt kann statischer Text sein oder aus den zugrunde liegenden Datenmodell-Elementwerten eingefügt werden. Beispiel: Liebe/Lieber {name}, wobei „Liebe/Lieber“ statischer Text und {name} der Name des Formulardaten-Elements ist. Zur Laufzeit wird dies je nach dem Wert des name-Elements in „Liebe Beate Bauer“ oder „Lieber Stefan Müller“ aufgelöst.

Der Rich-Text-Editor ist intuitiv genug, damit Business-Anwenderinnen und -Anwender Text erstellen und Formulardaten-Elemente einfügen können. Der Dokumentfragmenteditor bietet die Möglichkeit, Text zu formatieren, Schriftarten und -stile anzugeben, Sonderzeichen einzufügen und Hyperlinks zu erstellen.

Der Dokumentfragmenteditor kann auch Inline-Bedingungen in Ihren Text einfügen, wie in diesem [Video](https://helpx.adobe.com/de/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html) zu sehen.

>[!NOTE]
>
>Stellen Sie sicher, dass die Formulardatenmodellelemente, die Sie in Dokumentfragmente einfügen, untergeordnete Elemente des Stammelements sind. Stellen Sie beispielsweise in diesem Anwendungsfall sicher, dass die von Ihnen ausgewählten Elemente des Benutzerobjekts das untergeordnete Element des Balances-Objekts sind.

## Nächste Schritte

[Erstellen eines interaktiven Kommunikationsdokuments](./partsix.md)