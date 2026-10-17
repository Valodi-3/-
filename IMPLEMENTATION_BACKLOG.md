# Бэклог Реализации и Техническое ТЗ: «The House Remembers» (Phase 7)

> **Статус документа**: Приоритезированный бэклог задач и дорожная карта разработки для команды Antigravity. Все интерфейсы Godot и интеграции помечаются как предлагаемые до проверки реального кода.

---

## 1. Архитектура 2D Сцен Godot 4.x

```
root (Window)
└── Main (Node)
    ├── MansionMemory (Node - Autoload Singleton)
    ├── SaveSystem (Node - Autoload Singleton)
    ├── LevelManager (Node2D)
    │   └── CurrentLevel (Node2D)
    │       ├── TileMapLayer_Ground (TileMapLayer)
    │       ├── TileMapLayer_Walls (TileMapLayer - StaticBody2D)
    │       ├── NavigationRegion2D (NavigationRegion2D)
    │       ├── EnvironmentObjects (Node2D)
    │       │   ├── Doors (Area2D / StaticBody2D)
    │       │   ├── HidingSpots (Area2D)
    │       │   └── InteractivePuzzles (Node2D)
    │       └── Checkpoints (Node2D)
    ├── Player (CharacterBody2D)
    │   ├── CollisionShape2D
    │   ├── Camera2D
    │   ├── InteractionRayCast2D (RayCast2D)
    │   ├── PlayerLight2D (PointLight2D)
    │   └── PlayerSoundArea2D (Area2D)
    ├── Enemies (Node2D)
    │   ├── Caretaker_K01 (CharacterBody2D)
    │   ├── Servant_K02 (CharacterBody2D)
    │   └── Observer_K03 (CharacterBody2D)
    └── UILayer (CanvasLayer)
        ├── HUD (Control)
        │   ├── VirtualJoystick (Control)
        │   └── ContextButton (TouchScreenButton)
        ├── JournalUI (Control)
        └── GameOverUI (Control)
```

---

## 2. Приоритетизированный Бэклог Задач (Task Backlog)

Задачи ранжированы по приоритету (P0 — Критический блокер, P1 — Высокий приоритет, P2 — Средний приоритет) с указанием зависимостей и оценки в Story Points (SP).

| ID | Приоритет | Название задачи | Зависимости | Сложность (SP) | Описание и Критерии Приёмки |
|---|---|---|---|---|---|
| **TASK-01** | **P0** | Сверка API `MansionMemory.gd` | Нет | **2 SP** | Сверить предложенные методы `record_habit_action` и `process_decay` с существующим синглтоном Antigravity. |
| **TASK-02** | **P0** | ИИ K-01 (The Former Caretaker) | TASK-01 | **5 SP** | Реализовать FSM K-01 (`Patrol`, `Investigate`, `Chase`, `SearchHiding`) с помощью `NavigationAgent2D`. |
| **TASK-03** | **P0** | Реализация 15 Объектов Окружения | TASK-01 | **8 SP** | Разработать 2D-узлы дверей, клиньев, укрытий, половиц и светильников с группой `"Interactable"`. |
| **TASK-04** | **P1** | ИИ K-02 (The House's Servant) | TASK-02 | **8 SP** | Реализовать FSM K-02, реакцию на УФ-свет, слияние со стенами и смену флангов. |
| **TASK-05** | **P1** | ИИ K-03 (The Observer) | TASK-04 | **8 SP** | Реализовать FSM K-03 с опросом доминантных привычек из `MansionMemory` и фазой оглушения. |
| **TASK-06** | **P1** | Сборка Уровней 1 и 2 | TASK-02, TASK-03 | **8 SP** | Сборка `The Entrance` и `The Gallery` с альтернативными путями и головоломками. |
| **TASK-07** | **P1** | Сборка Уровней 3 и 4 | TASK-04, TASK-06 | **8 SP** | Сборка `The Bedrooms` и `The Archives` с механикой затухания памяти и затопленными зонами. |
| **TASK-08** | **P1** | Сборка Уровня 5 и 4 Концовки | TASK-05, TASK-07 | **13 SP** | Реализация `The Heart`, ветвления 4 концовок (A, B, C, D) и терминалов слома предсказаний. |
| **TASK-09** | **P2** | Сохранения и Миграция (`save_game.json`) | TASK-01 | **3 SP** | Интеграция сохранения прогресса с полной обратной совместимостью прошлых сейвов. |
| **TASK-10** | **P2** | Полировка UI Android и Тесты | TASK-08 | **5 SP** | Проверка контекстной кнопки, джойстика и баланса режимов Normal/Hard/Nightmare. |

---

## 3. Граф Зависимостей (Dependency Graph)

```
[TASK-01: MansionMemory Verification]
       │
       ├───► [TASK-02: K-01 Caretaker AI] ────► [TASK-04: K-02 Servant AI] ────► [TASK-05: K-03 Observer AI]
       │                 │                                │                                │
       ├───► [TASK-03: Environment Objects]              │                                │
       │                 │                                │                                │
       │                 ▼                                ▼                                ▼
       │       [TASK-06: Levels 1-2] ───────────► [TASK-07: Levels 3-4] ───────────► [TASK-08: Level 5 & Endings]
       │                                                                                   │
       └───────────────────────────────────────────────────────────────────────────────────┴──► [TASK-10: Polish & Testing]
```

---
*Бэклог готов к взятию в работу командой Antigravity.*
