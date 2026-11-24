---
icon: code
description: >-
  SurfChatApi ist die zentrale Schnittstelle der Chat-Funktion von surf-player. Sie erlaubt das
  Registrieren von Prozessoren, das Abrufen der Ignore-Liste sowie das Suchen
  im Nachrichtenverlauf.
---

# SurfChatApi

`SurfChatApi` ist das Hauptinterface der Chat-Funktion von surf-player. Es ist auf Paper und Minestom über sein Companion-Objekt direkt zugänglich.

## Zugriff

```kotlin
import dev.slne.surf.player.api.chat.SurfChatApi
```

Alles, was einen einzelnen Spieler betrifft, ist zusätzlich über die `ChatCapability` erreichbar (`SurfPlayerApi.player(uuid)`):

```kotlin
val player = SurfPlayerApi.player(uuid)

player.cachedIgnoreList       // entspricht SurfChatApi.cachedIgnoreList(uuid)
player.ignoreList()           // entspricht SurfChatApi.ignoreList(uuid)
```

---

## Nachrichten

### Nachricht protokollieren

```kotlin
suspend fun logMessage(data: MessageData)
```

Speichert eine Nachricht im Nachrichtenverlauf. Wird intern nach dem Versenden einer Nachricht aufgerufen.

### Signierte Nachricht senden

```kotlin
suspend fun sendSignedMessage(
    senderUuid: UUID,
    targetUuid: UUID,
    signedMessage: SignedMessage,
    contentComponent: Component
)
```

Versendet eine kryptografisch signierte Nachricht von einem Spieler an einen anderen. Die Signatur wird von Minecraft für die Chat-Authentifizierung verwendet. Der Absender muss auf diesem Server sein, der Empfänger kann irgendwo im Netzwerk sein.

{% hint style="warning" %}
Die Reihenfolge der Parameter hat sich gegenüber der alten surf-chat-API geändert: `signedMessage` steht jetzt nach `senderUuid` und `targetUuid`. Über die `ChatCapability` lautet der Aufruf `sendSignedMessage(targetUuid, signedMessage, contentComponent)`.
{% endhint %}

### Nachricht löschen (lokal)

```kotlin
fun deleteMessage(signature: SignedMessage.Signature)
```

Löscht eine Nachricht lokal anhand ihrer Signatur (für alle Spieler, die die Nachricht gesehen haben).

### Nachricht löschen (netzwerkweit)

```kotlin
suspend fun deleteRemoteMessage(deleter: UUID?, messageData: MessageData)
```

Löscht eine Nachricht netzwerkweit und markiert sie im Verlauf als gelöscht. `deleter` ist die UUID des löschenden Moderators, oder `null` für System-Löschungen.

---

## Prozessoren registrieren

### Pre-Prozessor registrieren

```kotlin
fun registerChatProcessor(processor: PreChatProcessor)
```

Registriert einen [`PreChatProcessor`](../funktionen/chat-processors.md), der Nachrichten **vor** dem Versenden verarbeitet. Pre-Prozessoren werden nach ihrer `order`-Eigenschaft sortiert ausgeführt.

### Post-Prozessor registrieren

```kotlin
fun registerChatProcessor(processor: PostChatProcessor)
```

Registriert einen [`PostChatProcessor`](../funktionen/chat-processors.md), der **nach** dem Versenden einer Nachricht ausgeführt wird.

### Message Redirector registrieren

```kotlin
fun registerMessageRedirector(redirector: MessageRedirector)
```

Registriert einen [`MessageRedirector`](../funktionen/message-redirector.md), der Nachrichten an externe Systeme (z. B. Discord) weiterleitet.

---

## Ignore-Liste

### Gecachte Ignore-Liste abrufen

```kotlin
fun cachedIgnoreList(uuid: UUID): List<IgnoreListEntry>
```

Gibt die lokal gecachte Ignore-Liste eines Spielers zurück. Diese Methode ist synchron und greift nur auf den In-Memory-Cache zu.

### Ignore-Liste laden

```kotlin
suspend fun ignoreList(uuid: UUID): List<IgnoreListEntry>
```

Lädt die Ignore-Liste eines Spielers und legt sie im Cache dieses Servers ab. Nützlich für Offline-Spieler oder wenn der Cache möglicherweise veraltet ist.

{% hint style="warning" %}
`ignoreList` ist eine Suspend-Funktion und muss aus einem Coroutine-Kontext aufgerufen werden.
{% endhint %}

---

## Nachrichtenverlauf

### Verlauf durchsuchen

```kotlin
@Throws(TimeoutCancellationException::class)
suspend fun lookupHistory(filter: HistoryFilter): ObjectList<HistoryEntry>
```

Durchsucht den Nachrichtenverlauf anhand eines [`HistoryFilter`](../funktionen/nachrichtenverlauf.md). Gibt eine Liste von [`HistoryEntry`](../funktionen/nachrichtenverlauf.md)-Objekten zurück.

{% hint style="warning" %}
`lookupHistory` ist eine Suspend-Funktion und kann eine `TimeoutCancellationException` werfen, wenn die Anfrage zu lange dauert.
{% endhint %}

```kotlin
// Beispiel: Die letzten 10 globalen Nachrichten abrufen
val filter = HistoryFilter(
    messageUuid = null,
    senderUuid = null,
    receiverUuid = null,
    messageType = MessageType.GLOBAL,
    after = null,
    server = null,
    deleted = false,
    deletedBy = null,
    limit = 10
)
val history = SurfChatApi.lookupHistory(filter)
```

---

## AutoMod

### Nachricht durch AutoMod prüfen

```kotlin
suspend fun passAutoMod(messageData: MessageData)
```

Führt die KI-Moderation für die Nachricht aus. Sie läuft nur, wenn sie in der Konfiguration aktiviert ist (`enabled: true`) und ein API-Key hinterlegt ist.

{% hint style="info" %}
Die frühere Denylist (Wortlisten, Link- und Zeichenprüfung) gibt es in surf-player nicht mehr.
{% endhint %}

---

## Externe Links

* [GitHub – SurfChatApi.kt](https://github.com/SLNE-Development/surf-player/blob/version/26.3/api/src/main/kotlin/dev/slne/surf/player/api/chat/SurfChatApi.kt)
* [GitHub – ChatCapability.kt](https://github.com/SLNE-Development/surf-player/blob/version/26.3/api/src/main/kotlin/dev/slne/surf/player/api/chat/ChatCapability.kt)
