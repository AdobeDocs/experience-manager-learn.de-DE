---
title: Installieren Sie die erforderlichen React-Bibliotheken des adaptiven Formulars
description: Hinzufügen der erforderlichen Abhängigkeiten zu Ihrem React-Projekt
feature: Adaptive Forms
version: 6.5
kt: 13285
topic: Development
role: User
level: Intermediate
source-git-commit: c6e83a627743c40355559d9cdbca2b70db7f23ed
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 1%

---


# Installieren der erforderlichen Abhängigkeiten

Installieren Sie die folgenden Abhängigkeiten in Ihrem React-Projekt, um Headless-adaptive Formulare in Ihrem React-Projekt zu verwenden

* @aemforms/af-response-components
* @aemforms/af-response-renderer

Aktualisieren Sie package.json, um die folgenden Abhängigkeiten einzuschließen. Zum Zeitpunkt des Schreibens war 0.22.41 die aktuelle Version

```json
"@aemforms/af-react-components": "^0.22.41",
"@aemforms/af-react-renderer": "^0.22.41",
```

## Proxy einrichten

Cross-Origin Resource Sharing (CORS) ist ein Sicherheitsmechanismus, der Webbrowser daran hindert, Anforderungen an eine andere Domäne als die zu stellen, auf der die App gehostet wird. CORS-Fehler können auftreten, wenn Sie versuchen, Daten von einer API abzurufen, die auf einer anderen Domäne gehostet wird. Wenn Sie einen Proxy einrichten, können Sie CORS-Einschränkungen umgehen und über Ihre React-App Anfragen an die API richten. Ich habe den folgenden Code in einer Datei namens setUpProxy.js im Ordner src verwendet. **Vergewissern Sie sich, dass Sie das Ziel so ändern, dass es auf Ihre Veröffentlichungsinstanz verweist.**

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

Außerdem müssen Sie die **http-proxy-middleware** -Modul zu Ihrem Projekt hinzufügen.

## Nächste Schritte

[Abrufen des einzubettenden Formulars](./fetch-the-form.md)