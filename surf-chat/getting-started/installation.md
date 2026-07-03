---
icon: bolt
description: >-
  Erfahre, wie du surf-chat als Abhängigkeit in dein Projekt einbindest.
---

# Installation

surf-chat wird als `compileOnly`-Abhängigkeit eingebunden. Das Plugin muss auf dem Server laufen, um zur Laufzeit verfügbar zu sein.

## Abhängigkeit hinzufügen

Füge surf-chat-api in deiner `build.gradle.kts` als Abhängigkeit hinzu:

```kotlin
dependencies {
    compileOnly("dev.slne.surf:surf-chat-api:+")
}
```

{% hint style="info" %}
surf-chat setzt [surf-api](https://app.gitbook.com/s/gzrBT4z2A7XDMgPwFSv5/) voraus. Stelle sicher, dass das surf-api-Gradle-Plugin konfiguriert ist, bevor du surf-chat einbindest.
{% endhint %}

## Zugriff auf die API

Nach der Einbindung ist die API über das Companion-Objekt `SurfChatApi` direkt zugänglich:

```kotlin
import dev.slne.surf.chat.api.SurfChatApi

// Nachrichten-Verlauf abrufen
val history = SurfChatApi.lookupHistory(HistoryFilter.empty())
```

{% hint style="warning" %}
Greife erst nach dem Start des surf-chat-Plugins auf `SurfChatApi` zu. Ein zu früher Zugriff (z. B. während `onLoad`) führt zu einem Fehler, da der Service noch nicht registriert ist.
{% endhint %}

## Externe Links

* [GitHub – surf-chat Repository](https://github.com/SLNE-Development/surf-chat/tree/version/26.1/)
* [GitHub – build.gradle.kts (surf-chat-api)](https://github.com/SLNE-Development/surf-chat/blob/version/26.1/surf-chat-api/build.gradle.kts)
