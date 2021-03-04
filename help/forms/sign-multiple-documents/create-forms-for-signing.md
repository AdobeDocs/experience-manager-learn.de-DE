---
title: Forms zum Signieren erstellen
description: Erstellen Sie Formulare, die in das Signierungspaket eingeschlossen werden müssen.
feature: Adaptive Formulare
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6893
thumbnail: 6893.jpg
topic: Entwicklung
role: Geschäftspraktiker
level: Anfänger
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 1%

---


# Formulare zum Signieren erstellen

Der nächste Schritt besteht darin, die adaptiven Formulare zu erstellen, die im Paket enthalten sein sollen. Beachten Sie beim Erstellen von Formularen zum Signieren die folgenden Punkte:

* Vergewissern Sie sich, dass die Formulare auf der Vorlage **SignMultipleForms** basieren. Dadurch wird sichergestellt, dass die Formulare vorab mit den Daten ausgefüllt werden, die aus der Datenbank abgerufen werden.

* Die Formulare müssen für die Verwendung von Adobe Sign konfiguriert sein, und das Feld &quot;signer1&quot;muss mit dem Feld &quot;E-Mail-Adresse des Kunden&quot;verknüpft sein
* Die Formulare müssen auch mit clientLib namens **getnextform** verknüpft sein
* Die Formulare müssen die Komponente &quot;Unterschriftsschritt&quot;verwenden.
* Das Formular muss auch die benutzerdefinierte Komponente **Mehrere Formulare signieren** verwenden. Mit dieser Komponente können Sie zum nächsten Formular navigieren, um sich im Paket anzumelden.
* Die Übermittlung des Formulars muss auf Trigger AEM Arbeitsablauf **Signaturstatus aktualisieren** konfiguriert werden
* Stellen Sie sicher, dass der Datendateipfad auf **Data.xml** eingestellt ist. Dies ist sehr wichtig, da der Beispielcode nach einer Datei namens &quot;Data.xml&quot;in der Nutzlast des Prozesses der Formularübermittlung sucht.

Nachdem Sie Ihr Formular erstellt haben, fügen Sie das adaptive Formularfragment **commonfields** in das Formular ein. Das Fragment wird als ausgeblendet markiert. Dieses Fragment enthält die folgenden Felder.

* **signed**  - Das Feld für den Status der Signatur
* **guid** - Eindeutige Kennung zur Identifizierung des Formulars im Paket
* **customerEmail**  - Dieses Feld enthält die E-Mail des Kunden



>[!NOTE]
>Wenn Sie Daten von einem Formular in ein anderes Formular im Paket übertragen möchten, vergewissern Sie sich bitte, dass die Formularfelder in allen Formularen identisch benannt sind.

## Gesamtes Formular

Sobald alle Formulare im Paket ausgefüllt und unterzeichnet sind, muss die entsprechende Meldung angezeigt werden. Diese Meldung wird mithilfe des Alldone-Formulars angezeigt. Das Alldone-Formular ist in den Musterformularen enthalten.

## Assets

Die Musterformulare einschließlich der in diesem Lernprogramm verwendeten können [von hier heruntergeladen werden.](assets/forms-for-signing.zip)
