---
title: Zusammenführen von Daten mit der XDP-Vorlage
description: Stellen Sie mit den erforderlichen Parametern eine POST-Anfrage an den Endpunkt.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-8185
thumbnail: 332439.jpg
exl-id: d144b3f6-7c7a-46a7-bc5f-1767895749d0
duration: 46
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '129'
ht-degree: 100%

---

# Durchführen eines POST-Aufrufs


Der nächste Schritt besteht darin, einen HTTP-POST-Aufruf an den Endpunkt mit den erforderlichen Parametern durchzuführen. Die Vorlage und die Datendateien werden als Ressourcendateien bereitgestellt. Die Eigenschaften des generierten PDF-Dokuments werden über den Parameter der Option in der Anfrage angegeben. Die Eigenschaft „embedFonts“ wird verwendet, um benutzerdefinierte Schriftarten in das generierte PDF-Dokument einzubetten.[Befolgen Sie diese Dokumentation, um benutzerdefinierte Schriftarten für Ihre Forms-Cloud-Instanz bereitzustellen.](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/intellij-set-up.html?lang=de) Die Eigenschaften werden in der Ressourcendatei „options.json“ angegeben. Da der Endpunkt über eine Token-basierte Authentifizierung verfügt, übergeben wir das Zugriffs-Token im Anfrage-Header.

Der folgende Code wurde verwendet, um eine PDF durch Zusammenführen von Daten mit der Vorlage zu generieren

```java
public class DocumentGeneration
{
        public String SAVE_LOCATION = "c:\\aspire1";
        public void mergeDataWithXdpTemplate(String postURL)
        {
                HttpPost httpPost = new HttpPost(postURL);
                CredentialUtilites cu = new CredentialUtilites();
                String accessToken = cu.getAccessToken();
                httpPost.addHeader("Authorization", "Bearer " + accessToken);
                ClassLoader classLoader = DocumentGeneration.class.getClassLoader();
                URL templateFile = classLoader.getResource("templates/custom_fonts.xdp");
                File xdpTemplate = new File(templateFile.getPath());
                URL url = classLoader.getResource("datafiles");
                System.out.println(url.getPath());
                File files[] = new File(url.getPath()).listFiles();
                for (int i = 0; i < files.length; i++) {
                        MultipartEntityBuilder builder = MultipartEntityBuilder.create();
                        ContentType strContent = ContentType.create("text/plain", Charset.forName("UTF-8"));
                        builder.setMode(HttpMultipartMode.BROWSER_COMPATIBLE);
                        builder.addBinaryBody("data", files[i]);
                        builder.addBinaryBody("template", xdpTemplate);
                        builder.addBinaryBody("options",GetOptions.getPDFOptions().getBytes(),ContentType.APPLICATION_JSON,"options"
                        try {
                                HttpEntity entity = builder.build();
                                httpPost.setEntity(entity);
                                CloseableHttpClient httpclient = HttpClients.createDefault();
                                CloseableHttpResponse response = httpclient.execute(httpPost);
                                InputStream generatedPDF = response.getEntity().getContent();
                                byte[] bytes = IOUtils.toByteArray(generatedPDF);
                                File saveLocation = new File(SAVE_LOCATION);
                                if (!saveLocation.exists()) {
                                        saveLocation.mkdirs();
                                }
                                File outputFile = new File(SAVE_LOCATION+File.separator+files[i].getName().replace("xml", "pdf"));
                                try (FileOutputStream outputStream = new FileOutputStream(outputFile)) {
                                        outputStream.write(bytes);
                                }
                        } catch (Exception e) {
                                System.out.println("The error is " + e.getMessage());
                        }

                }
                System.out.println("Done generating " + files.length + " files");

        }

}
```
