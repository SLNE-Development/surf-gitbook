---
icon: arrow-right-arrow-left
description: >-
  Mit einem MessageRedirector können Nachrichten an externe Systeme
  weitergeleitet werden – zum Beispiel an einen Discord-Kanal oder ein
  Logging-System.
---

# Message Redirector

`MessageRedirector` ermöglicht es, gesendete Nachrichten an externe Ziele weiterzuleiten, ohne den Chat-Fluss zu unterbrechen. Typische Anwendungsfälle sind:

* Weiterleitung in einen Discord-Kanal
* Logging in ein externes System
* Übertragung in ein Moderations-Dashboard

## Interface

```kotlin
interface MessageRedirector {
    suspend fun redirectMessage(signedMessage: SignedMessage, messageData: MessageData)
}
```

| Parameter       | Typ            | Beschreibung                                                                   |
| --------------- | -------------- | ------------------------------------------------------------------------------ |
| `signedMessage` | `SignedMessage`| Die originale Adventure-SignedMessage inklusive Signatur                       |
| `messageData`   | `MessageData`  | Alle Metadaten der Nachricht (Sender, Typ, Inhalt, Zeitstempel etc.)           |

{% hint style="info" %}
`redirectMessage` ist eine Suspend-Funktion. Du kannst darin direkt auf Datenbanken, HTTP-APIs oder andere asynchrone Ressourcen zugreifen.
{% endhint %}

---

## Registrierung

Redirectoren werden über `SurfChatApi.registerMessageRedirector(...)` registriert:

```kotlin
SurfChatApi.registerMessageRedirector(myRedirector)
```

Intern werden sie in der `MessageRedirectorRegistry` gehalten:

```kotlin
// Alle registrierten Redirectoren
MessageRedirectorRegistry.redirectors
```

---

## Beispiel: Discord-Weiterleitung

```kotlin
object DiscordChatRedirector : MessageRedirector {
    override suspend fun redirectMessage(
        signedMessage: SignedMessage,
        messageData: MessageData
    ) {
        if (messageData.type != MessageType.GLOBAL) return

        val sender = messageData.senderUser()
        val text   = messageData.plainMessage

        DiscordBot.sendToChannel(
            channelId = CHAT_CHANNEL_ID,
            message   = "**${sender.username}**: $text"
        )
    }
}

// Beim Plugin-Start registrieren
SurfChatApi.registerMessageRedirector(DiscordChatRedirector)
```

---

## MessageRedirectorRegistry

Die Registry verwaltet alle aktiven Redirectoren und kann zur Laufzeit modifiziert werden:

```kotlin
// Redirector direkt registrieren (entspricht SurfChatApi.registerMessageRedirector)
MessageRedirectorRegistry.register(myRedirector)

// Alle Redirectoren entfernen
MessageRedirectorRegistry.clear()
```

{% hint style="warning" %}
`MessageRedirectorRegistry.clear()` entfernt **alle** registrierten Redirectoren. Nutze diese Methode nur, wenn du die vollständige Kontrolle über alle Redirectoren hast.
{% endhint %}

---

## Externe Links

* [GitHub – MessageRedirector.kt](https://github.com/SLNE-Development/surf-chat/blob/version/26.1/surf-chat-api/src/main/kotlin/dev/slne/surf/chat/api/message/redirector/MessageRedirector.kt)
* [GitHub – MessageRedirectorRegistry.kt](https://github.com/SLNE-Development/surf-chat/blob/version/26.1/surf-chat-api/src/main/kotlin/dev/slne/surf/chat/api/message/redirector/MessageRedirectorRegistry.kt)
