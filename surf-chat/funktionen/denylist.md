---
icon: ban
description: >-
  Die Denylist von surf-chat erkennt unerlaubte Wörter und Links automatisch
  und führt konfigurierbare Moderationsmaßnahmen durch – von Verwarnungen bis
  zu permanenten Bans.
---

# Denylist

Die Denylist ist ein automatisches Moderationssystem. Enthält eine Nachricht ein verbotenes Wort oder einen unerlaubten Link, wird die konfigurierte Aktion ausgelöst und der Absender sanktioniert.

## DenylistEntry

Jeder Eintrag in der Denylist beschreibt ein verbotenes Wort und die zugehörige Sanktion.

```kotlin
data class DenylistEntry(
    val word: String,
    val reason: String,
    val addedBy: UUID?,
    val addedAt: OffsetDateTime,
    val action: DenylistAction
)
```

### Felder

| Feld       | Typ               | Beschreibung                                               |
| ---------- | ----------------- | ---------------------------------------------------------- |
| `word`     | `String`          | Das verbotene Wort oder Muster                             |
| `reason`   | `String`          | Grund für den Denylist-Eintrag (intern/für Moderatoren)    |
| `addedBy`  | `UUID?`           | UUID des Moderators, der den Eintrag erstellt hat          |
| `addedAt`  | `OffsetDateTime`  | Zeitpunkt der Erstellung                                   |
| `action`   | `DenylistAction`  | Die auszuführende Moderationsmaßnahme                      |

### Hilfsmethoden

```kotlin
// Moderator als SurfPlayer laden (Suspend)
val moderator = entry.addedBy()
```

---

## DenylistAction

`DenylistAction` beschreibt die Sanktion, die bei einem Treffer ausgeführt wird.

```kotlin
data class DenylistAction(
    val name: String,
    val actionType: DenylistActionType,
    val reason: String,
    val duration: Long
)
```

### Felder

| Feld         | Typ                   | Beschreibung                                                             |
| ------------ | --------------------- | ------------------------------------------------------------------------ |
| `name`       | `String`              | Anzeigename der Aktion (z. B. „Warnung: Beleidigung")                   |
| `actionType` | `DenylistActionType`  | Art der Sanktion (siehe unten)                                           |
| `reason`     | `String`              | Angezeigter Grund für die Sanktion                                       |
| `duration`   | `Long`                | Dauer der Sanktion in Millisekunden (nur bei zeitlich begrenzten Bans/Mutes relevant) |

---

## DenylistActionType

`DenylistActionType` definiert, welche Maßnahme ergriffen wird:

| Wert              | Beschreibung                                                                          |
| ----------------- | ------------------------------------------------------------------------------------- |
| `WARN`            | Der Spieler erhält eine Verwarnung                                                    |
| `KICK`            | Der Spieler wird vom Server geworfen                                                  |
| `MUTE`            | Der Spieler wird für die konfigurierte `duration` stummgeschaltet                     |
| `EXPIRABLE_BAN`   | Zeitlich begrenzter Ban für die konfigurierte `duration`                              |
| `PERMANENT_BAN`   | Dauerhafter Ban ohne Ablaufzeit                                                       |
| `COMMUNITY_BAN`   | Netzwerkweiter Community-Ban; kann zusätzlich eine Discord-Benachrichtigung auslösen  |

{% hint style="warning" %}
`COMMUNITY_BAN` ist die schwerste Maßnahme. Sie sollte nur für schwerwiegende oder wiederholte Verstöße eingesetzt werden, da sie netzwerkweit wirkt.
{% endhint %}

---

## AutoMod

surf-chat prüft Nachrichten automatisch über `SurfChatApi.passAutoMod(...)`:

```kotlin
suspend fun passAutoMod(messageData: MessageData)
```

Diese Methode wird intern aufgerufen, sobald eine Nachricht verarbeitet wird. Sie prüft:

1. Ob der Inhalt ein Wort aus der Denylist enthält
2. Ob die Nachricht einen nicht erlaubten Link enthält
3. Ob weitere Validierungsregeln verletzt wurden

Bei einem Treffer wird die zugehörige `DenylistAction` ausgelöst und das Ergebnis als [`MessageValidationResult.Failure`](../grundlagen/nachrichtenvalidierung.md) zurückgegeben.

---

## Externe Links

* [GitHub – DenylistEntry.kt](https://github.com/SLNE-Development/surf-chat/blob/version/26.1/surf-chat-api/src/main/kotlin/dev/slne/surf/chat/api/denylist/DenylistEntry.kt)
* [GitHub – DenylistAction.kt](https://github.com/SLNE-Development/surf-chat/blob/version/26.1/surf-chat-api/src/main/kotlin/dev/slne/surf/chat/api/denylist/DenylistAction.kt)
* [GitHub – DenylistActionType.kt](https://github.com/SLNE-Development/surf-chat/blob/version/26.1/surf-chat-api/src/main/kotlin/dev/slne/surf/chat/api/denylist/DenylistActionType.kt)
