---
title: Ausfüllen eines adaptiven Formulars mithilfe von Abfrageparametern.
description: Füllen Sie adaptive Formulare mit Daten aus Abfrageparametern aus.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: KT-11470
last-substantial-update: 2020-11-12T00:00:00Z
exl-id: 14ac6ff9-36b4-415e-a878-1b01ff9d3888
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 100%

---

# Vorausfüllen eines adaptiven Formulars mithilfe von Abfrageparametern

Für eines unserer Projekte gab es die Anforderung, ein adaptives Formular mithilfe der Abfrageparameter auszufüllen. In der folgenden URL sind beispielsweise die Felder „FirstName“ und „LastName“ im adaptiven Formular auf „John“ bzw. „Doe“ festgelegt.

```html
https://forms.enablementadobe.com/content/forms/af/testingxml.html?FirstName=John&LastName=Doe
```

Um dies zu erreichen, wurde eine neue adaptive Formularvorlage erstellt und mit einer Seitenkomponente verknüpft. In dieser Seitenkomponente haben wir ein JSP, um die Abfrageparameter zu erhalten und eine XML-Struktur zu erstellen, die zum Ausfüllen des adaptiven Formulars verwendet werden kann.

Die Details zum Erstellen einer neuen adaptiven Formularvorlage und einer Seitenkomponente werden [in diesem Video erklärt.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/storing-and-retrieving-form-data/part5.html?lang=de)

Im Folgenden finden Sie den Code, der auf der JSP-Seite verwendet wurde:

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
Das adaptive Formular sollte mit den Werten „John“ und „Doe“ ausgefüllt werden
