---
title: Einrichten des Versands von Web Kanal Dokument
seo-title: Einrichten des Versands von Web Kanal Dokument
description: Dies ist der letzte Teil eines mehrstufigen Tutorials zur Erstellung Ihres ersten interaktiven Kommunikations-Dokuments. In diesem Teil betrachten wir den Versand des Web-Kanal-Dokuments per E-Mail.
seo-description: Dies ist der letzte Teil eines mehrstufigen Tutorials zur Erstellung Ihres ersten interaktiven Kommunikations-Dokuments. In diesem Teil betrachten wir den Versand des Web-Kanal-Dokuments per E-Mail.
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
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 2%

---


# Einrichten des Versands des Web-Kanal-Dokuments {#setting-up-the-delivery-of-web-channel-document}


In diesem Teil betrachten wir den Versand des Web-Kanal-Dokuments per E-Mail.

Nachdem Sie Ihr interaktives Dokument für die interaktive Kommunikation mit dem Web Kanal definiert und getestet haben, benötigen Sie einen Versand-Mechanismus, um das Web-Kanal-Dokument an den Empfänger zu senden.

Um E-Mail als Versand für unser Web-Kanal-Dokument verwenden zu können, müssen wir eine geringfügige Änderung am Formulardatenmodell vornehmen.

[Weitere Informationen zum Web Kanal Versand per E-Mail](/help/forms/interactive-communications/delivery-of-web-channel-document-tutorial-use.md)

Melden Sie sich bei AEM Forms an.

* Navigieren Sie zu Forms ->Datenintegrationen

* Öffnen Sie das RetirementAccountStatement-Datenmodell im Bearbeitungsmodus.

* Wählen Sie das Bildobjekt aus und klicken Sie auf die Schaltfläche &quot;Bearbeiten&quot;.

* Wählen Sie das Symbol &quot;Stift&quot;, um das ID-Argument im Bearbeitungsmodus zu öffnen.

* Ändern Sie die Bindung in &quot;RequestAttribute&quot;.

* Legen Sie die Kontonummer im Bindungswert wie unten gezeigt fest.

* Auf diese Weise übergeben wir die Kontonummer über das Anforderungsattribut an das Formulardatenmodell

* Achten Sie darauf, Ihre Änderungen zu speichern.
   ![fdm](assets/requestattribute.gif)

## E-Mail-Versand des Web-Kanal-Dokuments {#test-email-delivery-of-web-channel-document} testen

* [Beispielelemente mit Package Manager installieren](assets/webchanneldelivery.zip)
* [Bei CRX anmelden](http://localhost:4502/crx/de/index.jsp#)

* Navigieren Sie zu /home/users

* Suchen Sie nach Admin-Benutzer unter dem Knoten des Benutzers.

* Wählen Sie den Profil-Knoten des Admin-Benutzers aus.

* Erstellen Sie eine Eigenschaft mit dem Namen &quot;accountNumber&quot;. Vergewissern Sie sich, dass der Eigenschaftstyp eine Zeichenfolge ist.

* Legen Sie den Wert dieser Eigenschaft der Kontonummer auf &quot;3059827&quot;fest. Sie können diesen Wert auf eine beliebige Zufallszahl einstellen, wie Sie möchten.

* [Öffnen Sie getad.html](http://localhost:4502/content/getad.html)

* Der mit dieser URL verknüpfte Code erhält die Kontonummer des angemeldeten Benutzers. Diese Kontonummer wird dann als Anforderungsattribut an den FDM übergeben. Der FDM ruft dann die mit dieser Kontonummer verknüpften Daten ab und füllt das Web-Kanal-Dokument.

>[!NOTE]
>
>Bitte schauen Sie sich die Datei **/apps/AEMForms/fetchad/GET.jsp** in crx an. Vergewissern Sie sich, dass die String-Variable webChannelDocument auf einen gültigen Kommunikations-Dokument-Pfad verweist.
