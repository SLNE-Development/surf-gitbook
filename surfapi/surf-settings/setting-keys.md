---
description: >-
  SettingKeys ist das zentrale Register aller vordefinierten Einstellungsschlüssel
  des Surf-Ökosystems.
icon: list
---

# SettingKeys

Das Objekt `SettingKeys` enthält alle standardmäßig registrierten [`SettingKey`](setting-key.md)-Instanzen von surf-player. Es dient als einzige Quelle der Wahrheit für die Schlüsselnamen und Standardwerte.

## Chat

| Key                        | Typ       | Standard | Beschreibung                                          |
| -------------------------- | --------- | -------- | ----------------------------------------------------- |
| `CHAT_PINGS`               | `Boolean` | `true`   | Spieler kann im Chat angepingt werden                 |
| `CHAT_DEATH_MESSAGES`      | `Boolean` | `true`   | Todesnachrichten werden im Chat angezeigt             |
| `DIRECT_MESSAGES`          | `Boolean` | `true`   | Direktnachrichten (private Nachrichten) aktiviert     |
| `CONNECTION_MESSAGES`      | `Boolean` | `true`   | Verbindungs-/Abmeldenachrichten werden angezeigt      |

## Lobby

| Key                  | Typ       | Standard | Beschreibung                                       |
| -------------------- | --------- | -------- | -------------------------------------------------- |
| `LOBBY_SCROLL_SOUND` | `Boolean` | `false`  | Scrollgeräusch im Lobby-Menü aktiviert             |

## Clan

| Key                   | Typ       | Standard | Beschreibung                                      |
| --------------------- | --------- | -------- | ------------------------------------------------- |
| `CLAN_INVITES`        | `Boolean` | `true`   | Clan-Einladungen werden empfangen                 |
| `CLAN_CHAT_MESSAGES`  | `Boolean` | `true`   | Clan-Chat-Nachrichten werden angezeigt            |

## Freunde

| Key                           | Typ       | Standard | Beschreibung                                          |
| ----------------------------- | --------- | -------- | ----------------------------------------------------- |
| `FRIEND_REQUESTS`             | `Boolean` | `true`   | Spieler nimmt Freundschaftsanfragen an (wird vom Microservice durchgesetzt) |
| `FRIEND_REQUEST_NOTIFICATIONS`| `Boolean` | `true`   | Freundschaftsanfragen-Benachrichtigungen aktiviert     |
| `FRIEND_NOTIFICATIONS`        | `Boolean` | `true`   | Allgemeine Freund-Benachrichtigungen aktiviert         |
| `FRIEND_SOUNDS`               | `Boolean` | `true`   | Töne bei Freundschaftsereignissen aktiviert            |

## Nametag

| Key             | Typ       | Standard | Beschreibung                                      |
| --------------- | --------- | -------- | ------------------------------------------------- |
| `SHOW_NAMETAGS` | `Boolean` | `true`   | Namensschilder anderer Spieler werden angezeigt   |

## Queue

| Key                          | Typ       | Standard | Beschreibung                                         |
| ---------------------------- | --------- | -------- | ---------------------------------------------------- |
| `AUTO_QUEUE_AFTER_RECONNECT` | `Boolean` | `true`   | Spieler wird nach einem Reconnect automatisch in die Warteschlange gestellt |

{% hint style="info" %}
Der frühere Key `SHOW_SCOREBOARD` (`scoreboard:show-scoreboard`) wurde entfernt.
{% endhint %}

## Verwendung

```kotlin
import dev.slne.surf.player.api.settings.SurfSettingsApi
import dev.slne.surf.player.api.settings.setting.SettingKeys

// Wert eines Schlüssels lesen
val showPings: Boolean = SurfSettingsApi.getSettingValue(player.uniqueId, SettingKeys.CHAT_PINGS)

// Dasselbe über den Spieler (SettingsCapability)
val showPings2: Boolean = SurfPlayerApi.player(player.uniqueId).setting(SettingKeys.CHAT_PINGS)
```

{% hint style="warning" %}
Erstelle keine eigenen Keys mit denselben Namespace-Pfaden wie die vordefinierten `SettingKeys`. Das führt zu Konflikten bei der Persistenz.
{% endhint %}

## Externe Links

* [GitHub – SettingKeys.kt](https://github.com/SLNE-Development/surf-player/blob/version/26.3/api/src/main/kotlin/dev/slne/surf/player/api/settings/setting/SettingKeys.kt)
