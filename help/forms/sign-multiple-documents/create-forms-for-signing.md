---
title: Erstellen von zu signierenden Formularen
description: Erstellen Sie Formulare, die in das Signaturpaket aufgenommen werden müssen.
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6893
thumbnail: 6893.jpg
topic: Development
role: User
level: Beginner
exl-id: 565d81a4-2918-44ea-a3e9-ed79f246f08a
duration: 89
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 100%

---

# Erstellen von zu signierenden Formularen

Der nächste Schritt besteht darin, die adaptiven Formulare zu erstellen, die in das Paket aufgenommen werden sollen. Beachten Sie beim Erstellen von Formularen zum Signieren die folgenden Punkte:

* Stellen Sie sicher, dass die Formulare auf der Vorlage **SignMultipleForms** (Mehrere Formulare signieren) basieren. Dadurch wird sichergestellt, dass die Formulare mit den aus der Datenbank abgerufenen Daten vorausgefüllt werden.

* Die Formulare müssen für Acrobat Sign konfiguriert werden und das Feld „signer1“ muss mit dem Feld „customerEmail“ verknüpft werden.
* Die Formulare müssen auch mit der Client-Bibliothek **getnextform** verknüpft werden.
* Die Formulare müssen die Komponente für den Signaturschritt verwenden.
* Das Formular muss auch die benutzerdefinierte Komponente **Mehrere Formulare signieren** verwenden. Mit dieser Komponente können Sie zum nächsten Formular navigieren, das im Paket signiert werden soll.
* Die Übermittlung des Formulars muss konfiguriert werden, um den AEM-Workflow **Signaturstatus aktualisieren** auszulösen.
* Stellen Sie sicher, dass der Datendateipfad auf **Data.xml** festgelegt ist. Dies ist sehr wichtig, da der Beispiel-Code in der Payload nach einer Datei namens „Data.xml“ sucht, um die Formularübermittlung zu verarbeiten.

Nachdem Sie Ihr Formular erstellt haben, schließen Sie das adaptive Formularfragment **commonfields** im Formular ein. Das Fragment wird als verborgen markiert. Dieses Fragment enthält die folgenden Felder:

* **signed**: In diesem Feld wird der Status der Signatur gespeichert.
* **guid**: Dies ist die eindeutige Kennung zur Identifizierung des Formulars im Paket.
* **customerEmail**: Dieses Feld enthält die E-Mail-Adresse der Kundin oder des Kunden.



>[!NOTE]
>Wenn Sie Daten aus einem Formular in ein anderes Formular im Paket übertragen möchten, stellen Sie sicher, dass die Formularfelder in allen Formularen identisch benannt sind.

## Alldone-Formular

Sobald alle Formulare im Paket ausgefüllt und signiert sind, sollte die entsprechende Nachricht angezeigt werden. Diese Meldung wird mithilfe des Alldone-Formulars angezeigt. Das Alldone-Formular ist Teil der Beispielformulare.

## Assets

Beispielformulare, darunter die in diesem Tutorial verwendeten, können [hier](assets/forms-for-signing.zip) heruntergeladen werden.

## Nächste Schritte

[Testen der Lösung auf Ihrem lokalen System](./testing-and-trouble-shooting.md)
