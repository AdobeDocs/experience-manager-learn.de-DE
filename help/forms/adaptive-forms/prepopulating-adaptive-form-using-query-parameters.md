---
title: Füllen Sie Adaptive Forms mithilfe von Abfrageparametern.
description: Füllen Sie Adaptive Forms mit Daten aus Abfrageparametern.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
kt: 11470
last-substantial-update: 2020-11-12T00:00:00Z
source-git-commit: fad7630d2d91d03b98a3982f73a689ef48700319
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# Adaptive Forms mithilfe von Abfrageparametern auffüllen

Einer unserer Kunden musste das adaptive Formular mithilfe der Abfrageparameter ausfüllen. In der folgenden URL sind beispielsweise die Felder FirstName und LastName im adaptiven Formular auf John bzw. Doe festgelegt.

```html
https://forms.enablementadobe.com/content/forms/af/testingxml.html?FirstName=John&LastName=Doe
```

Um dies zu erreichen, wurde eine neue adaptive Formularvorlage erstellt und mit einer Seitenkomponente verknüpft. In dieser Seitenkomponente haben wir eine JSP, um die Abfrageparameter zu speichern und eine XML-Struktur zu erstellen, die zum Ausfüllen des adaptiven Formulars verwendet werden kann.

Die Details zum Erstellen einer neuen adaptiven Formularvorlage und Seitenkomponente sind [in diesem Video erläutert.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/storing-and-retrieving-form-data/part5.html?lang=en)

Im Folgenden finden Sie den Code, der auf der JSP-Seite verwendet wurde

```java
java.util.Enumeration enumeration = request.getParameterNames();
String dataXml = "<afData><afUnboundData><data>";
while (enumeration.hasMoreElements())
{
   String parameterName = (String) enumeration.nextElement();
   dataXml = dataXml + "<" + parameterName + ">" + request.getParameter(parameterName) + "</" + parameterName + ">";

}

dataXml = dataXml + "</data></afUnboundData></afData>";
//System.out.println("The data xml is "+dataXml);
slingRequest.setAttribute("data", dataXml);
```

>[!NOTE]
>
>Wenn Ihr Formular ein Schema verwendet, ist die Struktur Ihrer XML anders und Sie müssen die XML entsprechend erstellen.


## Bereitstellen der Assets auf Ihrem System

* [Herunterladen und Installieren der adaptiven Formularvorlage mithilfe des Package Manager](assets/populate-with-xml.zip)
* [Herunterladen und Installieren des adaptiven Beispielformulars](assets/populate-af-with-query-paramters-form.zip)

* [Vorschau des adaptiven Formulars](http://localhost:4502/content/dam/formsanddocuments/testingxml/jcr:content?wcmmode=disabled&amp;FirstName=John&amp;LastName=Doe)
Das adaptive Formular sollte mit dem Wert John und Doe ausgefüllt werden.
