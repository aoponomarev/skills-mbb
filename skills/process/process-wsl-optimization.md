---
id: process-wsl-optimization
title: WSL2 & Docker Optimization Guide
scope: skills-mbb
tags: [#wsl, #docker, #optimization, #windows]
priority: medium
created_at: 2026-01-31
updated_at: 2026-01-31
---

# WSL2 & Docker Optimization Guide

> **Performance Note**: Настройки оптимизированы под систему с 64 ГБ ОЗУ и 16-ядерным процессором (i5-12600K) для предотвращения "жора" памяти процессом `Vmmem`.

## 1. WSL 2 Configuration (`.wslconfig`)

Этот файл ограничивает аппетиты виртуальной машины Linux.
**Location**: `C:\Users\[User]\.wslconfig`

```ini
[wsl2]
# Выделение 8 из 16 ядер процессора для стабильной работы
processors=8
# Выделение 16 ГБ ОЗУ (достаточно для n8n + Continue + Docker)
memory=16GB
# Резервный файл подкачки
swap=8GB
# Отключение GUI-приложений для экономии ресурсов
guiApplications=false
# Включение сквозной пересылки портов (доступ по localhost)
localhostForwarding=true

[experimental]
# Автоматический возврат неиспользуемой памяти в Windows
autoMemoryReclaim=gradual
# Улучшенная обработка DNS (предотвращает проблемы с сетью в контейнерах)
dnsTunneling=true
# Автоматическое сжатие виртуального диска (экономит место на C:)
autoDiskShrink=true
```

## 2. Docker Desktop Settings

### Resources
- **WSL Integration**: Убедись, что твоя Linux-дистрибуция (например, Ubuntu-22.04) включена в настройках Docker Desktop > Resources > WSL Integration.

### Features
- **Use containerd**: Enable (Improved image storage)
- **Beta Features / Docker MCP Toolkit**: Enable (Critical for Skills MCP)
- **AI / Docker Model Runner**: Enable (GPU acceleration support)
- **Kubernetes**: **DISABLE** (Saves ~4-8GB RAM immediately)

### Apply Changes
После изменения настроек Docker Desktop или `.wslconfig`:
```powershell
wsl --shutdown
```
Затем запустите Docker Desktop заново.

## 3. PowerShell & Encoding

Для корректной работы скриптов и Docker логов в Windows:
```powershell
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
$env:MSYS_NO_PATHCONV = 1  # Для Git Bash
```

## Related Skills
- [Disaster Recovery Protocol](./process-disaster-recovery.md)
- [Windows & Docker Paths](./process-windows-docker-paths.md)
