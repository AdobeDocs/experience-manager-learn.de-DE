---
title: Vorausfüllen von HTML5-Formularen mit Datenattributen.
description: Vorausfüllen von HTML5-Formulare durch Abrufen der Daten von der Backend-Quelle.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: ab0f5282-383b-4be6-9c57-cded6ab37528
last-substantial-update: 2020-01-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: ht
source-wordcount: '471'
ht-degree: 100%

---

# Vorausfüllen von HTML5-Formularen mit Datenattributen {#prepopulate-html-forms-using-data-attribute}


XDP-Vorlagen, die mit AEM Forms im HTML-Format gerendert werden, werden als HTML5- oder Mobile-Formulare bezeichnet. Ein gängiger Anwendungsfall besteht darin, diese Formulare vorab beim Rendern auszufüllen.

Es gibt zwei Möglichkeiten, Daten mit der XDP-Vorlage zusammenzuführen, wenn sie als HTML gerendert wird.

**dataRef**: Sie können den dataRef-Parameter in der URL verwenden. Dieser Parameter gibt den absoluten Pfad der Datendatei an, die mit der Vorlage zusammengeführt wird. Dieser Parameter kann eine URL eines REST-Dienstes sein, der die Daten im XML-Format zurückgibt.

**data**: Dieser Parameter gibt die UTF-8-kodierten Datenbytes an, die mit der Vorlage zusammengeführt werden. Wenn dieser Parameter angegeben ist, ignoriert das HTML5-Formular den dataRef-Parameter. Als Best Practice wird der Datenansatz empfohlen.

Es wird empfohlen, das Datenattribut in der Anfrage mit den Daten festzulegen, mit denen das Formular vorausgefüllt werden soll.

slingRequest.setAttribute(&quot;data&quot;, content);

In diesem Beispiel legen wir das Datenattribut mit dem Inhalt („content“) fest. Der Inhalt steht für die Daten, mit dem das Formular vorausausgefüllt werden soll. Normalerweise würden Sie den „Inhalt“ durch einen REST-Aufruf an einen internen Dienst abrufen.

Für diesen Anwendungsfall müssen Sie ein benutzerdefiniertes Profil erstellen. Die Erstellung eines benutzerdefinierten Profils wird in der [AEM Forms-Dokumentation](https://helpx.adobe.com/de/aem-forms/6/html5-forms/custom-profile.html) ausführlich beschrieben.

Nachdem Sie Ihr benutzerdefiniertes Profil erstellt haben, erstellen Sie eine JSP-Datei, die die Daten durch Aufrufe an Ihr Backend-System abruft.  Nachdem die Daten abgerufen wurden, verwenden Sie „slingRequest.setAttribute(&quot;data&quot;, content);“ zum Vorausfüllen des Formulars.

Wenn die XDP-Vorlage gerendert wird, können Sie auch einige Parameter an die XDP-Vorlage weitergeben und basierend auf dem Wert des Parameters die Daten vom Backend-System abrufen.

[Beispielsweise weist diese URL den Parameter „name“ auf.](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

Die JSP-Datei, die Sie schreiben, hat Zugriff auf den Parameter „name“ über „request.getParameter(&quot;name&quot;)“. Anschließend können Sie den Wert dieses Parameters an Ihren Backend-Prozess weitergeben, um die erforderlichen Daten abzurufen.
Um diese Funktion auf Ihrem System verwenden zu können, führen Sie die folgenden Schritte aus:

* [Laden Sie die Assets herunter und importieren Sie sie mithilfe von Package Manager in AEM.](assets/prepopulatemobileform.zip)
Das Paket installiert Folgendes:

   * benutzerdefiniertes Profil
   * Beispiel-XDP
   * Beispiel-POST-Endpunkt, der Daten zum Ausfüllen des Formulars zurückgibt

>[!NOTE]
>
>Wenn Sie Ihr Formular durch Aufrufen des Workbench-Prozesses ausfüllen möchten, sollten Sie statt der Datei „setdata.jsp“ die Datei „callWorkbenchProcess.jsp“ in „/apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp“ einschließen.

* [Verweisen Sie Ihren bevorzugten Browser auf diese URL.](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems). Das Formular sollte mit dem Wert des name-Parameters vorausgefüllt werden.
