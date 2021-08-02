---
title: AEM Service-Anmeldedaten
description: Laden Sie die Anmeldedaten für den Dienst von AEM Developer Console herunter.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
feature: Adaptive Formulare
topic: Entwicklung
kt: 8192
thumbnail: 330519.jpg
source-git-commit: f2a94910fbc29b705f82a66d8248cbcf54366874
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 2%

---


# Dienstberechtigungen

Integrationen mit AEM as a Cloud Service müssen sich sicher bei AEM authentifizieren können. AEM Developer Console generiert Service-Anmeldeinformationen, die von externen Anwendungen, Systemen und Diensten verwendet werden, um programmgesteuert mit AEM Author- oder Publish-Diensten über HTTP zu interagieren.

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

Die heruntergeladene Datei mit den Dienstanmeldeinformationen wird in der bereitgestellten Eclipse als Ressourcendatei mit dem Namen service_token.json gespeichert. Die Werte in der Datei service_token werden verwendet, um das JWT zu generieren und das JWT gegen ein Zugriffstoken auszutauschen. Die Dienstprogrammklasse GetServiceCredentials wird verwendet, um die Eigenschaftswerte aus der Ressourcendatei service_token.json abzurufen.
