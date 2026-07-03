---
description: >-
  Steuere das Aussehen eines NPCs über Spieler-Skins und verschiedene
  Körperposen wie Stehen, Sitzen oder Schlafen.
icon: shirt
---

# Skin & Pose

## Skin

Ein `NpcSkin` enthält die Skin-Daten eines Minecraft-Spielers: `value`, `signature` sowie die aktivierten Skin-Teile (`NpcSkinPart`).

### Skin von einem Spielernamen laden

```kotlin
import dev.slne.surf.npc.api.dsl.fetchedSkin

// In der DSL (suspend)
val skin = fetchedSkin("Notch")

// Über die API direkt
val skin = SurfNpcApi.fetchSkin("Notch")
```

{% hint style="warning" %}
`fetchSkin` ist eine Suspend-Funktion, da sie die Mojang-API kontaktiert. Sie sollte beim Start oder auf Anfrage aufgerufen werden – **nicht** in einem Tick-Handler.
{% endhint %}

### Standard-Skin verwenden

```kotlin
val emptySkin = NpcSkin.empty()
```

`NpcSkin.empty()` liefert einen vordefinierten Standard-Skin, der keinen Online-Abruf erfordert.

### Skin eines NPCs ändern

```kotlin
myNpc.setSkinData(newSkin)
```

### Skin-Teile (`NpcSkinPart`)

| Teil             | Beschreibung           |
| ---------------- | ---------------------- |
| `CAPE`           | Umhang                 |
| `JACKET`         | Jacke (zweite Schicht) |
| `LEFT_SLEEVES`   | Linker Ärmel           |
| `RIGHT_SLEEVES`  | Rechter Ärmel          |
| `LEFT_PANTS_LEG` | Linkes Hosenbein       |
| `RIGHT_PANTS_LEG`| Rechtes Hosenbein      |
| `HAT`            | Hut (zweite Schicht)   |

Alle Teile sind standardmäßig aktiviert. Über den `SkinBuilder` in der DSL können sie einzeln konfiguriert werden.

---

## Pose

Eine `NpcPose` legt die Körperhaltung des NPCs fest.

### Verfügbare Posen

| Pose          | Nutzbar | Beschreibung                  |
| ------------- | ------- | ----------------------------- |
| `STANDING`    | ✅       | Stehend (Standard)            |
| `FALL_FLYING` | ✅       | Fliegend (Elytra-Animation)   |
| `SLEEPING`    | ✅       | Schlafend                     |
| `SWIMMING`    | ✅       | Schwimmend                    |
| `SNEAKING`    | ✅       | Schleichend                   |
| `SITTING`     | ✅       | Sitzend                       |

{% hint style="info" %}
Posen, die mit `@Deprecated("No effect")` markiert sind (z. B. `SPIN_ATTACK`, `DYING`), haben bei NPCs keinen sichtbaren Effekt und sollten nicht verwendet werden.
{% endhint %}

### Pose setzen und lesen

```kotlin
import dev.slne.surf.npc.api.npc.NpcPose

// Pose setzen
myNpc.setPose(NpcPose.SITTING)

// Aktuelle Pose lesen
val aktuellePose: NpcPose = myNpc.getPose()
```

### Beispiel: NPC der winkt und schläft

```kotlin
// NPC in Schlafpose versetzen
myNpc.setPose(NpcPose.SLEEPING)

// Etwas später: zurück zur stehenden Pose
myNpc.setPose(NpcPose.STANDING)
```

## Externe Links

* [GitHub – NpcSkin.kt](https://github.com/SLNE-Development/surf-npc/blob/version/26.1/surf-npc-api/src/main/kotlin/dev/slne/surf/npc/api/npc/skin/NpcSkin.kt)
* [GitHub – NpcPose.kt](https://github.com/SLNE-Development/surf-npc/blob/version/26.1/surf-npc-api/src/main/kotlin/dev/slne/surf/npc/api/npc/NpcPose.kt)
