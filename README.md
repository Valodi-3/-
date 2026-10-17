# The House Remembers — Game Design Phase 7 Specifications (Godot 4 2D Top-Down)

> **Статус документа**: Полная геймдизайн-спецификация Фазы 7 для коммерческого психологического хоррора на Godot 4 (2D Top-Down, Android-first).

Этот репозиторий содержит официальную документацию и проектную спецификацию для игры **«The House Remembers»**.

## Структура Документации Фазы 7

1. **[GAME_DESIGN_V2.md](./GAME_DESIGN_V2.md)** — Основной геймдизайн-документ: концепция, 100% платная коммерческая модель, игровой цикл, сенсорное управление Android, совместимость с `user://save_game.json` и режимы сложности (Normal, Hard, Nightmare).
2. **[ENEMY_AI_DESIGN.md](./ENEMY_AI_DESIGN.md)** — Подробный дизайн ИИ трех противников (K-01 Caretaker, K-02 Servant, K-03 Observer) в единой сюжетной кампании: автоматы состояний (FSM), зрение, слух, способности, обман и правила защиты от неизбежных смертей.
3. **[LEVEL_PROGRESSION.md](./LEVEL_PROGRESSION.md)** — Детальная спецификация 5 уровней (`The Entrance`, `The Gallery`, `The Bedrooms`, `The Archives`, `The Heart`): планы комнат, цели, дуальные решения, головоломки, чекпоинты и критерии приёмки.
4. **[ENDINGS_AND_STORY.md](./ENDINGS_AND_STORY.md)** — Нарративный дизайн и разветвления 4 сюжетных концовок (*A. The Escape*, *B. The Truth*, *C. The Replacement*, *D. The Last Memory*), эскорт Марка и предупреждения игрока.
5. **[IMPLEMENTATION_BACKLOG.md](./IMPLEMENTATION_BACKLOG.md)** — Технический бэклог задач для команды Antigravity с приоритетами (P0/P1), зависимостями и оценкой в Story Points (SP).
6. **[SYSTEM_DESIGN.md](./SYSTEM_DESIGN.md)** — Системный дизайн и архитектурные схемы интеграции с Godot 4.
