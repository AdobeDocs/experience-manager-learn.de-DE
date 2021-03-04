---
title: HTML5-Forms mithilfe des Datenattributs vorauffüllen.
seo-title: HTML5-Forms mithilfe des Datenattributs vorauffüllen.
description: Füllen Sie HTML5-Formulare, indem Sie Daten aus der Backend-Quelle abrufen.
seo-description: Füllen Sie HTML5-Formulare, indem Sie Daten aus der Backend-Quelle abrufen.
feature: Adaptive Formulare
topics: mobile-forms
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5.
uuid: 889d2cd5-fcf2-4854-928b-0c2c0db9dbc2
discoiquuid: 3aa645c9-941e-4b27-a538-cca13574b21c
topic: Entwicklung
role: Entwickler
level: Erfahren
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '515'
ht-degree: 2%

---


# HTML5-Forms mit Datenattribut {#prepopulate-html-forms-using-data-attribute} vorbeleben

Besuchen Sie die Seite [AEM Forms samples](https://forms.enablementadobe.com/content/samples/samples.html?query=0) für einen Link zu einer Live-Demo dieser Funktion.

XDP-Vorlagen, die mit AEM Forms im HTML-Format wiedergegeben werden, werden als HTML5- oder Mobile Forms bezeichnet. In der Regel werden diese Formulare bei der Wiedergabe vorab ausgefüllt.

Es gibt 2 Möglichkeiten, Daten mit der xdp-Vorlage zusammenzuführen, wenn sie als HTML wiedergegeben werden.

**dataRef**: Sie können den Parameter dataRef in der URL verwenden. Dieser Parameter gibt den absoluten Pfad der Datendatei an, die mit der Vorlage zusammengeführt wird. Dieser Parameter kann eine URL zu einem REST-Dienst sein, der die Daten im XML-Format zurückgibt.

**data**: Dieser Parameter gibt die UTF-8-kodierten Datenbyte an, die mit der Vorlage zusammengeführt werden. Wenn dieser Parameter festgelegt ist, wird der Parameter „dataRef“ im HTML5-Formular ignoriert. Als Best Practice empfehlen wir die Verwendung des Datenansatzes.

Die empfohlene Methode besteht darin, das Datenattribut in der Anforderung mit den Daten festzulegen, mit denen das Formular im Voraus gefüllt werden soll.

slingRequest.setAttribute(&quot;data&quot;, content);

In diesem Beispiel legen wir das Datenattribut mit dem Inhalt fest. Der Inhalt stellt die Daten dar, mit denen Sie das Formular vorab füllen möchten. Normalerweise würden Sie den &quot;Inhalt&quot;abrufen, indem Sie einen REST-Aufruf an einen internen Dienst richten.

Um dies zu erreichen, müssen Sie ein benutzerdefiniertes Profil erstellen. Die Details zum Erstellen von benutzerdefiniertem Profil sind in der [AEM Forms-Dokumentation hier](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html) eindeutig dokumentiert.

Nachdem Sie Ihr benutzerdefiniertes Profil erstellt haben, erstellen Sie eine JSP-Datei, die die Daten abruft, indem Sie Aufrufe an Ihr Backend-System tätigen. Nach dem Abrufen der Daten verwenden Sie slingRequest.setAttribute(&quot;data&quot;, content); zum Vorausfüllen des Formulars

Wenn die XDP gerendert wird, können Sie auch einige Parameter an die xdp übergeben und basierend auf dem Wert des Parameters können Sie die Daten vom Backend-System abrufen.

[Beispielsweise hat diese URL den Parameter name](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

Die von Ihnen geschriebene JSP hat Zugriff auf den Parameter name über request.getParameter(&quot;name&quot;) . Anschließend können Sie den Wert dieses Parameters an Ihren Backend-Prozess übergeben, um die erforderlichen Daten abzurufen.
Gehen Sie wie folgt vor, um diese Funktion auf Ihrem System zu verwenden:

* [Herunterladen und Importieren der Assets AEM mit Package ](assets/prepopulatemobileform.zip)
ManagerDas Paket installiert Folgendes

   * CustomProfile
   * Beispiel-XDP
   * Beispiel-POST-Endpunkt, der Daten zum Ausfüllen des Formulars zurückgibt

>[!NOTE]
>
>Wenn Sie Ihr Formular durch Aufrufen von Workbench-Prozess ausfüllen möchten, sollten Sie callWorkbenchProcess.jsp anstelle von setdata.jsp in /apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp einfügen

* [Verweisen Sie Ihren bevorzugten Browser auf diese URL](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems). Das Formular sollte vorab mit dem Wert des Parameters name ausgefüllt werden
