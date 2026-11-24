---
icon: user-slash
description: >-
  Die Ignore-Liste erlaubt Spielern, Nachrichten anderer Spieler auszublenden.
  IgnoreListEntry speichert, wer wen seit wann ignoriert.
---

# Ignore-Liste

Die Chat-Funktion von surf-player verwaltet eine Ignore-Liste pro Spieler. Ignorierte Spieler sehen weder globale noch direkte Nachrichten des ignorierten Spielers.

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
fun cachedIgnoreList(uuid: UUID): List<IgnoreListEntry>
```

Gibt die lokal im Speicher gehaltene Ignore-Liste zurück. Für Online-Spieler ist diese immer aktuell. Alternativ: `SurfPlayerApi.player(uuid).cachedIgnoreList` (`ChatCapability`).

```kotlin
val ignored = SurfChatApi.cachedIgnoreList(player.uniqueId)
val isIgnoring = ignored.any { it.target == otherPlayer.uniqueId }
```

### Datenbankbasierte Variante (Suspend)

```kotlin
suspend fun ignoreList(uuid: UUID): List<IgnoreListEntry>
```

Lädt die Ignore-Liste und legt sie im Cache dieses Servers ab. Nützlich für Offline-Spieler oder wenn der Cache möglicherweise veraltet ist. Alternativ: `SurfPlayerApi.player(uuid).ignoreList()`.

{% hint style="warning" %}
`ignoreList` ist eine Suspend-Funktion und muss aus einem Coroutine-Kontext aufgerufen werden. Für Online-Spieler ist die gecachte Variante zu bevorzugen.
{% endhint %}

```kotlin
// Ignore-Liste eines offline Spielers laden
val offlineIgnoreList = SurfChatApi.ignoreList(offlinePlayerUuid)
```

---

## Prüfen, ob ein Spieler ignoriert wird

```kotlin
fun isIgnored(viewer: UUID, sender: UUID): Boolean {
    return SurfChatApi.cachedIgnoreList(viewer).any { it.target == sender }
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
            SurfChatApi.cachedIgnoreList(uuid).any { it.target == sender }
        }
    }
}
```

---

## Befehle

Spieler verwalten ihre Ignore-Liste mit `/ignore add <spieler>`, `/ignore remove <spieler>` und `/ignore list [seite]` (früher: `/ignore <spieler>` als Toggle und `/ignore #list`). Team-Mitglieder mit der Permission `surf.player.chat.ignore.exempt` können nicht ignoriert werden.

---

## Externe Links

* [GitHub – IgnoreListEntry.kt](https://github.com/SLNE-Development/surf-player/blob/version/26.3/api/src/main/kotlin/dev/slne/surf/player/api/chat/entry/IgnoreListEntry.kt)
* [GitHub – SurfChatApi.kt](https://github.com/SLNE-Development/surf-player/blob/version/26.3/api/src/main/kotlin/dev/slne/surf/player/api/chat/SurfChatApi.kt)
