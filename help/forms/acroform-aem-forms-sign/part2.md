---
title: Acroforms mit AEM Forms
seo-title: Adaptive Formulardaten mit Acroform zusammenführen
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 1ba56ad44df4dc327cf37d39ac72539b5c7af4a2
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 2%

---


# Schema aus dem Formular erstellen

Im nächsten Schritt erstellen Sie ein Schema aus dem im vorherigen Schritt erstellten Acroform. Im Rahmen dieses Lernprogramms wird eine Beispielanwendung zum Erstellen des Schemas bereitgestellt. Um das Schema zu erstellen, befolgen Sie bitte die folgenden Anweisungen:

1. Bei [CRXDE Lite anmelden](http://localhost:4502/crx/de)
2. Öffnen der Datei `/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. Ändern Sie den Ordner `saveLocation` in einen entsprechenden Ordner auf Ihrer Festplatte. Stellen Sie sicher, dass der Ordner, in dem Sie speichern, bereits erstellt wurde.
4. Verweisen Sie Ihren Browser auf die [XSD](http://localhost:4502/content/DocumentServices/CreateXsd.html) -Seite, die auf AEM gehostet wird.
5. Ziehen Sie das Acroform per Drag &amp; Drop.
6. Überprüfen Sie den in Schritt 3 angegebenen Ordner. Die Schema-Datei wird an diesem Speicherort gespeichert.

## Acroform hochladen

Damit diese Demo auf Ihrem System funktioniert, müssen Sie einen Ordner mit dem Namen `acroforms` &quot;AEM Assets&quot;erstellen. Laden Sie das Acroform in diesen `acroforms` Ordner hoch.

>[!NOTE]
Der Beispielcode sucht nach dem acroform in diesem Ordner. Das Formular &quot;acroform&quot;ist erforderlich, um die gesendeten Daten des adaptiven Formulars zusammenzuführen.
