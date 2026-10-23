# ТЕХНИЧЕСКАЯ АРХИТЕКТУРА 3D / 2.5D (2_5D_TECHNICAL_ARCHITECTURE.md)

> **Статус документа**: Техническая архитектура и спецификация рендеринга Фазы 11 для игры «The House Remembers» на движке Godot 4.7.2 (Android, Godot Compatibility Renderer).

---

## 1. Главный Архитектурный Принцип Рендеринга

> **ВАЖНОЕ ТЕХНИЧЕСКОЕ И ХУДОЖЕСТВЕННОЕ НАПРАВЛЕНИЕ**:
> **Актуальная версия «The House Remembers» использует настоящие 3D-сцены, 3D-меши (`MeshInstance3D`), скелетные 3D-модели персонажей (`Skeleton3D`), `Camera3D` и рендерер Godot Compatibility (OpenGL ES 3.0 / WebGL 2.0).**
> **Устаревшие требования к 2D-спрайтам не должны определять новую художественную работу.**

---

## 2. Иерархия 3D Дерева Сцен (3D Scene Tree Structure)

```
Level_Main (Node3D)
├── WorldEnvironment (WorldEnvironment - Fog, Ambient)
├── DirectionalLight3D / OmniLight3D (Lighting)
├── Environment3D (Node3D)
│   ├── GridMap / StaticBody3D (Walls, Floors, Doors)
│   └── InteractiveProps3D (Node3D / Area3D)
├── Camera3D (Camera3D - Fixed / Tracking Angle)
├── Player3D (CharacterBody3D)
│   ├── CollisionShape3D
│   ├── Skeleton3D & MeshInstance3D (Player Mesh)
│   ├── Flashlight3D (SpotLight3D)
│   └── InteractionRayCast3D (RayCast3D)
├── Keeper3D (CharacterBody3D)
│   ├── CollisionShape3D
│   ├── NavigationAgent3D
│   ├── Skeleton3D & MeshInstance3D (Enemy Mesh)
│   └── VisionArea3D (Area3D + RayCast3D Grid)
└── UILayer (CanvasLayer)
    ├── HUD (Control)
    └── ContextButton (TouchScreenButton)
```

---

## 3. Ограничения Рендерера Godot Compatibility на Android

1. **Материалы и Шейдеры**: Использование PBR-материалов (`StandardMaterial3D`) со стандартными картами Albedo, Roughness, Normal.
2. **Источники Света**: Лимит не более 3 активных точечных/конусных источников света (`OmniLight3D`, `SpotLight3D`) одновременно в кадре.
3. **Оптимизация Мешей**: Статический batching и LOD для повторно используемых 3D-элементов окружения.

---
*Спецификация утверждена для команды Antigravity и Codex в Godot 4.7.2.*
