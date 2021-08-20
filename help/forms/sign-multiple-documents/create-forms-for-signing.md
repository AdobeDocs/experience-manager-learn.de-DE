---
title: Erstellen von Forms für Signaturen
description: Erstellen Sie Formulare, die in das Signaturpaket aufgenommen werden müssen.
feature: Adaptive Formulare
version: 6.4,6.5
kt: 6893
thumbnail: 6893.jpg
topic: Entwicklung
role: User
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '329'
ht-degree: 1%

---


# Formulare zum Signieren erstellen

Der nächste Schritt besteht darin, die adaptiven Formulare zu erstellen, die in das Paket aufgenommen werden sollen. Beachten Sie beim Erstellen von Formularen zum Signieren die folgenden Punkte:

* Stellen Sie sicher, dass die Formulare auf der Vorlage **SignMultipleForms** basieren. Dadurch wird sichergestellt, dass die Formulare vorab mit den aus der Datenbank abgerufenen Daten gefüllt werden.

* Die Formulare müssen für die Verwendung von Adobe Sign konfiguriert werden und das signer1 -Feld muss mit dem Feld Customer Email verknüpft werden
* Die Formulare müssen auch mit clientLib namens **getnextform** verknüpft werden
* Die Formulare müssen die Signaturschritt-Komponente verwenden.
* Das Formular muss auch die benutzerdefinierte Komponente **Mehrere Formulare unterschreiben** verwenden. Mit dieser Komponente können Sie zum nächsten Formular navigieren, das Sie im Paket signieren können.
* Die Übermittlung des Formulars muss für den Trigger AEM Workflow **Update Signature Status** konfiguriert werden
* Stellen Sie sicher, dass der Datendateipfad auf **Data.xml** festgelegt ist. Dies ist sehr wichtig, da der Beispielcode in der Payload nach einer Datei namens Data.xml sucht, in der der Prozess der Formularübermittlung ausgeführt wird.

Nachdem Sie Ihr Formular verfasst haben, fügen Sie das adaptive Formularfragment **commonfields** in das Formular ein. Das Fragment wird als ausgeblendet markiert. Dieses Fragment enthält die folgenden Felder.

* **signed**  - Das Feld, in dem der Status der Signatur gespeichert werden soll
* **guid**  - Eindeutige Kennung zur Identifizierung des Formulars im Paket
* **customerEmail**  - In diesem Feld wird die E-Mail des Kunden gespeichert.



>[!NOTE]
>Wenn Sie Daten aus einem Formular in ein anderes Formular im Paket übertragen möchten, stellen Sie sicher, dass die Formularfelder in allen Formularen identisch benannt sind.

## Alle fertigen Formulare

Sobald alle Formulare im Paket ausgefüllt und signiert sind, muss die entsprechende Nachricht angezeigt werden. Diese Meldung wird mithilfe des Aldone-Formulars angezeigt. Das Formular Alldone ist in den Beispielformularen enthalten.

## Assets

Die in diesem Tutorial verwendeten Beispielformulare einschließlich der können [von hier heruntergeladen werden](assets/forms-for-signing.zip)
