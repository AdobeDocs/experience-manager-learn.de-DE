---
title: CSRF-Schutz
description: Erfahren Sie, wie Sie AEM CSRF-Token generieren und zu zulässigen POST-, PUT- und Löschanfragen für authentifizierte Benutzende zu AEM hinzufügen können.
version: Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-07-14T00:00:00Z
jira: KT-13651
thumbnail: KT-13651.jpeg
exl-id: 747322ed-f01a-48ba-a4a0-483b81f1e904
duration: 178
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 100%

---

# CSRF-Schutz

Erfahren Sie, wie Sie AEM CSRF-Token generieren und zu zulässigen POST-, PUT- und Löschanfragen für authentifizierte Benutzende zu AEM hinzufügen können.

AEM erfordert die Übermittlung eines gültigen CSRF-Tokens für __authentifizierte__ __POST-__, __PUT- oder __DELETE__-HTTP-Anfragen an den AEM-Author- und Publish-Service.

Das CSRF-Token ist nicht erforderlich für __GET__-Anfragen oder __anonyme__ Anfragen.

Wenn kein CSRF-Token mit einer POST-, PUT- oder DELETE-Anfrage gesendet wird, gibt AEM den „Fehler 403: Unzulässig“ zurück und AEM protokolliert den folgenden Fehler:

```log
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] isValidRequest: empty CSRF token - rejecting
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] doFilter: the provided CSRF token is invalid
```

Siehe die [Dokumentation für weitere Informationen zum AEM CSRF-Schutz](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html?lang=de).


## CSRF-Client-Bibliothek

AEM bietet eine Client-Bibliothek, die zum Generieren und Hinzufügen von XHR- und Formular-POST-Anfragen für CSRF-Token über das Patchen von Kernprotokollfunktionen verwendet werden kann. Die Funktion wird von der `granite.csrf.standalone`-Client-Bibliothekskategorie bereitgestellt.

Um diesen Ansatz zu verwenden, fügen Sie `granite.csrf.standalone` als Abhängigkeit zur Client-Bibliothek hinzu, die auf Ihrer Seite geladen wird. Wenn Sie beispielsweise die `wknd.site`-Kategorie der Client-Bibliothek verwenden, fügen Sie `granite.csrf.standalone` als Abhängigkeit zur Client-Bibliothek hinzu, die auf Ihrer Seite geladen wird.

## Benutzerdefinierte Formularübermittlung mit CSRF-Schutz

Wenn die Verwendung der [`granite.csrf.standalone`Client-Bibliothek](#csrf-client-library) nicht für Ihren Anwendungsfall verfügbar ist, können Sie einer Formularübermittlung manuell ein CSRF-Token hinzufügen. Das folgende Beispiel zeigt, wie ein CSRF-Token zu einer Formularübermittlung hinzugefügt werden kann.

Dieses Code-Snippet veranschaulicht, wie das CSRF-Token beim Senden des Formulars von AEM abgerufen und zu einer Formulareingabe mit dem Namen `:cq_csrf_token` hinzugefügt werden kann. Da das CSRF-Token eine kurze Lebensdauer hat, ist es am besten, das CSRF-Token unmittelbar vor dem Senden des Formulars abzurufen und festzulegen, um seine Gültigkeit zu gewährleisten.

```javascript
// Attach submit handler event to form onSubmit
document.querySelector('form').addEventListener('submit', async (event) => {
    event.preventDefault();

    const form = event.target;
    const response = await fetch('/libs/granite/csrf/token.json');
    const json = await response.json();
    
    // Create a form input named ``:cq_csrf_token`` with the CSRF token.
    let csrfTokenInput = form.querySelector('input[name=":cq_csrf_token"]');
    if (!csrfTokenInput?.value) {
        // If the form does not have a CSRF token input, add one.
        form.insertAdjacentHTML('beforeend', `<input type="hidden" name=":cq_csrf_token" value="${json.token}">`);
    } else {
        // If the form already has a CSRF token input, update the value.
        csrfTokenInput.value = json.token;
    }
    // Submit the form with the hidden input containing the CSRF token
    form.submit();
});
```

## Abrufen mit CSRF-Schutz

Wenn die Verwendung der [`granite.csrf.standalone`Client-Bibliothek](#csrf-client-library) nicht für Ihren Anwendungsfall verfügbar ist, können Sie einem XHR manuell ein CSRF-Token hinzufügen oder Anfragen abrufen. Das folgende Beispiel zeigt, wie ein CSRF-Token zu einem XHR hinzugefügt werden kann, das mit dem Abruf erstellt wurde.

Dieses Code-Snippet zeigt, wie ein CSRF-Token aus AEM abgerufen und zum `CSRF-Token`-HTTP-Anfragenheader einer Abrufanforderung hinzugefügt wird. Da das CSRF-Token eine kurze Lebensdauer hat, ist es am besten, das CSRF-Token unmittelbar vor der Abrufanfrage abzurufen und festzulegen, um seine Gültigkeit zu gewährleisten.

```javascript
/**
 * Get CSRF token from AEM.
 * CSRF token expire after a few minutes, so it is best to get the token before each request.
 * 
 * @returns {Promise<string>} that resolves to the CSRF token.
 */
async function getCsrfToken() {
    const response = await fetch('/libs/granite/csrf/token.json');
    const json = await response.json();
    return json.token;
}

// Fetch from AEM with CSRF token in a header named 'CSRF-Token'.
await fetch('/path/to/aem/endpoint', {
    method: 'POST',
    headers: {
        'CSRF-Token': await getCsrfToken()
    },
});
```

## Dispatcher-Konfiguration

Bei Verwendung von CSRF-Token im AEM Publish-Service muss die Dispatcher-Konfiguration aktualisiert werden, um GET-Anfragen an den CSRF-Token-Endpunkt zu ermöglichen. Die folgende Konfiguration ermöglicht GET-Anfragen an den CSRF-Token-Endpunkt im AEM Publish-Service. Wenn diese Konfiguration nicht hinzugefügt wird, gibt der CSRF-Token-Endpunkt die Antwort „404 Nicht gefunden“ zurück.

* `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
/0120 { /type "allow" /method "GET" /url "/libs/granite/csrf/token.json" }
...
```
