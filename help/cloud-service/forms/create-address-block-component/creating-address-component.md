---
title: Erstellen einer Adresskomponente
description: Erstellen einer neuen Adresskernkomponente in AEM Forms Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15752
exl-id: 280c9a30-e017-4bc0-9027-096aac82c22c
source-git-commit: ed64dd303a384d48f76c9b8e8e925f5d3b8f3247
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 91%

---

# Erstellen einer Adresskomponente

Melden Sie sich bei CRXDE in Ihrer lokalen Cloud-fähigen Instanz von AEM Forms an.

Erstellen Sie eine Kopie des Knotens ``/apps/bankingapplication/components/adaptiveForm/button`` und benennen Sie ihn in „addressblock“ um. Wählen Sie den Knoten „addressblock“ aus und legen Sie seine Eigenschaften wie unten dargestellt fest.

>[!NOTE]
>
> ``bankingapplication`` ist die appId, die beim Erstellen des Maven-Projekts bereitgestellt wurde. Diese appId kann in Ihrer Umgebung unterschiedlich sein. Sie können eine beliebige Komponente kopieren. Ich habe zufällig eine Kopie der Schaltflächenkomponente erstellt


![address-bloc](assets/address-properties.png)

## Eigenschaften des Knotens cq-template

Wählen Sie unter dem Knoten ``addressblock`` den Knoten ``cq-template`` aus und legen Sie die Eigenschaften wie unten dargestellt fest. Beachten Sie, dass fieldType auf das Panel 
![cq-Vorlage](assets/cq-template.png) festgelegt ist

## Hinzufügen von Knoten unter cq-template

Fügen Sie die folgenden Knoten des Typs ``nt:unstructured`` unter ``cq-template`` hinzu

* streetaddress
* city
* zip
* state

Diese Knoten stellen die Felder der Adressblock-Komponente dar. Die Felder „streetaddress“, „city“ und „zip“ sind Texteingabefelder und das Feld „state“ ein Dropdown-Feld.

## Festlegen der Eigenschaften des Knotens „streetaddress“

>[!NOTE]
>
> Die **_bankingapplication_** im Pfad bezieht sich auf die appId des Maven-Projekts. Dies kann in Ihrer Umgebung anders sein

Wählen Sie den Knoten ``streetaddress`` aus und legen Sie die Eigenschaften wie unten dargestellt fest.
![street-address](assets/streetaddress.png)

## Festlegen der Eigenschaften des Knotens „city“

Wählen Sie den Knoten ``city`` aus und legen Sie die Eigenschaften wie unten dargestellt fest.
![city](assets/city.png)

## Festlegen der Eigenschaften des Knotens „zip“

Wählen Sie den Knoten ``zip`` aus und legen Sie die Eigenschaften wie unten dargestellt fest.
![zip](assets/zip.png)

## Festlegen der Eigenschaften des Knotens „state“

Wählen Sie den Knoten ``state`` aus und legen Sie die Eigenschaften wie unten dargestellt fest. Beachten Sie den fieldType von „state“ – es ist als Dropdown-Liste festgelegt
![state](assets/state.png)

## Festlegen von Optionen für das Statusfeld

Wählen Sie den Knoten ``state`` aus und fügen Sie die folgenden Eigenschaften hinzu.

| Name | Typ | Wert |
|----------|----------|---------------------|
| enum | Zeichenfolge[] | CA,NY |
| enumNames | Zeichenfolge[] | Kalifornien, New York |


Die endgültige Adressblock-Komponente sieht wie folgt aus:

![final-address](assets/crx-address-block.png)

## Nächste Schritte

[Bereitstellen des Projekts](./deploy-your-project.md)
