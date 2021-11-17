---
title: Navigieren in verschachtelten Bereichen
description: Navigieren in verschachtelten Bereichen
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9335
source-git-commit: 9529b1f6d1a863fc570822c8ecd6c4be01b36729
workflow-type: tm+mt
source-wordcount: '247'
ht-degree: 0%

---

# Navigationsregisterkarten mit mehreren Bedienfeldern

Wenn Ihr Formular die Navigationsregisterkarten verlassen hat und eine der Registerkarten mehrere Bedienfelder aufweist, können Sie den Titel der untergeordneten Bedienfelder ausblenden und dennoch zwischen den Registerkarten und den untergeordneten Bedienfeldern dieser Registerkarte navigieren

## Adaptives Formular erstellen

Erstellen Sie ein adaptives Formular mit der folgenden Struktur. Das Stammbedienfeld enthält untergeordnete Bedienfelder, die links als Registerkarten angezeigt werden. Einige dieser &quot;**Tabs**&quot; zusätzliche untergeordnete Bedienfelder haben. Die Registerkarte &quot;Familie&quot;verfügt beispielsweise über zwei untergeordnete Bedienfelder namens &quot;Ehegatte und Kinder&quot;.

Eine Symbolleiste wird auch unter dem FormContainer mit den Schaltflächen &quot;Vorschau&quot;und &quot;Weiter&quot;hinzugefügt

![toolbar-spacing](assets/multiple-panels.png)



Das Standardverhalten dieses Formulars besteht darin, alle Bedienfelder auf der linken Seite anzuzeigen und dann von einer Registerkarte zu einer anderen zu navigieren, wenn Sie auf die nächste Schaltfläche klicken.

Um dieses Standardverhalten zu ändern, müssen wir die folgenden Schritte ausführen

>[!VIDEO](https://video.tv.adobe.com/v/338369?quality=9&learn=on)


Fügen Sie den folgenden Code zum click -Ereignis der **Nächste** Schaltfläche mit dem Code-Editor

```javascript
window.guideBridge.setFocus(null, 'nextItemDeep', true);
```

Fügen Sie den folgenden Code zum click -Ereignis der **Prev** Schaltfläche mit dem Code-Editor

```javascript
window.guideBridge.setFocus(null, 'prevItemDeep', true);
```

Der obige Code hilft Ihnen bei der Navigation zwischen den Registerkarten und den untergeordneten Bedienfeldern der einzelnen Registerkarten.

## Überschrift der untergeordneten Bedienfelder ausblenden

Verwenden Sie den Stileditor, um den Titel der untergeordneten Registerkarten-Bedienfelder auszublenden.

>[!VIDEO](https://video.tv.adobe.com/v/338370?quality=9&learn=on)

>[!NOTE]
>
>Die in diesem Artikel beschriebene Funktion funktioniert auf der letzten Registerkarte nicht. Wenn beispielsweise die Registerkarte Adresse untergeordnete Bedienfelder enthält, funktioniert diese Funktion nicht.
