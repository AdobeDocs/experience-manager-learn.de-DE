---
title: PrePopulate HTML5 Forms using data attribute.
description: Füllen Sie HTML5-Formulare, indem Sie Daten aus der Backend-Quelle abrufen.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: ab0f5282-383b-4be6-9c57-cded6ab37528
last-substantial-update: 2020-01-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 2%

---

# PrePopulate HTML5 Forms using data attribute {#prepopulate-html-forms-using-data-attribute}


XDP-Vorlagen, die mit AEM Forms im HTML-Format wiedergegeben werden, werden als HTML5 oder Mobile Forms bezeichnet. Ein gängiger Anwendungsfall besteht darin, diese Formulare vorab auszufüllen, wenn sie wiedergegeben werden.

Es gibt 2 Möglichkeiten, Daten mit der XDP-Vorlage zusammenzuführen, wenn sie als HTML gerendert werden.

**dataRef**: Sie können den Parameter dataRef in der URL verwenden. Dieser Parameter gibt den absoluten Pfad der Datendatei an, die mit der Vorlage zusammengeführt wird. Dieser Parameter kann eine URL zu einem REST-Dienst sein, der die Daten im XML-Format zurückgibt.

**data**: Dieser Parameter gibt die UTF-8-kodierten Datenbytes an, die mit der Vorlage zusammengeführt werden. Wenn dieser Parameter festgelegt ist, wird der Parameter „dataRef“ im HTML5-Formular ignoriert. Als Best Practice wird empfohlen, den Datenansatz zu verwenden.

Es wird empfohlen, das Datenattribut in der Anfrage mit den Daten festzulegen, mit denen das Formular vorausgefüllt werden soll.

slingRequest.setAttribute(&quot;data&quot;, content);

In diesem Beispiel legen wir das Datenattribut mit dem Inhalt fest. Der Inhalt stellt die Daten dar, mit denen Sie das Formular vorab ausfüllen möchten. Normalerweise würden Sie den &quot;Inhalt&quot;abrufen, indem Sie einen REST-Aufruf an einen internen Dienst durchführen.

Dazu müssen Sie ein benutzerdefiniertes Profil erstellen. Die Details zur Erstellung eines benutzerdefinierten Profils werden in [Dokumentation zu AEM Forms](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html).

Nachdem Sie Ihr benutzerdefiniertes Profil erstellt haben, erstellen Sie eine JSP-Datei, die die Daten abruft, indem Sie Aufrufe an Ihr Backend-System vornehmen. Nachdem die Daten abgerufen wurden, verwenden Sie slingRequest.setAttribute(&quot;data&quot;, content); zum Vorausfüllen des Formulars

Wenn die XDP gerendert wird, können Sie auch einige Parameter an die xdp übergeben und basierend auf dem Wert des Parameters können Sie die Daten aus dem Backend-System abrufen.

[Beispielsweise hat diese URL den Parameter name .](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

Die JSP, die Sie schreiben, hat Zugriff auf den Parameter name über request.getParameter(&quot;name&quot;) . Anschließend können Sie den Wert dieses Parameters an Ihren Backend-Prozess übergeben, um die erforderlichen Daten abzurufen.
Um diese Funktion auf Ihrem System verwenden zu können, führen Sie die folgenden Schritte aus:

* [Herunterladen und Importieren von Assets in AEM mit Package Manager](assets/prepopulatemobileform.zip)
Das Paket installiert Folgendes

   * CustomProfile
   * Beispiel-XDP
   * Beispielendpunkt für POST, der Daten zum Ausfüllen des Formulars zurückgibt

>[!NOTE]
>
>Wenn Sie Ihr Formular durch Aufrufen des Workbench-Prozesses ausfüllen möchten, sollten Sie die callWorkbenchProcess.jsp in Ihre /apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp anstatt in setdata.jsp aufnehmen.

* [Verweisen Sie Ihren bevorzugten Browser auf diese URL](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems). Das Formular sollte vorab mit dem Wert des Parameter name ausgefüllt werden
