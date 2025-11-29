---
description: >-
  Der PlayerLookupService ist auf allen Platformen verfügbar und hilft dabei,
  Minecraft Namen oder UUIDs herrauszufinden.
icon: user
---

# PlayerLookupService

Er bietet einen stabilen Zugriff auf die Mojang Api, die Minecraft Services und die Minetools Api. Die Ergebnisse werden für 15 Minuten gespeichert um schnellen Zugriff zu bieten.

**Spielername von UUID abrufen**

<pre class="language-kotlin"><code class="lang-kotlin">import dev.slne.surf.surfapi.core.api.service.PlayerLookupService

<strong>val playerName: String? = PlayerLookupService.getUsername(playerUuid)
</strong></code></pre>

**UUID von Spielername abrufen**

<pre class="language-kotlin"><code class="lang-kotlin">import dev.slne.surf.surfapi.core.api.service.PlayerLookupService
import java.util.UUID

<strong>val playerUuid: UUID? = PlayerLookupService.getUsername(playerName)
</strong></code></pre>

{% hint style="danger" %}
Beachte, das alle Interaktionen mit dem PlayerLookupService async aufgerufen werden müssen.
{% endhint %}
