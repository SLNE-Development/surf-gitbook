---
description: >-
  SurfCoreApi ist die zentrale Schnittstelle für netzwerkweite Spieler-
  und Serververwaltung sowie das plattformübergreifende Event-System.
icon: network-wired
---

# SurfCoreApi

`SurfCoreApi` bietet Zugriff auf alle online Spieler des Netzwerks, Server-Metadaten, Spielerweiterleitung und ein einfaches Event-System. Die API ist auf **allen Plattformen** (Paper und Velocity) verfügbar.

## Zugriff

```kotlin
import dev.slne.surf.core.api.common.SurfCoreApi

val players = SurfCoreApi.getOnlinePlayers()
```

---

## Spielerverwaltung

### Alle Online-Spieler abrufen

```kotlin
fun getOnlinePlayers(): ObjectSet<SurfPlayer>
```

Gibt alle aktuell im Netzwerk eingeloggten Spieler zurück.

### Spieler nach Name oder UUID

```kotlin
fun getPlayer(name: String): SurfPlayer?
fun getPlayer(uuid: UUID): SurfPlayer?
```

Gibt einen online Spieler zurück, oder `null` wenn er nicht gefunden wird.

### Offline-Spieler abrufen

```kotlin
suspend fun getOfflinePlayer(name: String): SurfPlayer?
suspend fun getOfflinePlayer(uuid: UUID): SurfPlayer?
```

Lädt einen Spieler aus der Datenbank – auch wenn er nicht online ist.

{% hint style="warning" %}
`getOfflinePlayer` ist eine Suspend-Funktion und muss aus einem Coroutine-Kontext aufgerufen werden.
{% endhint %}

---

## Serververwaltung

### Aktuellen Server abfragen

```kotlin
fun getCurrentServerName(): String
fun getCurrentServerCategory(): String
fun getCurrentServerDisplayName(): String
fun getCurrentServer(): SurfServer
fun getCurrentProxy(): SurfProxyServer
```

### Server nach Name oder Kategorie

```kotlin
fun getServerByName(name: String): SurfServer?
fun getServerByCategory(category: String): ObjectSet<SurfServer>
fun getServerWithLeastPlayers(category: String): SurfServer?
```

`getServerWithLeastPlayers` eignet sich zum Lastausgleich, z. B. um Spieler auf den am wenigsten besetzten Lobby-Server zu schicken.

### Alle Server abrufen

```kotlin
fun getServers(): ObjectSet<SurfServer>
fun getProxies(): ObjectSet<SurfProxyServer>
fun getCommonServers(): ObjectSet<CommonSurfServer>
```

---

## Spieler weiterleiten

```kotlin
suspend fun sendPlayerAwaiting(surfPlayer: SurfPlayer, surfServer: SurfServer): SurfServerConnectResult
suspend fun sendPlayerAwaiting(surfPlayer: SurfPlayer, surfProxyServer: SurfProxyServer): SurfProxyServerConnectionResult
```

Leitet einen Spieler zu einem anderen Server weiter und wartet auf das Ergebnis.

```kotlin
val server = SurfCoreApi.getServerWithLeastPlayers("lobby") ?: return
val result = SurfCoreApi.sendPlayerAwaiting(surfPlayer, server)
```

{% hint style="warning" %}
Diese Methoden sind Suspend-Funktionen.
{% endhint %}

### Nachricht netzwerkweit senden

```kotlin
fun sendText(player: SurfPlayer, text: Component)
```

Sendet eine Nachricht an einen Spieler – unabhängig davon, auf welchem Server er sich befindet. Die Übertragung erfolgt über Redis.

---

## Event-System

surf-core stellt ein plattformübergreifendes Event-System zur Verfügung:

### Listener registrieren

```kotlin
fun registerListener(listener: Any)
```

### Event feuern

```kotlin
fun fireEvent(event: SurfEvent)
```

### Direkt auf ein Event subscriben

```kotlin
fun subscribe(eventClass: KClass<out SurfEvent>, handler: (SurfEvent) -> Unit)
```

```kotlin
SurfCoreApi.subscribe(PlayerJoinNetworkEvent::class) { event ->
    println("${(event as PlayerJoinNetworkEvent).player.username} hat das Netzwerk betreten")
}
```

---

## Offline-Spielernamen laden

```kotlin
suspend fun loadOfflinePlayerNameEntries(): List<OfflinePlayerNameCache.Entry>
```

Lädt alle gespeicherten Spielernamen-Einträge aus dem Cache. Nützlich für Autovervollständigung oder Suche nach Offline-Spielern.

## Externe Links

* [GitHub – SurfCoreApi.kt](https://github.com/SLNE-Development/surf-core/blob/version/26.2/surf-core-api/surf-core-api-common/src/main/kotlin/dev/slne/surf/core/api/common/SurfCoreApi.kt)
