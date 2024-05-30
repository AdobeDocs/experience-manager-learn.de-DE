---
title: Anzeigen von QR-Code im adaptiven Formular
description: Anzeigen von QR-Code in einem adaptiven Formular
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-15603
last-substantial-update: 2024-05-28T00:00:00Z
source-git-commit: e20d9f80cc7e1c6f5f6c81233d9a5178551e2fa2
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 1%

---

# Beispielkomponente für QR-Code

Die Einbettung eines QR-Codes in ein adaptives Formular kann die Benutzerfreundlichkeit und Effizienz beim Zugriff auf zusätzliche Informationen im Zusammenhang mit dem Formular erheblich verbessern.

Die Beispielkomponente verwendet [QRCode.js](https://davidshimjs.github.io/qrcodejs/).

QRCode.js ist eine JavaScript-Bibliothek für die Erstellung von QRCode. Es unterstützt Browser-übergreifend mit HTML5-Arbeitsfläche und Tabellen-Tag in DOM.

Die Komponente generiert den QR-Code basierend auf dem Wert, der in der Konfigurationseigenschaft der Komponente angegeben ist.
![Bild](assets/qr-code-url.png)

Der folgende Code wurde in der body.jsp der Komponente &quot;qr-code-generator&quot;verwendet.

&quot;url&quot;ist die URL, die in den QR-Code eingebettet werden muss. Diese URL wird in den Konfigurationseigenschaften der QR-Code-Komponente angegeben.

```java
<%@include file="/libs/foundation/global.jsp"%>
<body>
    <h2>Scan the QR Code for more information related to this form</h2>
    <div data-url="<%=properties.get("url")%>">
    </div>
    <div id="qrcode">
    </div>
</body>
```



Der folgende Code verwendet die makeCode-Methode der Bibliothek QRCode.js in der Client-Bibliothek der Komponente qr-code-generator. Der generierte QR-Code wird an das div angehängt, das durch id identifiziert wird **&quot;qrcode&quot;**.

```javascript
$(document).ready(function()
  {
      var qrcode = new QRCode("qrcode");
      qrcode.makeCode(document.querySelector("[data-url]").getAttribute("data-url"));
      
 });
```

## Bereitstellen der Assets auf Ihrem lokalen Server

* [Laden Sie die QR-Code-Komponente mit Package Manager herunter und installieren Sie sie.](assets/qrcode.zip)
* [Laden Sie das adaptive Beispielformular mit Package Manager herunter und installieren Sie es.](assets/form-with-qr-code.zip)
* [Zeigen Sie das Formular in einer Vorschau an](http://localhost:4502/content/dam/formsanddocuments/qrcode/w9form/jcr:content?wcmmode=disabled). Der Hilfeabschnitt des Formulars enthält den QR-Code.


