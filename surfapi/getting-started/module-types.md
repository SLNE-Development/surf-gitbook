---
icon: crop-simple
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
metaLinks:
  alternates:
    - https://app.gitbook.com/s/yE16Xb3IemPxJWydtPOj/getting-started/quickstart
---

# Modul Arten

## Modul Arten

In surf-api gibt es verschiedene Arten von Modulen, die jeweils unterschiedliche Funktionen und Anwendungsbereiche haben.

### Api Module

Das Api-Modul des Projektes kann veröffentlicht werden und Zugriff auf die Funktionen des Projektes bieten. Basisklassen, welche in jedem Modul benötigt werden, sowohl welche die Api nutzen, als auch welche die nur intern genutzt werden, sollten in diesem Modul liegen. Das Api-Modul sollte die [`core`](installation.md) Abhängigkeit beinhalten.

## Core Module

Das Core-Modul des Projektes beinhaltet Klassen und Logik, welche von anderen internen Modulen genutzt werden können, aber nicht öffentlich über die Api zugänglich sind. Das Core-Modul sollte das Api Modul die [`core`](installation.md) Abhängigkeit beinhalten.

## Fallback Module

Das Fallback-Modul des Projektes beinhaltet Datenbank Logik, welche durch \[surf-database] bereitgestellt wird. Dieses wird nur benötigt, wenn eine Datenbankanbindung genutzt wird und kein Cloud-Plugin vorliegt.

## Paper Module

Das Paper Module des Projektes beinhaltet jegliche Logik, welche auf dem Paper Server läuft. Die Hauptklasse des Moduls sollte `PaperMain` heißen. Das Paper Module sollte das Core Modul und die [`paper`](installation.md) Abhängigkeit beinhalten.

## Velocity Module

Das Velocity Module des Projektes beinhaltet jegliche Logik, welche auf dem Velocity Proxy läuft. Die Hauptklasse des Moduls sollte `VelocityMain` heißen. Das Velocity Module sollte das Core Modul und die [`velocity`](installation.md) Abhängigkeit beinhalten.

{% hint style="warning" %}
**Hinweis:** Cloud Module sind anders aufgebaut. Für weitere Informationen siehe das \[Cloud Dokumentation]
{% endhint %}
