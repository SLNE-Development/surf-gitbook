---
icon: shield-check
description: >-
  MessageValidationResult repräsentiert das Ergebnis einer Nachrichten-
  prüfung. Es enthält alle möglichen Fehlertypen, die surf-chat beim
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
result.isSuccess()           // true, wenn die Validierung erfolgreich war
result.isFailure()           // true, wenn ein Fehler aufgetreten ist
result.getErrorOrNull()      // MessageValidationError oder null
result.getErrorOrThrow()     // MessageValidationError oder IllegalStateException
```

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
| `DenylistedWord`   | Unerlaubtes Wort: \<wort> | Ein Wort aus der [Denylist](../funktionen/denylist.md) wurde erkannt |
| `BadLink`          | Unerlaubter Link: \<url>  | Die Nachricht enthält einen nicht erlaubten Link              |
| `BadCharacters`    | Unerlaubte Zeichen: \<c>  | Unerlaubte Sonderzeichen im Text                              |
| `TooOften`         | Spam                      | Der Spieler schreibt zu schnell (Spam-Schutz)                 |
| `AutoDisabled`     | Zu viele Spieler          | Chat wurde automatisch deaktiviert (z. B. Überlastung)        |
| `ChatDisabled`     | Chat deaktiviert          | Der Chat wurde manuell deaktiviert                            |

### DenylistedWord

```kotlin
data class DenylistedWord(val denylistEntry: DenylistEntry) : MessageValidationError(...)
```

Enthält den vollständigen [`DenylistEntry`](../funktionen/denylist.md), der den Treffer ausgelöst hat – inklusive der konfigurierten Moderationsmaßnahme.

### BadLink

```kotlin
data class BadLink(val url: String) : MessageValidationError(...)
```

Gibt die genaue URL zurück, die den Fehler ausgelöst hat.

### TooOften

```kotlin
data class TooOften(val next: Long) : MessageValidationError(...)
```

`next` enthält den Unix-Zeitstempel (in Millisekunden), ab wann der Spieler wieder schreiben darf.

```kotlin
// Beispiel: Countdown anzeigen
val waitSeconds = (error.next - System.currentTimeMillis()) / 1000
player.sendMessage("Bitte warte noch ${waitSeconds}s.")
```

---

## Externe Links

* [GitHub – MessageValidationResult.kt](https://github.com/SLNE-Development/surf-chat/blob/version/26.1/surf-chat-api/src/main/kotlin/dev/slne/surf/chat/api/message/MessageValidationResult.kt)
