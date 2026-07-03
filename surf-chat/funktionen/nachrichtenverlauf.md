---
icon: clock-rotate-left
description: >-
  Der Nachrichtenverlauf speichert alle gesendeten Nachrichten persistent und
  ermöglicht eine gefilterte Suche über HistoryFilter und HistoryEntry.
---

# Nachrichtenverlauf

surf-chat speichert alle gesendeten Nachrichten persistent. Über `SurfChatApi.lookupHistory(...)` lässt sich der Verlauf gefiltert abrufen.

## HistoryEntry

`HistoryEntry` repräsentiert eine gespeicherte Nachricht im Verlauf.

```kotlin
@Serializable
data class HistoryEntry(
    val messageUuid: SerializableUUID,
    val senderUuid: SerializableUUID,
    val receiverUuid: SerializableUUID?,
    val messageType: MessageType,
    val sentAt: SerializableOffsetDateTime,
    val message: String,
    val server: String,
    val deleted: Boolean,
    val deletedAt: SerializableOffsetDateTime?,
    val deletedBy: SerializableUUID?,
    val deletionReason: String?
)
```

### Felder

| Feld              | Typ                          | Beschreibung                                                        |
| ----------------- | ---------------------------- | ------------------------------------------------------------------- |
| `messageUuid`     | `UUID`                       | Eindeutige ID der Nachricht                                         |
| `senderUuid`      | `UUID`                       | UUID des Absenders                                                  |
| `receiverUuid`    | `UUID?`                      | UUID des Empfängers; `null` bei globalen/Team-Nachrichten           |
| `messageType`     | `MessageType`                | Typ der Nachricht (`GLOBAL`, `TEAM`, `DIRECT`)                      |
| `sentAt`          | `OffsetDateTime`             | Zeitstempel des Versands                                            |
| `message`         | `String`                     | Nachrichteninhalt als Klartext                                      |
| `server`          | `String`                     | Name des Servers, von dem die Nachricht gesendet wurde              |
| `deleted`         | `Boolean`                    | `true`, wenn die Nachricht gelöscht wurde                           |
| `deletedAt`       | `OffsetDateTime?`            | Zeitstempel der Löschung (oder `null`)                              |
| `deletedBy`       | `UUID?`                      | UUID des Moderators, der gelöscht hat (oder `null`)                 |
| `deletionReason`  | `String?`                    | Optionaler Löschgrund                                               |

### Hilfsmethoden

```kotlin
// Sender als SurfPlayer laden (Suspend)
val sender = entry.sender()

// Empfänger als SurfPlayer laden (Suspend, null wenn kein Empfänger)
val receiver = entry.receiver()
```

---

## HistoryFilter

Mit `HistoryFilter` steuerst du, welche Nachrichten zurückgegeben werden. Alle Felder sind optional – `null` bedeutet „kein Filter für dieses Feld".

```kotlin
@Serializable
data class HistoryFilter(
    val messageUuid: SerializableUUID?,
    val senderUuid: SerializableUUID?,
    val receiverUuid: SerializableUUID?,
    val messageType: MessageType?,
    val after: SerializableOffsetDateTime?,
    val server: String?,
    val deleted: Boolean?,
    val deletedBy: SerializableUUID?,
    val limit: Int = 100
)
```

### Felder

| Feld           | Typ               | Beschreibung                                                                 |
| -------------- | ----------------- | ---------------------------------------------------------------------------- |
| `messageUuid`  | `UUID?`           | Suche nach einer bestimmten Nachrichten-ID                                   |
| `senderUuid`   | `UUID?`           | Nur Nachrichten dieses Absenders                                             |
| `receiverUuid` | `UUID?`           | Nur Nachrichten an diesen Empfänger                                          |
| `messageType`  | `MessageType?`    | Nur Nachrichten dieses Typs (z. B. `MessageType.DIRECT`)                     |
| `after`        | `OffsetDateTime?` | Nur Nachrichten nach diesem Zeitpunkt                                        |
| `server`       | `String?`         | Nur Nachrichten von diesem Server                                            |
| `deleted`      | `Boolean?`        | `true` = nur gelöschte, `false` = nur nicht gelöschte, `null` = alle        |
| `deletedBy`    | `UUID?`           | Nur Nachrichten, die von diesem Moderator gelöscht wurden                    |
| `limit`        | `Int`             | Maximale Anzahl zurückgegebener Einträge (Standard: `100`)                   |

### Leerer Filter

```kotlin
// Gibt die letzten 50 Nachrichten ohne Einschränkung zurück
val filter = HistoryFilter.empty()
```

---

## Verlauf abrufen

```kotlin
@Throws(TimeoutCancellationException::class)
suspend fun lookupHistory(filter: HistoryFilter): ObjectList<HistoryEntry>
```

### Beispiele

```kotlin
// Alle globalen Nachrichten der letzten Stunde
val filter = HistoryFilter(
    messageUuid  = null,
    senderUuid   = null,
    receiverUuid = null,
    messageType  = MessageType.GLOBAL,
    after        = OffsetDateTime.now().minusHours(1),
    server       = null,
    deleted      = false,
    deletedBy    = null,
    limit        = 50
)
val entries = SurfChatApi.lookupHistory(filter)
```

```kotlin
// Direktnachrichten zwischen zwei Spielern
val filter = HistoryFilter(
    messageUuid  = null,
    senderUuid   = playerA,
    receiverUuid = playerB,
    messageType  = MessageType.DIRECT,
    after        = null,
    server       = null,
    deleted      = null,
    deletedBy    = null,
    limit        = 100
)
val conversation = SurfChatApi.lookupHistory(filter)
```

{% hint style="warning" %}
`lookupHistory` ist eine Suspend-Funktion und kann eine `TimeoutCancellationException` werfen. Umschließe den Aufruf ggf. mit einem `try-catch`.
{% endhint %}

---

## Externe Links

* [GitHub – HistoryEntry.kt](https://github.com/SLNE-Development/surf-chat/blob/version/26.1/surf-chat-api/src/main/kotlin/dev/slne/surf/chat/api/entry/HistoryEntry.kt)
* [GitHub – HistoryFilter.kt](https://github.com/SLNE-Development/surf-chat/blob/version/26.1/surf-chat-api/src/main/kotlin/dev/slne/surf/chat/api/entry/HistoryFilter.kt)
