---
icon: comment-dots
description: >-
  surf-chat ist das zentrale Chat-System des Surf-Netzwerks. Es verwaltet
  globale Nachrichten, Team-Chat, Direktnachrichten und bietet eine
  erweiterbare API für eigene Chat-Funktionen.
metaLinks: {}
---

# Willkommen bei surf-chat

{% hint style="info" %}
surf-chat ist Teil von **surf-player** (Monorepo [SLNE-Development/surf-player](https://github.com/SLNE-Development/surf-player)). Die Chat-API liegt in `dev.slne.surf.player:surf-player-api` (Paket `dev.slne.surf.player.api.chat`); die Plugins heißen `surf-player-paper` und `surf-player-minestom`. Admin-Befehle findest du unter `/surfplayer chat ...` (früher `/surfchat`). Die Denylist wurde entfernt.
{% endhint %}

surf-chat stellt ein vollständiges Chat-Management-System für Paper- und Minestom-Server bereit. Es unterstützt verschiedene Nachrichtentypen, ein mehrstufiges Verarbeitungssystem mit Chat-Prozessoren, automatische Moderation, eine Ignore-Liste sowie einen durchsuchbaren Nachrichtenverlauf.

### Erste Schritte

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td>⚡ <strong>Installation</strong></td><td>surf-chat als Abhängigkeit einbinden</td><td><a href="getting-started/installation.md">installation.md</a></td></tr><tr><td>💬 <strong>SurfChatApi</strong></td><td>Das zentrale API-Interface kennenlernen</td><td><a href="grundlagen/surf-chat-api.md">surf-chat-api.md</a></td></tr><tr><td>📨 <strong>Nachrichten</strong></td><td>MessageData, MessageType und MessageContext verstehen</td><td><a href="grundlagen/nachrichten.md">nachrichten.md</a></td></tr></tbody></table>

### Funktionen

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td>⚙️ <strong>Chat-Prozessoren</strong></td><td>Nachrichten vor und nach dem Senden verarbeiten</td><td><a href="funktionen/chat-processors.md">chat-processors.md</a></td></tr><tr><td>🔀 <strong>Message Redirector</strong></td><td>Nachrichten an externe Systeme weiterleiten</td><td><a href="funktionen/message-redirector.md">message-redirector.md</a></td></tr><tr><td>📜 <strong>Nachrichtenverlauf</strong></td><td>Gesendete Nachrichten durchsuchen und verwalten</td><td><a href="funktionen/nachrichtenverlauf.md">nachrichtenverlauf.md</a></td></tr><tr><td>🚫 <strong>Ignore-Liste</strong></td><td>Nachrichten von bestimmten Spielern ausblenden</td><td><a href="funktionen/ignorieren.md">ignorieren.md</a></td></tr><tr><td>✅ <strong>Nachrichtenvalidierung</strong></td><td>Validierungsergebnisse und Fehlerfälle</td><td><a href="grundlagen/nachrichtenvalidierung.md">nachrichtenvalidierung.md</a></td></tr></tbody></table>

### Externe Links

* [GitHub – surf-player Repository](https://github.com/SLNE-Development/surf-player)
* [GitHub – Chat-API](https://github.com/SLNE-Development/surf-player/tree/version/26.3/api/src/main/kotlin/dev/slne/surf/player/api/chat)
* [GitHub – Migration von Befehlen, Permissions und API](https://github.com/SLNE-Development/surf-player/blob/version/26.3/docs/command-migration.md)
