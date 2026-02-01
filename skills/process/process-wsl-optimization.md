---
id: process-wsl-optimization
title: WSL2 & Docker Optimization Guide
scope: skills-mbb
tags: [#wsl, #docker, #optimization, #windows, #hardware]
priority: medium
created_at: 2026-01-31
updated_at: 2026-02-01
---

# WSL2 & Docker Optimization Guide

> **Context Aware**: Настройки WSL зависят от активного профиля оборудования.
> **Current Profile**: См. `INFRASTRUCTURE_CONFIG.yaml` -> `profiles.active`.

## 1. Hardware-Specific Configuration (`.wslconfig`)

Файл `.wslconfig` (в `C:\Users\[User]\`) управляет ресурсами виртуальной машины Linux.
Значения должны соответствовать вашему текущему оборудованию (Home/Office).

### Profile: Home (High Performance)
*Target: Intel Core i5-12600K (16 threads) / 64GB RAM*

```ini
[wsl2]
# Выделяем 12 потоков (оставляем 4 E-cores для Windows фоновых задач)
processors=12

# Выделяем 32GB ОЗУ (50% от 64GB)
# Этого достаточно для n8n + AI моделей, при этом Windows остается отзывчивой
memory=32GB

# Swap файл (резерв)
swap=8GB

# Отключение GUI экономит ресурсы
guiApplications=false
localhostForwarding=true

[experimental]
# Критически важно для 64GB: авто-возврат неиспользуемой памяти
autoMemoryReclaim=gradual
dnsTunneling=true
autoDiskShrink=true
```

### Profile: Office (Standard / Unknown)
*Target: Standard Laptop/PC (~16-32GB RAM)*

```ini
[wsl2]
processors=4
memory=8GB
swap=4GB
guiApplications=false
localhostForwarding=true

[experimental]
autoMemoryReclaim=gradual
```

## 2. Docker Desktop Settings

### Resources
- **WSL Integration**: Убедитесь, что ваш дистрибутив (Ubuntu-22.04) включен.
- **Resource Saver**: Для 64GB RAM можно поставить `Mode: Auto` (или отключить, если нужны всегда горячие сервисы).

### Features (Critical for MBB)
- **Use containerd**: Enable (Improved image storage).
- **Beta Features / Docker MCP Toolkit**: Enable (Для работы MCP).
- **Kubernetes**: **DISABLE** (Экономит ресурсы, не используется в MBB).

## 3. Applying Changes

После изменения `.wslconfig`:

1. Откройте PowerShell.
2. Выполните полную перезагрузку WSL:
   ```powershell
   wsl --shutdown
   ```
3. Запустите Docker Desktop.
4. Проверьте выделенные ресурсы в WSL:
   ```bash
   # В терминале Ubuntu/Git Bash
   free -h   # Показать память
   nproc     # Показать ядра
   ```

## Related Skills
- [Infrastructure Config (SSOT)](../../INFRASTRUCTURE_CONFIG.yaml)
- [Disaster Recovery Protocol](./process-disaster-recovery.md)
