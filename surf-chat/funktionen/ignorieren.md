---
icon: user-slash
description: >-
  Die Ignore-Liste erlaubt Spielern, Nachrichten anderer Spieler auszublenden.
  IgnoreListEntry speichert, wer wen seit wann ignoriert.
---

# Ignore-Liste

surf-chat verwaltet eine Ignore-Liste pro Spieler. Ignorierte Spieler sehen weder globale noch direkte Nachrichten des ignorierten Spielers.

## IgnoreListEntry

Jeder Eintrag in der Ignore-Liste wird als `IgnoreListEntry` gespeichert.

```kotlin
@Serializable
data class IgnoreListEntry(
    val user: UUID,
    val target: UUID,
    val createdAt: SerializableOffsetDateTime
)
```

### Felder

| Feld        | Typ               | Beschreibung                                          |
| ----------- | ----------------- | ----------------------------------------------------- |
| `user`      | `UUID`            | UUID des Spielers, der ignoriert                      |
| `target`    | `UUID`            | UUID des ignorierten Spielers                         |
| `createdAt` | `OffsetDateTime`  | Zeitpunkt, ab dem der Eintrag gilt                    |

---

## Ignore-Liste abrufen

### Gecachte Variante (synchron)

```kotlin
fun getCachedIgnoreList(uuid: UUID): List<IgnoreListEntry>
```

Gibt die lokal im Speicher gehaltene Ignore-Liste zurück. Für Online-Spieler ist diese immer aktuell.

```kotlin
val ignored = SurfChatApi.getCachedIgnoreList(player.uniqueId)
val isIgnoring = ignored.any { it.target == otherPlayer.uniqueId }
```

### Datenbankbasierte Variante (Suspend)

```kotlin
suspend fun getIgnoreList(uuid: UUID): List<IgnoreListEntry>
```

Lädt die Ignore-Liste direkt aus der Datenbank. Nützlich für Offline-Spieler oder wenn der Cache möglicherweise veraltet ist.

{% hint style="warning" %}
`getIgnoreList` ist eine Suspend-Funktion und muss aus einem Coroutine-Kontext aufgerufen werden. Für Online-Spieler ist die gecachte Variante zu bevorzugen.
{% endhint %}

```kotlin
// Ignore-Liste eines offline Spielers laden
val offlineIgnoreList = SurfChatApi.getIgnoreList(offlinePlayerUuid)
```

---

## Prüfen, ob ein Spieler ignoriert wird

```kotlin
fun isIgnored(viewer: UUID, sender: UUID): Boolean {
    return SurfChatApi.getCachedIgnoreList(viewer).any { it.target == sender }
}
```

---

## Beispiel: Nachricht ausblenden

Der folgende Pre-Prozessor entfernt Spieler aus den Betrachtern, wenn sie den Absender ignorieren:

```kotlin
object IgnoreListProcessor : PreChatProcessor {
    override val order = 20

    override fun process(context: MessageContext): MessageContext = context.edit {
        val sender = messageData.sender
        viewers.removeIf { audience ->
            val uuid = (audience as? Player)?.uniqueId ?: return@removeIf false
            SurfChatApi.getCachedIgnoreList(uuid).any { it.target == sender }
        }
    }
}
```

---

## Externe Links

* [GitHub – IgnoreListEntry.kt](https://github.com/SLNE-Development/surf-chat/blob/version/26.1/surf-chat-api/src/main/kotlin/dev/slne/surf/chat/api/entry/IgnoreListEntry.kt)
* [GitHub – SurfChatApi.kt](https://github.com/SLNE-Development/surf-chat/blob/version/26.1/surf-chat-api/src/main/kotlin/dev/slne/surf/chat/api/SurfChatApi.kt)
