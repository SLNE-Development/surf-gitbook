---
icon: gears
description: >-
  Chat-Prozessoren ermöglichen es, Nachrichten vor (PreChatProcessor) und
  nach (PostChatProcessor) dem Versenden zu verarbeiten – z. B. um Inhalte
  zu filtern, Empfänger hinzuzufügen oder Aktionen auszulösen.
---

# Chat-Prozessoren

surf-chat unterscheidet zwei Arten von Prozessoren: **Pre-Prozessoren** laufen, bevor eine Nachricht sichtbar wird, **Post-Prozessoren** laufen danach. Beide erhalten einen [`MessageContext`](../grundlagen/nachrichten.md#messagecontext).

## PreChatProcessor

`PreChatProcessor` ermöglicht das Bearbeiten oder Abbrechen einer Nachricht **vor** dem Versenden.

```kotlin
interface PreChatProcessor {
    val order: Int

    fun process(context: MessageContext): MessageContext
    suspend fun processAsync(context: MessageContext): MessageContext = process(context)
}
```

### Felder & Methoden

| Element         | Beschreibung                                                                                     |
| --------------- | ------------------------------------------------------------------------------------------------ |
| `order`         | Reihenfolge der Ausführung. Niedrigere Werte werden zuerst ausgeführt.                           |
| `process`       | Synchrone Verarbeitungsfunktion. Muss implementiert werden.                                      |
| `processAsync`  | Asynchrone (Suspend-)Variante. Standardmäßig delegiert sie an `process`. Kann überschrieben werden. |

{% hint style="info" %}
Pre-Prozessoren werden nach ihrer `order`-Eigenschaft sortiert. Verwende niedrige Werte (z. B. `0`) für frühe Prüfungen wie Spam-Schutz und hohe Werte (z. B. `100`) für spätere Anpassungen.
{% endhint %}

### Nachricht abbrechen

Setze `isCancelled = true` im Kontext, um das Versenden der Nachricht zu verhindern:

```kotlin
object SpamCheckProcessor : PreChatProcessor {
    override val order = 10

    override fun process(context: MessageContext): MessageContext {
        if (isSpamming(context.messageData.sender)) {
            context.cancel()
        }
        return context
    }
}
```

### Nachricht bearbeiten

Ändere den Inhalt oder Empfängerkreis der Nachricht:

```kotlin
object PrefixProcessor : PreChatProcessor {
    override val order = 50

    override fun process(context: MessageContext): MessageContext = context.edit {
        val newRender: MessageContextRenderer = { _, _ ->
            buildText {
                primary("[Global] ")
                append(context.messageData.message)
            }
        }
        render = newRender
    }
}
```

### Registrierung

```kotlin
SurfChatApi.registerChatProcessor(SpamCheckProcessor)
SurfChatApi.registerChatProcessor(PrefixProcessor)
```

---

## PostChatProcessor

`PostChatProcessor` ist ein funktionales Interface, das **nach** dem Versenden ausgeführt wird. Es ist ideal für Logging, Statistiken oder das Weiterleiten an externe Systeme.

```kotlin
fun interface PostChatProcessor {
    suspend fun process(messageContext: MessageContext)
}
```

{% hint style="info" %}
`PostChatProcessor.process` ist eine Suspend-Funktion. Sie kann direkt auf Coroutine-Kontext und suspend-basierte APIs (z. B. Datenbankzugriffe) zugreifen.
{% endhint %}

### Beispiel: Nachrichten in einer Datenbank protokollieren

```kotlin
val logProcessor = PostChatProcessor { context ->
    if (!context.isCancelled) {
        database.logMessage(
            sender = context.messageData.sender,
            text   = context.messageData.plainMessage,
            type   = context.messageData.type
        )
    }
}

SurfChatApi.registerChatProcessor(logProcessor)
```

---

## ChatProcessorRegistry

Alle Prozessoren werden intern in der `ChatProcessorRegistry` verwaltet. Sie wird automatisch befüllt, wenn du Prozessoren über `SurfChatApi.registerChatProcessor(...)` registrierst.

```kotlin
// Alle registrierten Pre-Prozessoren
chatProcessorRegistry.preChatProcessors

// Alle registrierten Post-Prozessoren
chatProcessorRegistry.postChatProcessors
```

---

## Externe Links

* [GitHub – PreChatProcessor.kt](https://github.com/SLNE-Development/surf-chat/blob/version/26.1/surf-chat-api/src/main/kotlin/dev/slne/surf/chat/api/processor/PreChatProcessor.kt)
* [GitHub – PostChatProcessor.kt](https://github.com/SLNE-Development/surf-chat/blob/version/26.1/surf-chat-api/src/main/kotlin/dev/slne/surf/chat/api/processor/PostChatProcessor.kt)
* [GitHub – ChatProcessorRegistry.kt](https://github.com/SLNE-Development/surf-chat/blob/version/26.1/surf-chat-api/src/main/kotlin/dev/slne/surf/chat/api/processor/ChatProcessorRegistry.kt)
