---
description: >-
  Die Glowing Api ermöglicht die Kontrolle des Glowing Effektes für Entities und
  Blöcke auf Packet Ebene.
icon: keyboard-brightness
---

# Glowing Api

{% hint style="warning" %}
Die Glowing Api ist nur auf der Paper/Bukkit Plattform verfügbar.
{% endhint %}

{% code title="Glowing-Api Methoden" %}
```kotlin
fun makeGlowing(target: Entity, viewer: Player, color: NamedTextColor? = null)
fun makeGlowing(block: Block, viewer: Player, color: NamedTextColor)
fun makeGlowing(location: Location, viewer: Player, color: NamedTextColor)

fun removeGlowing(target: Entity, viewer: Player)
fun removeGlowing(targetId: Int, viewer: Player)
fun removeGlowing(block: Block, viewer: Player)
fun removeGlowing(location: Location, viewer: Player)
```
{% endcode %}

{% code title="Beispiel" fullWidth="true" expandable="true" %}
```kotlin
glowingApi.makeGlowing(player, viewer, NamedTextColor.RED)

glowingApi.removeGlowing(player, viewer)
```
{% endcode %}
