---
icon: bolt
description: >-
  Erfahre, wie du die Chat-API von surf-player als Abhängigkeit in dein Projekt einbindest.
---

# Installation

{% hint style="info" %}
surf-chat ist Teil von **surf-player**. Alle früheren Player-Plugins (surf-chat, surf-clan, surf-friends, surf-settings, …) wurden im Monorepo [SLNE-Development/surf-player](https://github.com/SLNE-Development/surf-player) zusammengeführt. Die einzige öffentliche API ist `surf-player-api`; `dev.slne.surf:surf-chat-api` existiert nicht mehr.
{% endhint %}

Die API wird als `compileOnly`-Abhängigkeit eingebunden. Das surf-player-Plugin der jeweiligen Plattform muss auf dem Server laufen, um zur Laufzeit verfügbar zu sein.

## Abhängigkeit hinzufügen

Füge surf-player-api in deiner `build.gradle.kts` als Abhängigkeit hinzu:

```kotlin
repositories {
    maven("https://reposilite.slne.dev/releases")
}

dependencies {
    compileOnly("dev.slne.surf.player:surf-player-api:1.0.3")
}
```

{% hint style="info" %}
surf-player setzt [surf-api](https://app.gitbook.com/s/gzrBT4z2A7XDMgPwFSv5/) voraus. Stelle sicher, dass das surf-api-Gradle-Plugin konfiguriert ist, bevor du surf-player-api einbindest.
{% endhint %}

## Plugin-Abhängigkeit deklarieren

Damit surf-player vor deinem Plugin geladen wird, deklariere eine Abhängigkeit auf das Plugin deiner Plattform:

```kotlin
// Paper
surfPaperPluginApi {
    serverDependencies {
        registerRequired("surf-player-paper")
    }
}

// Minestom
minestomPluginFile {
    pluginDependencies {
        register("surf-player-minestom")
    }
}
```

Der Chat ist auf Paper und Minestom verfügbar, nicht auf Velocity.

## Zugriff auf die API

Nach der Einbindung ist die API über das Companion-Objekt `SurfChatApi` direkt zugänglich:

```kotlin
import dev.slne.surf.player.api.chat.SurfChatApi

// Nachrichten-Verlauf abrufen
val history = SurfChatApi.lookupHistory(HistoryFilter.empty())
```

Alles, was einen einzelnen Spieler betrifft, erreichst du zusätzlich über `SurfPlayerApi.player(uuid)` (`ChatCapability`):

```kotlin
val ignored = SurfPlayerApi.player(uuid).ignoreList()
```

{% hint style="warning" %}
Greife erst nach dem Start des surf-player-Plugins auf `SurfChatApi` zu. Ein zu früher Zugriff (z. B. während `onLoad`) führt zu einem Fehler, da der Service noch nicht registriert ist.
{% endhint %}

## Externe Links

* [GitHub – surf-player Repository](https://github.com/SLNE-Development/surf-player)
* [GitHub – README (öffentliche API)](https://github.com/SLNE-Development/surf-player/blob/version/26.3/README.md)
* [GitHub – Migration von Befehlen, Permissions und API](https://github.com/SLNE-Development/surf-player/blob/version/26.3/docs/command-migration.md)
