# qSecuritySystem API

    

qsecuritysystem-api is the official integration layer for qSecuritySystem.
It provides a stable, type-safe contract for plugins, addons, and external services to interact with moderation features.


---

## Overview

This API allows you to:

- Manage punishments (ban, mute, kick)

- Work with reports system

- Control player checks

- Access moderation tools (freeze, vanish)

- Read player security data

- Track staff activity


> ⚠️ Requires qSecuritySystem (core plugin) to be installed.




---

## Getting Started

### Installation

Gradle (Kotlin DSL)
```java
dependencies {
    compileOnly("com.qustaneu:qsecuritysystem-api:1.0.0")
}
```
Maven
```java
<dependency>
    <groupId>com.qustaneu</groupId>
    <artifactId>qsecuritysystem-api</artifactId>
    <version>1.0.0</version>
    <scope>provided</scope>
</dependency>
```
> ❗ Do NOT shade the API into your plugin.




---

## Server Setup

Make sure you installed:

- qSecuritySystem (core plugin)

- qSecuritySystemAPI (API bootstrap)



---

## Getting API Instance

```java
import com.qustaneu.qsecuritysystem.api.QSecuritySystemApi;
import org.bukkit.Bukkit;
import org.bukkit.plugin.RegisteredServiceProvider;

RegisteredServiceProvider<QSecuritySystemApi> provider =
        Bukkit.getServicesManager().getRegistration(QSecuritySystemApi.class);

QSecuritySystemApi api = null;

if (provider != null) {
    api = provider.getProvider();
}
```

---

## Safety Check

```java
if (api == null || !api.isAvailable()) {
    getLogger().warning("qSecuritySystem API is not available!");
    return;
}
```

---

## Usage Examples

### Punishments

```java
boolean banned = api.punishments().isBanned(uuid);
boolean muted = api.punishments().isMuted(uuid);

api.punishments().issuePunishment(
        targetUUID,
        staffUUID,
        "Spam",
        "MUTE",
        System.currentTimeMillis() + 3600000
);
```

---

## Reports

```java
api.reports().createReport(
        reporterUUID,
        targetUUID,
        "Cheating"
);

api.reports().getOpenReports();
api.reports().closeReport(reportId);
```

---

## Checks

```java
api.checks().startCheck(targetPlayer, staffPlayer);
api.checks().endCheck(targetUUID);
```


---

## Moderation Tools

```java
boolean frozen = api.moderation().isFrozen(uuid);
boolean vanished = api.moderation().isVanished(uuid);

api.moderation().setFrozen(uuid, true);
api.moderation().setVanished(uuid, true);
```

---

Player Security

```java
api.players().isFrozen(uuid);
api.players().isFlagged(uuid);
```

---

## Staff Logs
```java
api.staffLogs().getRecent();
```

---

## API Structure

```java
api.punishments()
api.reports()
api.checks()
api.players()
api.moderation()
api.staffLogs()
```

---

## ⚠️ Limitations

claimReport → not implemented

isStaffModeEnabled / isFlagged → default values

Staff logs are currently in-memory

Some moderation features are contract-only



---

## ⚠️ Common Mistakes

❌ Shading the API into your plugin

❌ Not checking api.isAvailable()

❌ Using API without core plugin

❌ Using wrong methods (getPunishmentApi())



---

## 🧠 Best Practices

Always validate API availability

Use only exposed interfaces

Avoid direct dependency on internal logic

Handle null safely



---

## 🏗 Architecture

The API module contains only:

Interfaces

Models / records

Enums

Result objects


✔ Ensures loose coupling
✔ Safe updates
✔ Stable integrations


---

## Links

- Core plugin: https://www.curseforge.com/minecraft/bukkit-plugins/qsecuritysystem


