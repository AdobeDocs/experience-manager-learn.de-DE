---
title: Installieren der erforderlichen React-Bibliotheken für adaptive Formulare
description: Hinzufügen der erforderlichen Abhängigkeiten zu Ihrem React-Projekt
feature: Adaptive Forms
version: 6.5
jira: KT-13285
topic: Development
role: User
level: Intermediate
exl-id: 0ed44016-d52a-4980-a0b1-06da149c3cb1
duration: 54
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 100%

---

# Installieren der erforderlichen Abhängigkeiten

Installieren Sie die folgenden Abhängigkeiten in Ihrem React-Projekt, um adaptive Headless-Formulare in Ihrem React-Projekt zu verwenden

* @aemforms/af-react-components
* @aemforms/af-react-renderer

Aktualisieren Sie die package.json, um die folgenden Abhängigkeiten einzuschließen. Zum Zeitpunkt des Schreibens war 0.22.41 die aktuelle Version

```json
"@aemforms/af-react-components": "^0.22.41",
"@aemforms/af-react-renderer": "^0.22.41",
```

>[!NOTE]
>
>Die Dropdown-Liste und das Kartenlayout in diesem Tutorial wurden mithilfe der [Material-Benutzeroberflächen-Bibliothek](https://mui.com/) erstellt. Sie müssen die entsprechenden Material-Benutzeroberfläche-Pakete herunterladen, damit der Code auf Ihrem System funktioniert.

## Einrichten des Proxy

Cross-Origin Resource Sharing (CORS) ist ein Sicherheitsmechanismus, der Webbrowser daran hindert, Anfragen an eine andere Domain als die zu stellen, auf der die App gehostet wird. CORS-Fehler können auftreten, wenn Sie versuchen, Daten von einer API abzurufen, die auf einer anderen Domäne gehostet wird. Indem Sie einen Proxy einrichten, können Sie CORS-Einschränkungen umgehen und über Ihre React-App Anfragen an die API richten. Ich habe den folgenden Code in einer Datei namens setUpProxy.js im src-Ordner verwendet. **Vergewissern Sie sich, dass Sie das Ziel so ändern, dass es auf Ihre Veröffentlichungsinstanz verweist.**

```
const { createProxyMiddleware } = require('http-proxy-middleware');
const proxy = {
    target: 'https://mypublishinstance:4503/',
    changeOrigin: true
}
module.exports = function(app) {
  app.use(
    '/adobe',
    createProxyMiddleware(proxy)
  ),
  app.use(
    '/content',
    createProxyMiddleware(proxy)
  );
};
```

Außerdem müssen Sie das **http-proxy-middleware**-Modul installieren und zu Ihrem Projekt hinzufügen.

## Nächste Schritte

[Abrufen des einzubettenden Formulars](./fetch-the-form.md)
