---
icon: envelope
description: >-
  MessageData, MessageType und MessageContext sind die grundlegenden
  Datenstrukturen von surf-chat. Sie repräsentieren Nachrichten und ihren
  Verarbeitungskontext.
---

# Nachrichten

## MessageType

`MessageType` definiert den Empfängerkreis einer Nachricht.

```kotlin
@Serializable
@JvmInline
value class MessageType(val value: String) {
    companion object {
        val GLOBAL = MessageType("GLOBAL")   // Für alle Spieler sichtbar
        val TEAM   = MessageType("TEAM")     // Nur für Team-Mitglieder
        val DIRECT = MessageType("DIRECT")   // Direktnachricht zwischen zwei Spielern
    }
}
```

| Wert       | Beschreibung                                      |
| ---------- | ------------------------------------------------- |
| `GLOBAL`   | Nachricht ist für alle Spieler sichtbar           |
| `TEAM`     | Nachricht ist nur für Team-Mitglieder sichtbar    |
| `DIRECT`   | Direktnachricht zwischen zwei Spielern            |

---

## MessageData

`MessageData` enthält alle relevanten Informationen einer Nachricht.

```kotlin
@Serializable
data class MessageData(
    val message: Component,
    val messageUuid: UUID,
    val sender: UUID,
    val receiver: UUID?,
    val sentAt: OffsetDateTime,
    val server: String,
    val signature: SerializableSignature?,
    val type: MessageType
)
```

### Felder

| Feld           | Typ                       | Beschreibung                                                                    |
| -------------- | ------------------------- | ------------------------------------------------------------------------------- |
| `message`      | `Component`               | Nachrichteninhalt als Adventure-Component                                       |
| `messageUuid`  | `UUID`                    | Eindeutige ID dieser Nachricht                                                  |
| `sender`       | `UUID`                    | UUID des sendenden Spielers                                                     |
| `receiver`     | `UUID?`                   | UUID des Empfängers; `null` bei globalen oder Team-Nachrichten                  |
| `sentAt`       | `OffsetDateTime`          | Zeitstempel des Versands                                                        |
| `server`       | `String`                  | Name des Servers, von dem die Nachricht gesendet wurde                          |
| `signature`    | `SerializableSignature?`  | Kryptografische Signatur der Nachricht (von Minecraft); `null` bei unsignierten Nachrichten |
| `type`         | `MessageType`             | Typ der Nachricht (`GLOBAL`, `TEAM`, `DIRECT`)                                  |

### Hilfsmethoden

```kotlin
// Plaintext-Inhalt der Nachricht (lazy gecacht)
val plainText: String = messageData.plainMessage

// Sender-Spieler laden (Suspend)
val sender = messageData.senderUser()

// Empfänger-Spieler laden (Suspend, null wenn kein Empfänger)
val receiver = messageData.receiverUser()

// Kopie mit anderem Empfänger erstellen
val modified = messageData.withReceiver(newReceiverUuid)
```

{% hint style="info" %}
`senderUser()` und `receiverUser()` sind Suspend-Funktionen. Sie laden den Spieler aus dem surf-core-Cache oder der Datenbank.
{% endhint %}

---

## MessageContext

`MessageContext` ist der Verarbeitungskontext einer Nachricht, der durch die [Chat-Prozessoren](../funktionen/chat-processors.md) gereicht wird.

```kotlin
data class MessageContext(
    var messageData: MessageData,
    var isCancelled: Boolean,
    val viewers: MutableSet<Audience>,
    var render: MessageContextRenderer
)
```

### Felder

| Feld          | Typ                       | Beschreibung                                                          |
| ------------- | ------------------------- | --------------------------------------------------------------------- |
| `messageData` | `MessageData`             | Die zugehörigen Nachrichten-Daten (veränderbar)                       |
| `isCancelled` | `Boolean`                 | Gibt an, ob die Nachricht unterdrückt werden soll                     |
| `viewers`     | `MutableSet<Audience>`    | Alle Audiences, die die Nachricht erhalten sollen                     |
| `render`      | `MessageContextRenderer`  | Funktion, die den angezeigten Component für jeden Betrachter rendert  |

### `MessageContextRenderer`

Der Renderer ist eine Funktion, die für jeden Betrachter individuell den anzuzeigenden Component zurückgibt:

```kotlin
typealias MessageContextRenderer = (viewerUUID: UUID, viewAudience: Audience) -> Component
```

### Kontext bearbeiten

```kotlin
// Nachricht abbrechen (wird nicht gesendet)
context.cancel()

// Kontext per DSL bearbeiten
context.edit {
    messageData = messageData.withReceiver(someUuid)
    viewers.add(adminAudience)
}
```

### Beispiel: Nachricht für Admins mitlesen lassen

```kotlin
val adminProcessor = PostChatProcessor { context ->
    val admins: Audience = Bukkit.getOnlinePlayers()
        .filter { it.hasPermission("chat.spy") }
        .let { Audience.audience(it) }
    context.viewers.add(admins)
}
```

---

## Externe Links

* [GitHub – MessageData.kt](https://github.com/SLNE-Development/surf-chat/blob/version/26.1/surf-chat-api/src/main/kotlin/dev/slne/surf/chat/api/message/MessageData.kt)
* [GitHub – MessageContext.kt](https://github.com/SLNE-Development/surf-chat/blob/version/26.1/surf-chat-api/src/main/kotlin/dev/slne/surf/chat/api/message/MessageContext.kt)
* [GitHub – MessageType.kt](https://github.com/SLNE-Development/surf-chat/blob/version/26.1/surf-chat-api/src/main/kotlin/dev/slne/surf/chat/api/message/MessageType.kt)
