---
title: Austauschen von JWT gegen Zugriffs-Token
description: Tauschen Sie das JSON Web Token (JWT) mit Adobe IMS-APIs gegen ein AEM-Zugriffs-Token aus.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-8185
thumbnail: KT-8185.jpg
exl-id: ab7b8a06-3009-477d-9e98-590912e8e176
duration: 33
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '85'
ht-degree: 100%

---

# Austauschen von JWT gegen Zugriffs-Token


Das im vorherigen Schritt erstellte JWT wird mit Adobe IMS-APIs gegen ein Zugriffs-Token ausgetauscht, das dann für den Zugriff auf AEM as a Cloud Service verwendet werden kann. Um ein Zugriffs-Token anzufordern, senden Sie eine POST-Anfrage mit JWT, client_id und client_secret an den IMS-Authentifizierungsdienst.

Der folgende Code wurde verwendet, um ein Austausch-JWT für ein Zugriffs-Token zu generieren

```java
public String getAccessToken() {
        
        String jwtToken = getJWTToken();
        GetServiceCredentials getCredentials = new GetServiceCredentials();
        System.out.println("Getting Access Token");

        try {
                HttpClient httpClient = HttpClientBuilder.create().build();
                HttpHost authServer = new HttpHost(getCredentials.getIMS_ENDPOINT(), 443, "https");
                HttpPost authPostRequest = new HttpPost("/ims/exchange/jwt");
                List < NameValuePair > nameValuePairs = new ArrayList < NameValuePair > ();
                nameValuePairs.add(new BasicNameValuePair("jwt_token", jwtToken));
                nameValuePairs.add(new BasicNameValuePair("client_id", getCredentials.getCLIENT_ID()));
                nameValuePairs.add(new BasicNameValuePair("client_secret", getCredentials.getCLIENT_SECRET()));
                authPostRequest.setEntity(new UrlEncodedFormEntity(nameValuePairs, Consts.UTF_8));
                HttpResponse response;
                response = httpClient.execute(authServer, authPostRequest);
                StatusLine statusLine = response.getStatusLine();
                System.out.println("The status code is " + statusLine.getStatusCode());
                HttpEntity result = response.getEntity();
                String jsonResponseStr = EntityUtils.toString(result);
                System.out.println(jsonResponseStr);
                JsonReader jsonReader = new JsonReader(new StringReader(jsonResponseStr));
                JsonObject jsonObject = JsonParser.parseReader(jsonReader).getAsJsonObject();
                
                System.out.println("Returning access_token " + jsonObject.get("access_token").getAsString());
                return jsonObject.get("access_token").getAsString();

        } catch (Exception e) {
                System.out.print("Error: " + e.getMessage());
        }
        return "null";

}
```
