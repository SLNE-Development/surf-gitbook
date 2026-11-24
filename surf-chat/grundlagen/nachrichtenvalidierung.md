---
icon: shield-check
description: >-
  MessageValidationResult repräsentiert das Ergebnis einer Nachrichten-
  prüfung. Es enthält alle möglichen Fehlertypen, die der Chat von surf-player beim
  Verarbeiten einer Nachricht erkennt.
---

# Nachrichtenvalidierung

Bevor eine Nachricht gesendet wird, durchläuft sie mehrere Validierungsschritte. Das Ergebnis wird als `MessageValidationResult` zurückgegeben.

## MessageValidationResult

`MessageValidationResult` ist eine versiegelte Klasse mit zwei Zuständen:

```kotlin
sealed class MessageValidationResult {
    class Success : MessageValidationResult()
    data class Failure(val error: MessageValidationError) : MessageValidationResult()
}
```

### Hilfsmethoden

```kotlin
result.isFailure()           // true, wenn ein Fehler aufgetreten ist
result.getErrorOrThrow()     // MessageValidationError oder IllegalStateException

result is MessageValidationResult.Success                    // true, wenn die Validierung erfolgreich war
(result as? MessageValidationResult.Failure)?.error          // MessageValidationError oder null
```

{% hint style="info" %}
`isSuccess()` und `getErrorOrNull()` gibt es in surf-player nicht mehr. Verwende stattdessen `!result.isFailure()` bzw. die Ausdrücke oben.
{% endhint %}

### Beispiel

```kotlin
val result: MessageValidationResult = /* ... */

if (result.isFailure()) {
    val error = result.getErrorOrThrow()
    player.sendMessage(error.errorMessage)
    return
}
```

---

## Fehlertypen

Alle Fehlertypen erben von `MessageValidationError` und besitzen:

| Eigenschaft    | Typ         | Beschreibung                                                  |
| -------------- | ----------- | ------------------------------------------------------------- |
| `errorMessage` | `Component` | Angezeigter Fehlertext für den Spieler                        |
| `name`         | `String`    | Interner Name des Fehlers (z. B. für Logs oder Moderatoren)   |

### Übersicht aller Fehlertypen

| Fehlerklasse       | `name`                    | Ursache                                                       |
| ------------------ | ------------------------- | ------------------------------------------------------------- |
| `EmptyContent`     | Kein Inhalt               | Die Nachricht ist leer                                        |
| `TooManyCaps`      | Zu viele Großbuchstaben   | Zu viele Großbuchstaben im Text                               |
| `AutoDisabled`     | Zu viele Spieler          | Chat wurde automatisch deaktiviert (z. B. Überlastung)        |
| `ChatDisabled`     | Chat deaktiviert          | Der Chat ist auf diesem Server deaktiviert                    |

{% hint style="warning" %}
Die Fehlertypen `DenylistedWord`, `BadLink`, `BadCharacters` und `TooOften` gibt es in surf-player nicht mehr, da diese Prüfungen (Denylist, Link- und Zeichenprüfung) entfallen sind. Ein exhaustives `when` über `MessageValidationError` muss nur noch die vier Fehlerklassen oben behandeln. `MessageValidationError` ist jetzt eine eigene Klasse im Paket `dev.slne.surf.player.api.chat.message` und nicht mehr in `MessageValidationResult` verschachtelt.
{% endhint %}

---

## Externe Links

* [GitHub – MessageValidationResult.kt](https://github.com/SLNE-Development/surf-player/blob/version/26.3/api/src/main/kotlin/dev/slne/surf/player/api/chat/message/MessageValidationResult.kt)
* [GitHub – MessageValidationError.kt](https://github.com/SLNE-Development/surf-player/blob/version/26.3/api/src/main/kotlin/dev/slne/surf/player/api/chat/message/MessageValidationError.kt)
