---
title: OpenAI-Bildgenerierung über eine benutzerdefinierte Erweiterung der Inhaltsfragment-Konsole
description: Erfahren Sie, wie Sie mit OpenAI oder DALL-E 2 digitale Bilder aus einer Beschreibung in natürlicher Sprache generieren und generiertes Bild mit einer benutzerdefinierten Erweiterung der Inhaltsfragmentkonsole in AEM hochladen.
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11649
thumbnail: KT-11649.png
doc-type: article
last-substantial-update: 2023-01-04T00:00:00Z
exl-id: f3047f1d-1c46-4aee-9262-7aab35e9c4cb
source-git-commit: 6b5c755bd8fe6bbf497895453b95eb236f69d5f6
workflow-type: tm+mt
source-wordcount: '1396'
ht-degree: 3%

---

# Generieren von AEM-Bild-Assets mit OpenAI

Erfahren Sie, wie Sie mit OpenAI oder DALL.E 2 ein Bild generieren und es für Content Velocity in AEM DAM hochladen.

>[!VIDEO](https://video.tv.adobe.com/v/3413093?quality=12&learn=on)

Dieses Beispiel AEM die Erweiterung der Inhaltsfragmentkonsole ist eine [Aktionsleiste](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/) Erweiterung, die ein digitales Bild aus der Eingabe natürlicher Sprachen generiert, mithilfe von [OpenAI-API](https://openai.com/api/) oder [DALL.E 2](https://openai.com/dall-e-2/). Das generierte Bild wird in das AEM DAM hochgeladen und die Bildeigenschaft des ausgewählten Inhaltsfragments wird aktualisiert, um auf dieses neu generierte, hochgeladene Bild aus DAM zu verweisen.

In diesem Beispiel erfahren Sie Folgendes:

1. Generieren von Bildern mit [OpenAI-API](https://beta.openai.com/docs/guides/images/image-generation-beta) oder [DALL.E 2](https://openai.com/dall-e-2/)
2. Hochladen von Bildern in AEM
3. Aktualisierung der Inhaltsfragmenteigenschaft

Die Beispielerweiterung weist folgende Funktionen auf:

![Adobe I/O Runtime-Aktionsfluss für die digitale Bildgenerierung](./assets/digital-image-generation/flow.png){align="center"}

1. Wählen Sie Inhaltsfragment aus und klicken Sie auf die `Generate Image` im [Aktionsleiste](#extension-registration) öffnet die [modal](#modal).
1. Die [modal](#modal) zeigt ein benutzerdefiniertes Eingabeformular an, das mit [Reaktionsspektrum](https://react-spectrum.adobe.com/react-spectrum/).
1. Beim Senden des Formulars wird der angegebene Benutzer gesendet `Image Description` Text, das ausgewählte Inhaltsfragment und der AEM Host für [Benutzerdefinierte Adobe I/O Runtime-Aktion](#adobe-io-runtime-action).
1. Die [Adobe I/O Runtime-Aktion](#adobe-io-runtime-action) validiert die Eingaben.
1. Als Nächstes wird die OpenAI [Bildgenerierung](https://beta.openai.com/docs/guides/images/image-generation-beta) API und verwendet `Image Description` Text, um anzugeben, welches Bild generiert werden soll.
1. Die [Bildgenerierung](https://beta.openai.com/docs/guides/images/image-generation-beta) Endpunkt erstellt ein Originalbild der Größe _1024 x 1024_ Pixel, die den Parameterwert der Eingabeaufforderung verwenden, und gibt die generierte Bild-URL als Antwort zurück.
1. Die [Adobe I/O Runtime-Aktion](#adobe-io-runtime-action) lädt das generierte Bild zur App Builder-Laufzeit herunter.
1. Anschließend wird der Bild-Upload aus der App Builder-Laufzeit unter einem vordefinierten Pfad in DAM AEM.
1. Der AEM as a Cloud Service speichert das Bild im DAM und gibt Erfolgs- oder Fehlerantworten für die Adobe I/O Runtime-Aktion zurück. Die erfolgreiche Upload-Antwort aktualisiert den Bildeigenschaftswert des ausgewählten Inhaltsfragments mithilfe einer anderen HTTP-Anforderung, die über die Adobe I/O Runtime-Aktion AEM werden soll.
1. Das Modal empfängt die Antwort von der Adobe I/O Runtime-Aktion und stellt AEM Asset-Detaillink des neu generierten, hochgeladenen Bildes bereit.

## Erweiterungspunkt

Dieses Beispiel erstreckt sich auf den Erweiterungspunkt `actionBar` , um der Inhaltsfragmentkonsole eine benutzerdefinierte Schaltfläche hinzuzufügen.

| AEM Benutzeroberfläche erweitert | Erweiterungspunkt |
| ------------------------ | --------------------- | 
| [Inhaltsfragment-Konsole](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/?lang=de) | [Aktionsleiste](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/) |

## Beispielerweiterung

Im Beispiel wird ein vorhandenes Adobe Developer Console-Projekt und die folgenden Optionen verwendet, wenn die App Builder-App über initialisiert wird `aio app init`.

+ Nach welchen Vorlagen suchen Sie?: `All Extension Points`
+ Wählen Sie die zu installierenden Vorlagen aus:` @adobe/aem-cf-admin-ui-ext-tpl`
+ Wie soll Ihre Erweiterung benannt werden?: `Image generation`
+ Geben Sie eine kurze Beschreibung Ihrer Erweiterung an: `An example action bar extension that generates an image using OpenAI and uploads it to AEM DAM.`
+ Mit welcher Version möchten Sie beginnen?: `0.0.1`
+ Was möchten Sie als Nächstes tun?
   + `Add a custom button to Action Bar`
      + Geben Sie den Beschriftungsnamen für die Schaltfläche an: `Generate Image`
      + Müssen Sie ein Modal für die Schaltfläche anzeigen? `y`
   + `Add server-side handler`
      + Mit Adobe I/O Runtime können Sie bei Bedarf Server-losen Code aufrufen. Wie möchten Sie diese Aktion benennen: `generate-image`

Die generierte App Builder-Erweiterung wird wie unten beschrieben aktualisiert.

### Ersteinrichtung

1. Registrieren Sie sich kostenlos . [OpenAI-API](https://openai.com/api/) und erstellen Sie ein [API-Schlüssel](https://beta.openai.com/account/api-keys)
1. Fügen Sie diesen Schlüssel zum App Builder-Projekt hinzu. `.env` file

   ```
       # Specify your secrets here
       # This file must not be committed to source control
       ## Adobe I/O Runtime credentials
       ...
       AIO_runtime_apihost=https://adobeioruntime.net
       ...
       # OpenAI secret API key
       OPENAI_API_KEY=my-openai-secrete-key-to-generate-images
       ...
   ```

1. Pass `OPENAI_API_KEY` als Parameter für die Adobe I/O Runtime-Aktion aktualisieren Sie die `src/aem-cf-console-admin-1/ext.config.yaml`

   ```yaml
       ...
   
       runtimeManifest:
         packages:
           aem-cf-console-admin-1:
             license: Apache-2.0
             actions:
               generate-image:
                 function: actions/generate-image/index.js
                 web: 'yes'
                 runtime: nodejs:16
                 inputs:
                   LOG_LEVEL: debug
                   OPENAI_API_KEY: $OPENAI_API_KEY
       ...
   ```

1. Installieren unter Node.js-Bibliotheken
   1. [Die OpenAI Node.js-Bibliothek](https://github.com/openai/openai-node#installation) - um die OpenAI-API einfach aufzurufen
   1. [AEM-Upload](https://github.com/adobe/aem-upload#install) - um Bilder in AEM-CS-Instanzen hochzuladen.


>[!TIP]
>
>In den folgenden Abschnitten erfahren Sie mehr über die wichtigsten JavaScript-Dateien für React- und Adobe I/O Runtime-Aktionsaktionen. Für Ihre Referenz verwenden Sie die Schlüsseldateien aus `web-src` und  `actions` Ordner des AppBuilder-Projekts bereitgestellt werden, siehe [adobe-appbuilder-cfc-ext-image-generation-code.zip](./assets/digital-image-generation/adobe-appbuilder-cfc-ext-image-generation-code.zip).


### App-Routen{#app-routes}

Die `src/aem-cf-console-admin-1/web-src/src/components/App.js` enthält die [React-Router](https://reactrouter.com/en/main).

Es gibt zwei logische Routen:

1. Die erste Route ordnet Anforderungen der `index.html`, die die für die [Erweiterungsregistrierung](#extension-registration).

   ```javascript
   <Route index element={<ExtensionRegistration />} />
   ```

1. Der zweite Satz von Routen ordnet URLs React-Komponenten zu, die den Inhalt des Modals der Erweiterung rendern. Die `:selection` param steht für einen durch Trennzeichen getrennten Listeninhaltsfragmentpfad.

   Wenn die Erweiterung über mehrere Schaltflächen zum Aufrufen diskreter Aktionen verfügt, wird jede [Erweiterungsregistrierung](#extension-registration) zugeordnet zu einer hier definierten Route.

   ```javascript
   <Route
       exact path="content-fragment/:selection/generate-image-modal"
       element={<GenerateImageModal />}
       />
   ```

### Registrierung der Erweiterung

`ExtensionRegistration.js`, die der `index.html` route ist der Einstiegspunkt für die AEM-Erweiterung und definiert:

1. Der Speicherort der Erweiterungsschaltfläche wird im AEM Authoring-Erlebnis (`actionBar` oder `headerMenu`)
1. Die Definition der Erweiterungsschaltfläche in `getButton()` function
1. Der Klick-Handler für die Schaltfläche im `onClick()` function

+ `src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButton() {
            return {
              'id': 'generate-image',     // Unique ID for the button
              'label': 'Generate Image',  // Button label 
              'icon': 'PublishCheck'      // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },

          // Click handler for the extension button
          onClick(selections) {
            // Collect the selected content fragment paths 
            const selectionIds = selections.map(selection => selection.id);

            // Create a URL that maps to the 
            const modalURL = "/index.html#" + generatePath(
              "/content-fragment/:selection/generate-image-modal",
              {
                // Set the :selection React route parameter to an encoded, delimited list of paths of the selected content fragments
                selection: encodeURIComponent(selectionIds.join('|')),
              }
            );

            // Open the route in the extension modal using the constructed URL
            guestConnection.host.modal.showUrl({
              title: "Generate Image",
              url: modalURL
            })
          }
        },

      }
    })
  }
  init().catch(console.error)
```

### Modal

Jede Route der Erweiterung, wie in [`App.js`](#app-routes), wird einer React-Komponente zugeordnet, die im Modal der Erweiterung gerendert wird.

In dieser Beispielanwendung gibt es eine modale React-Komponente (`GenerateImageModal.js`) mit vier Status:

1. Laden, was angibt, dass der Benutzer warten muss
1. Die Warnmeldung, die den Benutzern nahe legt, jeweils nur ein Inhaltsfragment auszuwählen
1. Das Formular Bild generieren , über das der Benutzer eine Bildbeschreibung in der natürlichen Sprache bereitstellen kann.
1. Die Antwort des Bildgenerierungsvorgangs, die den Link AEM Asset-Details des neu generierten, hochgeladenen Bildes bereitstellt.

Wichtig: Jede Interaktion mit AEM aus der Erweiterung sollte an eine [AppBuilder Adobe I/O Runtime-Aktion](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/), bei dem es sich um einen separaten Server-losen Prozess handelt, der in ausgeführt wird. [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/).
Die Verwendung von Adobe I/O Runtime-Aktionen zur Kommunikation mit AEM und die Vermeidung von CORS-Verbindungsproblemen (Cross Origin Resource Sharing).

Wenn die _Bild generieren_ Formular gesendet wird, eine benutzerdefinierte `onSubmitHandler()` Ruft die Adobe I/O Runtime-Aktion auf und übergibt die Bildbeschreibung, den aktuellen AEM-Host (Domäne) und das AEM Zugriffstoken des Benutzers. Die Aktion ruft dann die [Bildgenerierung](https://beta.openai.com/docs/guides/images/image-generation-beta) API zum Generieren eines Bildes mithilfe der gesendeten Bildbeschreibung. Nächste Verwendung [AEM-Upload](https://github.com/adobe/aem-upload) Knotenmodul `DirectBinaryUpload` -Klasse das generierte Bild in AEM hochgeladen und verwendet schließlich [AEM Inhaltsfragment-API](https://experienceleague.adobe.com/docs/experience-manager-65/assets/extending/assets-api-content-fragments.html?lang=de) , um die Inhaltsfragmente zu aktualisieren.

Wenn die Antwort von der Adobe I/O Runtime-Aktion empfangen wird, wird das Modal aktualisiert, um die Ergebnisse des Bildgenerierungsvorgangs anzuzeigen.

+ `src/aem-cf-console-admin-1/web-src/src/components/GenerateImageModal.js`

```javascript
export default function GenerateImageModal() {
  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the application state
  const [imageDescription, setImageDescription] = useState(null);
  const [validationState, setValidationState] = useState({});

  const [actionInvokeInProgress, setActionInvokeInProgress] = useState(false);
  const [actionResponse, setActionResponse] = useState();

  // Get the selected content fragment paths from the route parameter `:selection`
  const { selection } = useParams();
  const fragmentIds = selection?.split('|') || [];

  console.log('Selected Fragment Ids', fragmentIds);

  if (!fragmentIds || fragmentIds.length === 0) {
    console.error('The Content Fragments are not selected, can NOT generate images');
    return;
  }

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Determine view to display in the modal
  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />;
  } if (actionInvokeInProgress) {
    // If the 'Generate Image' action has been invoked but not completed, display a loading spinner
    return <Spinner />;
  } if (fragmentIds.length > 1) {
    // If more than one CF selected show warning and suggest to select only one CF
    return renderMoreThanOneCFSelectionError();
  } if (fragmentIds.length === 1 && !actionResponse) {
    // Display the 'Generate Image' modal and ask for image description
    return renderImgGenerationForm();
  } if (actionResponse) {
    // If the 'Generate Image' actio has completed, display the response
    return renderActionResponse();
  }

  /**
   * Renders the message suggesting to select only on CF at a time to not lose credits accidentally
   *
   * @returns the suggestion or error message to select one CF at a time
   */
  function renderMoreThanOneCFSelectionError() {
    return (
      <Provider theme={defaultTheme} colorScheme="light">
        <Content width="100%">
          <Text>
            As this operation
            <strong> uses credits from Generative AI services</strong>
            {' '}
            such as DALL.E 2 (or Stable Dufusion), we allow only one Generate Image at a time.
            <p />
            <strong>So please select only one Content Fragment at this moment.</strong>
          </Text>

          <Flex width="100%" justifyContent="end" alignItems="center" marginTop="size-400">
            <ButtonGroup align="end">
              <Button variant="negative" onPress={() => guestConnection.host.modal.close()}>Close</Button>
            </ButtonGroup>
          </Flex>

        </Content>
      </Provider>
    );
  }

  /**
   * Renders the form asking for image description in the natural language and
   * displays message this action uses credits from Generative AI services.
   *
   *
   * @returns the image description input field and credit usage message
   */
  function renderImgGenerationForm() {
    return (

      <Provider theme={defaultTheme} colorScheme="light">
        <Content width="100%">

          <Flex width="100%">
            <Form
              width="100%"
            >
              <TextField
                label="Image Description"
                description="The image description in natural language, for e.g. Alaskan adventure in wilderness, animals, and flowers."
                isRequired
                validationState={validationState?.propertyName}
                onChange={setImageDescription}
                contextualHelp={(
                  <ContextualHelp>
                    <Heading>Need help?</Heading>
                    <Content>
                      <Text>
                        The
                        <strong>description of an image</strong>
                        {' '}
                        you are looking for in the natural language, for e.g. &quot;Family vacation on the beach with blue ocean, dolphins, boats and drink&quot;
                      </Text>
                    </Content>
                  </ContextualHelp>
                  )}
              />

              <Text>
                <p />
                Please note this will use credits from Generative AI services such as OpenAI/DALL.E 2. The AI-generated images are saved to this AEM as a Cloud Service Author service using logged user access (IMS) token.
              </Text>

              <ButtonGroup align="end">
                <Button variant="accent" onPress={onSubmitHandler}>Use Credits</Button>
                <Button variant="accent" onPress={() => guestConnection.host.modal.close()}>Close</Button>
              </ButtonGroup>
            </Form>
          </Flex>

        </Content>
      </Provider>

    );
  }

  function buildAssetDetailsURL(aemImgURL) {
    const urlParts = aemImgURL.split('.com');
    const aemAssetdetailsURL = `${urlParts[0]}.com/ui#/aem/assetdetails.html${urlParts[1]}`;

    return aemAssetdetailsURL;
  }

  /**
   * Displays the action response received from the App Builder
   *
   * @returns Displays App Builder action and details
   */
  function renderActionResponse() {
    return (
      <Provider theme={defaultTheme} colorScheme="light">
        <Content width="100%">

          {actionResponse.status === 'success'
            && (
              <>
                <Heading level="4">
                  Successfully generated an image, uploaded it to this AEM-CS Author service, and associated it to the selected Content Fragment.
                </Heading>

                <Text>
                  {' '}
                  Please see generated image in AEM-CS
                  {' '}
                  <Link>
                    <a href={buildAssetDetailsURL(actionResponse.aemImgURL)} target="_blank" rel="noreferrer">
                      here.
                    </a>
                  </Link>
                </Text>
              </>
            )}

          {actionResponse.status === 'failure'
            && (
            <Heading level="4">
              Failed to generate, upload image, please check App Builder logs.
            </Heading>
            )}

          <Flex width="100%" justifyContent="end" alignItems="center" marginTop="size-400">
            <ButtonGroup align="end">
              <Button variant="negative" onPress={() => guestConnection.host.modal.close()}>Close</Button>
            </ButtonGroup>
          </Flex>

        </Content>
      </Provider>
    );
  }

  /**
   * Handle the Generate Image form submission.
   * This function calls the supporting Adobe I/O Runtime actions such as
   * - Call the Generative AI service (DALL.E) with 'image description' to generate an image
   * - Download the AI generated image to App Builder runtime
   * - Save the downloaded image to AEM DAM and update Content Fragment's image reference property to use this new image
   *
   * When invoking the Adobe I/O Runtime actions, the following parameters are passed as they're used by the action to connect to AEM:
   * - AEM Host to connect to
   * - AEM access token to connect to AEM with
   * - The Content Fragment path to update
   *
   * @returns In case of success the updated content fragment, otherwise failure message
   */
  async function onSubmitHandler() {
    console.log('Started Image Generation orchestration');

    // Validate the form input fields
    if (imageDescription?.length > 1) {
      setValidationState({ imageDescription: 'valid' });
    } else {
      setValidationState({ imageDescription: 'invalid' });
      return;
    }

    // Mark the extension as invoking the action, so the loading spinner is displayed
    setActionInvokeInProgress(true);

    // Set the HTTP headers to access the Adobe I/O runtime action
    const headers = {
      Authorization: `Bearer ${guestConnection.sharedContext.get('auth').imsToken}`,
      'x-gw-ims-org-id': guestConnection.sharedContext.get('auth').imsOrg,
    };

    // Set the parameters to pass to the Adobe I/O Runtime action
    const params = {

      aemHost: `https://${guestConnection.sharedContext.get('aemHost')}`,

      fragmentId: fragmentIds[0],
      imageDescription,
    };

    const generateImageAction = 'generate-image';

    try {
      const generateImageActionResponse = await actionWebInvoke(allActions[generateImageAction], headers, params);

      // Set the response from the Adobe I/O Runtime action
      setActionResponse(generateImageActionResponse);

      console.log(`Response from ${generateImageAction}:`, actionResponse);
    } catch (e) {
      // Log and store any errors
      console.error(e);
    }

    // Set the action as no longer being invoked, so the loading spinner is hidden
    setActionInvokeInProgress(false);
  }
}
```

>[!NOTE]
>
>Im `buildAssetDetailsURL()` -Funktion `aemAssetdetailsURL` Variablenwert geht davon aus, dass [Unified Shell](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/aem-cloud-service-on-unified-shell.html#overview) aktiviert ist. Wenn Sie die einheitliche Shell deaktiviert haben, müssen Sie die `/ui#/aem` aus dem Variablenwert.


### Adobe I/O Runtime-Aktion

Eine AEM App Builder-App kann 0 oder viele Adobe I/O Runtime-Aktionen definieren oder verwenden.
Die Adobe Runtime-Aktion ist für Arbeiten verantwortlich, die die Interaktion mit AEM, Adobe oder Webdiensten von Drittanbietern erfordern.

In dieser Beispielanwendung wird die `generate-image` Adobe I/O Runtime-Aktion ist für Folgendes zuständig:

1. Erstellen eines Bildes mit [OpenAI-API-Bildgenerierung](https://beta.openai.com/docs/guides/images/image-generation-beta) service
1. Hochladen des generierten Bildes in die AEM-CS-Instanz mithilfe von [AEM-Upload](https://github.com/adobe/aem-upload) Bibliothek
1. Senden einer HTTP-Anfrage an die AEM Content Fragment-API, um die Bildeigenschaft des Inhaltsfragments zu aktualisieren.
1. Ausgabe der wichtigsten Informationen zu Erfolgen und Fehlern bei der Anzeige durch das Modal (`GenerateImageModal.js`)


#### Einstiegspunkt (`index.js`)

Die `index.js` orchestriert über 1 bis 3 Aufgaben mithilfe der jeweiligen JavaScript-Module, d. h. `generate-image-using-openai, upload-generated-image-to-aem, update-content-fragement`. Diese Module und der zugehörige Code werden im nächsten Abschnitt beschrieben. [Unterabschnitte](#image-generation-module---generate-image-using-openaijs).

+ `src/aem-cf-console-admin-1/actions/generate-image/index.js`

```javascript
/**
 *
 * This action orchestrates an image generation by calling the OpenAI API (DALL.E 2) and saves generated image to AEM.
 *
 * It leverages following modules
 *  - 'generate-image-using-openai' - To generate an image using OpenAI API
 *  - 'upload-generated-image-to-aem' - To upload the generated image into AEM-CS instance
 *  - 'update-content-fragement' - To update the CF image property with generated image's DAM path
 *
 */

const { Core } = require('@adobe/aio-sdk');
const {
  errorResponse, stringParameters, getBearerToken, checkMissingRequestInputs,
} = require('../utils');

const { generateImageUsingOpenAI } = require('./generate-image-using-openai');

const { uploadGeneratedImageToAEM } = require('./upload-generated-image-to-aem');

const { updateContentFragmentToUseGeneratedImg } = require('./update-content-fragement');

// main function that will be executed by Adobe I/O Runtime
async function main(params) {
  // create a Logger
  const logger = Core.Logger('main', { level: params.LOG_LEVEL || 'info' });

  try {
    // 'info' is the default level if not set
    logger.info('Calling the main action');

    // log parameters, only if params.LOG_LEVEL === 'debug'
    logger.debug(stringParameters(params));

    // check for missing request input parameters and headers
    const requiredParams = ['aemHost', 'fragmentId', 'imageDescription'];
    const requiredHeaders = ['Authorization'];
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders);

    if (errorMessage) {
      // return and log client errors
      return errorResponse(400, errorMessage, logger);
    }

    // extract the user Bearer token from the Authorization header
    const token = getBearerToken(params);

    // Call OpenAI (DALL.E 2) API to generate an image using image description
    const generatedImageURL = await generateImageUsingOpenAI(params);
    logger.info(`Generated image using OpenAI API and url is : ${generatedImageURL}`);

    // Upload the generated image to AEM-CS
    const uploadedImagePath = await uploadGeneratedImageToAEM(params, generatedImageURL, token);
    logger.info(`Uploaded image to AEM, path is: ${uploadedImagePath}`);

    // Update Content Fragment with the newly generated image reference
    const updateContentFragmentPath = await updateContentFragmentToUseGeneratedImg(params, uploadedImagePath, token);
    logger.info(`Updated Content Fragment path is: ${updateContentFragmentPath}`);

    let result;
    if (updateContentFragmentPath) {
      result = {
        status: 'success', message: 'Successfully generated and uploaded image to AEM', genTechServiceImageURL: generatedImageURL, aemImgURL: uploadedImagePath, fragmentPath: updateContentFragmentPath,
      };
    } else {
      result = { status: 'failure', message: 'Failed to generated and uploaded image, please check App Builder logs' };
    }

    const response = {
      statusCode: 200,
      body: result,
    };

    logger.info('Adobe I/O Runtime action response', response);

    // Return the response to the caller
    return response;
  } catch (error) {
    // log any server errors
    logger.error(error);
    // return with 500
    return errorResponse(500, 'server error', logger);
  }
}

exports.main = main;
```

#### Bildgenerierung

Dieses Modul ist für das Aufrufen des [Bildgenerierung](https://beta.openai.com/docs/guides/images/image-generation-beta) Endpunkt mit [openai](https://github.com/openai/openai-node) -Bibliothek. So rufen Sie den geheimen OpenAI-API-Schlüssel ab, der im `.env` -Datei verwendet `params.OPENAI_API_KEY`.

+ `src/aem-cf-console-admin-1/actions/generate-image/generate-image-using-openai.js`

```javascript
/**
 * This module calls OpenAI API to generate an image based on image description provided to Action
 *
 */

const { Configuration, OpenAIApi } = require('openai');

const { Core } = require('@adobe/aio-sdk');

// Placeholder than actual OpenAI Image
const PLACEHOLDER_IMG_URL = 'https://www.gstatic.com/webp/gallery/2.png';

async function generateImageUsingOpenAI(params) {
  // create a Logger
  const logger = Core.Logger('generateImageUsingOpenAI', { level: params.LOG_LEVEL || 'info' });

  let generatedImageURL = PLACEHOLDER_IMG_URL;

  // create configuration object with the API Key
  const configuration = new Configuration({
    apiKey: params.OPENAI_API_KEY,
  });

  // create OpenAIApi object
  const openai = new OpenAIApi(configuration);

  logger.info(`Generating image for input: ${params.imageDescription}`);

  try {
    // invoke createImage method with details
    const response = await openai.createImage({
      prompt: params.imageDescription,
      n: 1,
      size: '1024x1024',
    });

    generatedImageURL = response.data.data[0].url;

    logger.info(`The OpenAI generate image url is: ${generatedImageURL}`);
  } catch (error) {
    logger.error(`Error while generating image, details are: ${error}`);
  }

  return generatedImageURL;
}

module.exports = {
  generateImageUsingOpenAI,
};
```

#### Hochladen in AEM

Dieses Modul ist für das Hochladen des OpenAI-generierten Bildes in AEM verantwortlich, indem [AEM-Upload](https://github.com/adobe/aem-upload) -Bibliothek. Das generierte Bild wird zunächst mithilfe von Node.js in die App Builder-Laufzeitumgebung heruntergeladen [Dateisystem](https://nodejs.org/api/fs.html) und nach Abschluss des Uploads in AEM wird sie gelöscht.

In unten stehendem Code `uploadGeneratedImageToAEM` -Funktion orchestriert generierte Bilddownloads zur Laufzeit, lädt sie in AEM hoch und löscht sie aus der Laufzeit. Das Bild wird in die `/content/dam/wknd-shared/en/generated` Pfad, stellen Sie sicher, dass alle Ordner im DAM vorhanden sind, dessen Voraussetzung für die Verwendung ist. [AEM-Upload](https://github.com/adobe/aem-upload) -Bibliothek.

+ `src/aem-cf-console-admin-1/actions/generate-image/upload-generated-image-to-aem.js`

```javascript
/**
 * This module uploads the generated image to AEM-CS instance using current user's IMS token
 *
 */

const { Core } = require('@adobe/aio-sdk');
const fs = require('fs');

const {
  DirectBinaryUploadErrorCodes,
  DirectBinaryUpload,
  DirectBinaryUploadOptions,
} = require('@adobe/aem-upload');

const codes = DirectBinaryUploadErrorCodes;
const IMG_EXTENSION = '.png';

const GENERATED_IMAGES_DAM_PATH = '/content/dam/wknd-shared/en/generated';

async function downloadImageToRuntime(logger, generatedImageURL) {
  logger.log('Downloading generated image to the runtime');

  // placeholder image name
  let generatedImageName = 'generated.png';

  try {
    // Get the generated image name from the image URL
    const justImgURL = generatedImageURL.substring(0, generatedImageURL.indexOf(IMG_EXTENSION) + 4);
    generatedImageName = justImgURL.substring(justImgURL.lastIndexOf('/') + 1);

    // Read image from URL as the buffer
    const response = await fetch(generatedImageURL);
    const buffer = await response.buffer();

    // Write/download image to the runtime
    fs.writeFileSync(generatedImageName, buffer, (err) => {
      if (err) throw err;
      logger.log('Saved the generated image!');
    });
  } catch (error) {
    logger.error(`Error while downloading image on the runtime, details are: ${error}`);
  }

  return generatedImageName;
}

function setupEventHandlers(binaryUpload, logger) {
  binaryUpload.on('filestart', (data) => {
    const { fileName } = data;

    logger.log(`Started file upload ${fileName}`);
  });

  binaryUpload.on('fileprogress', (data) => {
    const { fileName, transferred } = data;

    logger.log(`Fileupload is in progress ${fileName} & ${transferred}`);
  });

  binaryUpload.on('fileend', (data) => {
    const { fileName } = data;

    logger.log(`Finished file upload ${fileName}`);
  });

  binaryUpload.on('fileerror', (data) => {
    const { fileName, errors } = data;

    logger.log(`Error in file upload ${fileName} and ${errors}`);
  });
}

async function getImageSize(downloadedImgName) {
  const stats = fs.statSync(downloadedImgName);
  return stats.size;
}

async function uploadImageToAEMFromRuntime(logger, aemURL, downloadedImgName, accessToken) {
  let aemImageURL;
  try {
    logger.log('Uploading generated image to AEM from the runtime');

    const binaryUpload = new DirectBinaryUpload();

    // setup event handlers to track the progress, success or error
    setupEventHandlers(binaryUpload, logger);

    // get downloaded image size
    const imageSize = await getImageSize(downloadedImgName);
    logger.info(`The image upload size is: ${imageSize}`);

    // The deatils of the file to be uploaded
    const uploadFiles = [
      {
        fileName: downloadedImgName, // name of the file as it will appear in AEM
        fileSize: imageSize, // total size, in bytes, of the file
        filePath: downloadedImgName, // Full path to the local file
      },
    ];

    // Provide AEM URL and DAM Path where images will be uploaded
    const options = new DirectBinaryUploadOptions()
      .withUrl(`${aemURL}${GENERATED_IMAGES_DAM_PATH}`)
      .withUploadFiles(uploadFiles);

    // Add headers like content type and authorization
    options.withHeaders({
      'content-type': 'image/png',
      Authorization: `Bearer ${accessToken}`,
    });

    // Start the upload to AEM
    await binaryUpload.uploadFiles(options)
      .then((result) => {
        // Handle Error
        result.getErrors().forEach((error) => {
          if (error.getCode() === codes.ALREADY_EXISTS) {
            logger.error('The generated image already exists');
          }
        });

        // Handle Upload result and check for errors
        result.getFileUploadResults().forEach((fileResult) => {
          // log file upload result
          logger.info(`File upload result ${JSON.stringify(fileResult)}`);

          fileResult.getErrors().forEach((fileErr) => {
            if (fileErr.getCode() === codes.ALREADY_EXISTS) {
              const fileName = fileResult.getFileName();
              logger.error(`The generated image already exists ${fileName}`);
            }
          });
        });
      })
      .catch((err) => {
        logger.info(`Failed to uploaded generated image to AEM${err}`);
      });

    logger.info('Successfully uploaded generated image to AEM');

    aemImageURL = `${aemURL + GENERATED_IMAGES_DAM_PATH}/${downloadedImgName}`;
  } catch (error) {
    logger.info(`Error while uploading generated image to AEM, see ${error}`);
  }

  return aemImageURL;
}

async function deleteFileFromRuntime(logger, downloadedImgName) {
  try {
    logger.log('Deleting the generated image from the runtime');

    fs.unlinkSync(downloadedImgName);

    logger.log('Successfully deleted the generated image from the runtime');
  } catch (error) {
    logger.error(`Error while deleting generated image from the runtime, details are: ${error}`);
  }
}

async function uploadGeneratedImageToAEM(params, generatedImageURL, accessToken) {
  // create a Logger
  const logger = Core.Logger('uploadGeneratedImageToAEM', { level: params.LOG_LEVEL || 'info' });

  const aemURL = params.aemHost;

  logger.info(`Uploading generated image from ${generatedImageURL} to AEM ${aemURL} by streaming the bytes.`);

  // download image to the App Builder runtime
  const downloadedImgName = await downloadImageToRuntime(logger, generatedImageURL);

  // Upload image to AEM from the App Builder runtime
  const aemImageURL = await uploadImageToAEMFromRuntime(logger, aemURL, downloadedImgName, accessToken);

  // Delete the downloaded image from the App Builder runtime
  await deleteFileFromRuntime(logger, downloadedImgName);

  return aemImageURL;
}

module.exports = {
  uploadGeneratedImageToAEM,
};
```

#### Inhaltsfragment aktualisieren

Dieses Modul ist für die Aktualisierung der Bildeigenschaft des angegebenen Inhaltsfragments mit dem DAM-Pfad des neu hochgeladenen Bildes mithilfe der AEM Inhaltsfragment-API verantwortlich.

+ `src/aem-cf-console-admin-1/actions/generate-image/update-content-fragement.js`

```javascript
/**
 * This module updates the CF image property with generated image's DAM path
 *
 */
const { Core } = require('@adobe/aio-sdk');

const ADVENTURE_MODEL_IMG_PROPERTY_NAME = 'primaryImage';

const ARTICLE_MODEL_IMG_PROPERTY_NAME = 'featuredImage';

const AUTHOR_MODEL_IMG_PROPERTY_NAME = 'profilePicture';

function findImgPropertyName(fragmenPath) {
  if (fragmenPath && fragmenPath.includes('/adventures')) {
    return ADVENTURE_MODEL_IMG_PROPERTY_NAME;
  } if (fragmenPath && fragmenPath.includes('/magazine')) {
    return ARTICLE_MODEL_IMG_PROPERTY_NAME;
  }
  return AUTHOR_MODEL_IMG_PROPERTY_NAME;
}

async function updateContentFragmentToUseGeneratedImg(params, uploadedImagePath, accessToken) {
  // create a Logger
  const logger = Core.Logger('updateContentFragment', { level: params.LOG_LEVEL || 'info' });

  const fragmenPath = params.fragmentId;
  const imgPropName = findImgPropertyName(fragmenPath);
  const relativeImgPath = uploadedImagePath.substring(uploadedImagePath.indexOf('/content/dam'));

  logger.info(`Update CF ${fragmenPath} to use ${relativeImgPath} image path`);

  const body = {
    properties: {
      elements: {
        [imgPropName]: {
          value: relativeImgPath,
        },
      },
    },
  };

  const res = await fetch(`${params.aemHost}${fragmenPath.replace('/content/dam/', '/api/assets/')}.json`, {
    method: 'put',
    body: JSON.stringify(body),
    headers: {
      Authorization: `Bearer ${accessToken}`,
      'Content-Type': 'application/json',
    },

  });

  if (res.ok) {
    logger.info(`Successfully updated ${fragmenPath}`);
    return fragmenPath;
  }

  logger.info(`Failed to update ${fragmenPath}`);
  return '';
}

module.exports = {
  updateContentFragmentToUseGeneratedImg,
};
```
