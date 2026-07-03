---
description: >-
  SurfPlayer ist die netzwerkweite Spieler-Repräsentation von surf-core und
  enthält Informationen zu Online-Status, aktuellem Server und Metadaten.
icon: user-astronaut
---

# SurfPlayer

`SurfPlayer` ist das zentrale Datenobjekt für Spieler im Surf-Netzwerk. Es ist nicht an eine bestimmte Plattform gebunden und kann auf Paper- wie auch Velocity-Servern verwendet werden.

## Eigenschaften

| Eigenschaft          | Typ                  | Beschreibung                                        |
| -------------------- | -------------------- | --------------------------------------------------- |
| `uuid`               | `UUID`               | Eindeutige Spieler-UUID                             |
| `lastKnownName`      | `String?`            | Zuletzt bekannter Spielername                       |
| `firstSeen`          | `OffsetDateTime?`    | Zeitpunkt des ersten Logins                         |
| `lastSeen`           | `OffsetDateTime?`    | Zeitpunkt des letzten Logins                        |
| `currentServerName`  | `String?`            | Name des Servers, auf dem der Spieler aktuell ist   |
| `currentProxyName`   | `String?`            | Name des Proxys, über den der Spieler verbunden ist |
| `lastKnownIpAddress` | `InetAddress?`       | Zuletzt bekannte IP-Adresse                         |
| `transferred`        | `Boolean`            | Ob der Spieler gerade weitergeleitet wird           |

## Berechnete Eigenschaften

```kotlin
val username: String           // lastKnownName ?: "#Unbekannt"
val currentServer: SurfServer? // aktuelles Backend-Server-Objekt
val currentProxy: SurfProxyServer? // aktuelles Proxy-Objekt
```

## Methoden

### Online-Status prüfen

```kotlin
fun isOnline(): Boolean
```

Gibt `true` zurück, wenn der Spieler aktuell online ist.

### Spieler weiterleiten

```kotlin
suspend fun sendAwaiting(server: SurfServer): SurfServerConnectResult
suspend fun sendAwaiting(proxy: SurfProxyServer): SurfProxyServerConnectionResult
```

Kurzform von `SurfCoreApi.sendPlayerAwaiting(...)`.

```kotlin
val lobby = SurfCoreApi.getServerWithLeastPlayers("lobby") ?: return
surfPlayer.sendAwaiting(lobby)
```

## Spieler abrufen

```kotlin
// Online-Spieler
val player: SurfPlayer? = SurfCoreApi.getPlayer(player.uniqueId)

// Offline-Spieler (suspend)
val offline: SurfPlayer? = SurfCoreApi.getOfflinePlayer("Notch")
```

{% hint style="info" %}
`SurfPlayer` ist keine Bukkit-`Player`-Instanz. Für plattformspezifische Aktionen (z. B. Inventar öffnen) muss der Bukkit-Spieler über `Bukkit.getPlayer(surfPlayer.uuid)` abgerufen werden.
{% endhint %}

## Externe Links

* [GitHub – SurfPlayer.kt](https://github.com/SLNE-Development/surf-core/blob/version/26.2/surf-core-api/surf-core-api-common/src/main/kotlin/dev/slne/surf/core/api/common/player/SurfPlayer.kt)
