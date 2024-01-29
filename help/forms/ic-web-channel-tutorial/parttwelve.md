---
title: Einrichten der Bereitstellung von Web-Kanaldokumenten
description: Dies ist der letzte Teil eines mehrstufigen Tutorials zum Erstellen Ihres ersten Dokuments zur interaktiven Kommunikation. In diesem Teil beschäftigen wir uns mit dem Versand eines Web-Kanaldokuments per E-Mail.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 1a7cf095-c5d8-4d92-a018-883cda76fe70
topic: Development
role: Developer
level: Beginner
exl-id: 510d1782-59b9-41a6-a071-a16170f2cd06
duration: 90
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '351'
ht-degree: 100%

---

# Einrichten der Bereitstellung von Web-Kanaldokumenten {#setting-up-the-delivery-of-web-channel-document}


In diesem Teil beschäftigen wir uns mit dem Versand eines Web-Kanaldokuments per E-Mail.

Nachdem Sie Ihr Web-Kanaldokument zur interaktiven Kommunikation definiert und getestet haben, benötigen Sie einen Versandmechanismus, um das Web-Kanaldokument an die Empfängerin oder den Empfänger zu senden.

Um E-Mails als Versandmechanismus für unser Web-Kanaldokument verwenden zu können, müssen wir eine geringfügige Änderung am Formulardatenmodell vornehmen.

[Weitere Informationen zum Web-Kanalversand per E-Mail](/help/forms/interactive-communications/delivery-of-web-channel-document-tutorial-use.md)

Melden Sie sich bei AEM Forms an.

* Navigieren Sie zu Formulare > Datenintegrationen

* Öffnen Sie das Datenmodell „RetirementAccountStatement“ im Bearbeitungsmodus.

* Wählen Sie das Saldo-Objekt aus und klicken Sie auf die Schaltfläche „Bearbeiten“.

* Wählen Sie das Stiftsymbol aus, um das id-Argument im Bearbeitungsmodus zu öffnen.

* Ändern Sie die Bindung zu „RequestAttribute“.

* Legen Sie „accountnumber“ (also die Kontonummer) unter „Bindungswert“ fest, wie unten dargestellt.

* Auf diese Weise übergeben wir die Kontonummer über das Anfrageattribut an das Formulardatenmodell.

* Denken Sie daran, Ihre Änderungen zu speichern.
  ![FDM](assets/requestattribute.gif)

## Testen des E-Mail-Versands des Web-Kanaldokuments {#test-email-delivery-of-web-channel-document}

* [Installieren Sie die Beispiel-Assets mit Package Manager.](assets/webchanneldelivery.zip)
* [Melden Sie sich bei crx an.](http://localhost:4502/crx/de/index.jsp#)

* Navigieren Sie zu „/home/users“.

* Suchen Sie nach der Admin-Benutzerin bzw. dem Admin-Benutzer unter dem Knoten der Benutzerin bzw. des Benutzers.

* Wählen Sie den Profilknoten der Admin-Benutzerin oder des Admin-Benutzers aus.

* Erstellen Sie eine Eigenschaft „accountnumber“. Stellen Sie sicher, dass der Eigenschaftstyp eine Zeichenfolge ist.

* Legen Sie für diese accountnumber-Eigenschaft den Wert „3059827“ fest. Sie können diesen Wert beliebig auf eine zufällige Zahl einstellen.

* [Öffnen Sie getad.html.](http://localhost:4502/content/getad.html)

* Der mit dieser URL verknüpfte Code erhält die Kontonummer der angemeldeten Benutzerin bzw. des angemeldeten Benutzers. Diese Kontonummer wird dann als Anfrageattribut an das FDM weitergegeben. Das FDM ruft dann die mit dieser Kontonummer verknüpften Daten ab und füllt das Web-Kanaldokument auf.

>[!NOTE]
>
>Sehen Sie sich die Datei **/apps/AEMForms/fetchad/GET.jsp** in crx an. Vergewissern Sie sich, dass die Zeichenfolgenvariable „webChannelDocument“ auf einen gültigen Pfad für Kommunikationsdokumente verweist.

## Nächste Schritte

[Einrichten des E-Mail-Versands](../interactive-communications/delivery-of-web-channel-document-tutorial-use.md)