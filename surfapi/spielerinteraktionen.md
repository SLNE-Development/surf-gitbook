---
description: Hier findest du Anleitungen um Spielerinteraktionen zu erstellen.
icon: user-check
---

# Spielerinteraktionen

**Nachrichten senden**

Sende eine Nachricht mit dem [SurfComponentBuilder](component-builder.md) an eine Audience.

{% code overflow="wrap" expandable="true" %}
```kotlin
player.sendText {
    appendPrefix()
    error("Das ist eine Fehlernachricht.")
}
```
{% endcode %}

**Sound abspielen**

Spiele einen Ton für eine einzelne Audience ab.

```kotlin
player.playSound {
    type(Sound.BLOCK_COMPARATOR_CLICK)
    source(net.kyori.adventure.sound.Sound.Source.BLOCK)
    pitch(1f)
    volume(1f)
}

player.playSound(true) { // "true" zeigt, dass der Self-Emitter genutzt wird
    type(Sound.BLOCK_COMPARATOR_CLICK)
    source(net.kyori.adventure.sound.Sound.Source.BLOCK)
    pitch(1f)
    volume(1f)
}
```

**Titel anzeigen**

Zeige einen Titel einer Audience.

```kotlin
player.showTitle {
    title {
        primary("Beispiel-Titel")
    }
    subtitle {
        spacer("Beispiel Sub-Title")
    }
    times {
        fadeIn(250.milliseconds)
        stay(10.seconds)
        fadeOut(250.milliseconds)
    }
}
```
