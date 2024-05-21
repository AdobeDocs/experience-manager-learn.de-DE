---
title: Verwenden des Adobe Sign-API in AEM Forms
description: Senden von Dokumenten zur Unterschrift mit Hilfsmethoden von Adobe Sign
feature: Adaptive Forms
jira: KT-15474
topic: Development
role: Developer
level: Beginner
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 3400b728-58ca-44c3-a882-e3170755f845
duration: 74
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '298'
ht-degree: 100%

---

# Verwenden von Hilfsmethoden von Adobe Sign

In bestimmten Anwendungsfällen ist es möglicherweise erforderlich, ein Dokument zur Signatur zu senden, ohne einen AEM-Workflow zu verwenden. In solchen Fällen ist es sehr praktisch, die Wrapper-Methoden zu verwenden, die im in diesem Artikel bereitgestellten Beispielpaket enthalten sind.

## Implementieren des benutzerdefinierten OSGi-Bundles

[Implementieren des OSGi-Bundles](assets/AdobeSignHelperMethods.core-1.0.0-SNAPSHOT.jar) über die AEM OSGi-Web-Konsole. Geben Sie den API-Integrationsschlüssel und den API-Benutzernamen mithilfe der OSGi-Konfiguration wie unten angezeigt über den Konfigurations-Manager der AEM OSGi-Web-Konsole an.

 Beachten Sie, dass die `AdobeSignHelperMethods` des OSGi-Bundles nicht als Produkt-Code von Adobe Experience Manager (AEM) erkannt wird und daher nicht vom Adobe-Support unterstützt wird.
![Sign-Konfiguration](assets/sign-configuration.png)


## API-Dokumentation

Die folgenden API-Dokumentationen sind über die `AcrobatSignHelperMethods` des OSGi-Dienstes verfügbar, der im OSGi-Paket bereitgestellt wird.

### getTransientDocumentID

`String getTransientDocumentID(Document documentForSigning) throws IOException`


Das Dokument, mit dem eine Vereinbarung oder ein Web-Formular erstellt wird. Das Dokument wird zuerst von der Absenderin bzw. vom Absender in Acrobat Sign hochgeladen. Dies wird als _Übergang_ bezeichnet, da die Option nur 7 Tage nach dem Upload verfügbar ist. Bei diesen Methoden wird `com.adobe.aemfd.docmanager.Document` akzeptiert und die vorübergehende Dokument-ID wird zurückgegeben.

### getAgreementID

`String getAgreementId(String transientDocumentID, String email) throws ClientProtocolException, IOException`

Senden Sie das Dokumentmit der vorübergehenden Dokument-ID zur Unterschrift an die durch den E-Mail-Parameter identifizierte Benutzerin bzw. den Benutzer.

### getWidgetID

`String getWidgetID(String transientDocumentID)`

Ein Widget ist wie eine wiederverwendbare Vorlage, die Benutzerinnen und Benutzern mehrmals präsentiert und mehrmals unterschrieben werden kann. Verwenden Sie diese Methode, um die Widget-ID mit der vorübergehenden Dokument-ID abzurufen.

### getWidgetURL

`String getWidgetURL(String widgetId) throws ClientProtocolException, IOException`

Rufen Sie eine Widget-URL für eine bestimmte Widget-ID ab. Diese Widget-URL kann dann den Benutzenden zur Unterschrift des Dokuments angezeigt werden.

## Verwenden des API

`AcrobatSignHelperMethods` ist ein OSGi-Dienst, daher muss er mit der Anmerkung @Reference in Ihrem Java-Code kommentiert werden.

```java
...
// Import the AcrobatSignHelperMethods from the provided bundle
import com.acrobatsign.core.AcrobatSignHelperMethods;
...

@Component(service = { Example.class })
public class ExampleImpl implements Example {

 // Gain a reference to the provided AcrobatSignHelperMethods OSGi service
 @Reference
 com.acrobatsign.core.AcrobatSignHelperMethods acrobatSignHelperMethods;

 function void example() { 
    ...
    // Use the AcrobatSignHelperMethods API methods in your code
    String transientDocumentId = acrobatSignHelperMethods.getTransientDocumentID(documentForSigning);

    String agreementId = acrobatSignHelperMethods.getAgreementId(transientDocumentID, "johndoe@example.com");
    ...
 }
}
```
