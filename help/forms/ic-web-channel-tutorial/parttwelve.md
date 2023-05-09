---
title: Bereitstellung des Webkanaldokuments einrichten
seo-title: Setting up the delivery of web channel document
description: Dies ist der letzte Teil eines mehrstufigen Tutorials zum Erstellen Ihres ersten interaktiven Kommunikationsdokuments. In diesem Teil sehen wir uns den Versand des Webkanaldokuments per E-Mail an.
seo-description: This is the final part of a multistep tutorial for creating your first interactive communications document. In this part, we look at the delivery of web channel document via email.
uuid: c1066600-1abd-4401-b04f-b93c28603cc7
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 1a7cf095-c5d8-4d92-a018-883cda76fe70
topic: Development
role: Developer
level: Beginner
exl-id: 510d1782-59b9-41a6-a071-a16170f2cd06
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 1%

---

# Bereitstellung des Webkanaldokuments einrichten {#setting-up-the-delivery-of-web-channel-document}


In diesem Teil sehen wir uns den Versand des Webkanaldokuments per E-Mail an.

Nachdem Sie Ihr Dokument zur interaktiven Kommunikation mit Webkanälen definiert und getestet haben, benötigen Sie einen Bereitstellungsmechanismus, um das Webkanaldokument an den Empfänger zu senden.

Um E-Mails als Bereitstellungsmechanismus für unser Webkanaldokument verwenden zu können, müssen wir eine geringfügige Änderung am Formulardatenmodell vornehmen.

[Weitere Informationen zum Webkanal-Versand per E-Mail](/help/forms/interactive-communications/delivery-of-web-channel-document-tutorial-use.md)

Melden Sie sich bei AEM Forms an.

* Navigieren Sie zu Forms > Datenintegrationen .

* Öffnen Sie das Datenmodell RetirementAccountStatement im Bearbeitungsmodus.

* Wählen Sie das Bildobjekt aus und klicken Sie auf die Schaltfläche &quot;Bearbeiten&quot;.

* Wählen Sie das Stiftsymbol aus, um das id-Argument im Bearbeitungsmodus zu öffnen.

* Ändern Sie die Bindung in &quot;RequestAttribute&quot;.

* Legen Sie die Kontonummer im Bindungswert wie unten dargestellt fest.

* Auf diese Weise übergeben wir die Kontonummer über das Anfrageattribut an das Formulardatenmodell.

* Achten Sie darauf, Ihre Änderungen zu speichern.
   ![fdm](assets/requestattribute.gif)

## Testen der E-Mail-Bereitstellung des Webkanal-Dokuments {#test-email-delivery-of-web-channel-document}

* [Installieren von Beispiel-Assets mit Package Manager](assets/webchanneldelivery.zip)
* [Bei crx anmelden](http://localhost:4502/crx/de/index.jsp#)

* Navigieren Sie zu /home/users

* Suchen Sie nach Admin-Benutzer unter dem Knoten des Benutzers.

* Wählen Sie den Profilknoten des Admin-Benutzers aus.

* Erstellen Sie eine Eigenschaft mit der Bezeichnung &quot;Kundennummer&quot;. Stellen Sie sicher, dass der Eigenschaftstyp eine Zeichenfolge ist.

* Setzen Sie den Wert dieser Eigenschaft der Kundennummer auf &quot;3059827&quot;. Sie können diesen Wert beliebig auf eine zufällige Zahl einstellen.

* [Öffnen Sie getad.html](http://localhost:4502/content/getad.html)

* Der mit dieser URL verknüpfte Code erhält die Kontonummer des angemeldeten Benutzers. Diese Kontonummer wird dann als Anforderungsattribut an den FDM übergeben. Der FDM ruft dann die mit dieser Kontonummer verknüpften Daten ab und füllt das Webkanaldokument aus.

>[!NOTE]
>
>Bitte schauen Sie sich die **/apps/AEMForms/fetchad/GET.jsp** Datei in crx. Vergewissern Sie sich, dass die String-Variable webChannelDocument auf einen gültigen Pfad für Kommunikationsdokumente verweist.

## Nächste Schritte

[E-Mail-Versand einrichten](../interactive-communications/delivery-of-web-channel-document-tutorial-use.md)