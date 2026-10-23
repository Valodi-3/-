# The House Remembers — Полный Производственный План (Phase 11 Blueprint)

> **Статус проекта**: Полная техническая и геймдизайн спецификация Фазы 11 по превращению Godot 4.7.2 прототипа «The House Remembers» в коммерческую 3D/2.5D психологическую хоррор-игру (Android-First, Godot Compatibility Renderer, 100% Paid Commercial Model).

Главный принцип проекта:
> **«The House Remembers — это запоминающаяся психологическая хоррор-игра, в которой дом изучает привычки игрока, а игрок учится обманывать дом.»**

---

## Navigation & Master Documentation Index (Фаза 11)

### 0. Технический Аудит и Отчёты Доставки
* **[TECHNICAL_AUDIT_AND_DEFECTS.md](./TECHNICAL_AUDIT_AND_DEFECTS.md)** — Аудит дефектов (B-01 аудио с неустановленной первопричиной), гипотезы (RAM, draw calls, sticking) и реализованные Codex фичи.
* **[TEAM_RESPONSIBILITIES_AND_STATUS.md](./TEAM_RESPONSIBILITIES_AND_STATUS.md)** — Чёткое разделение ответственности между Manus (UI/UX), Antigravity (Visual 3D Assets) и Codex (Implementation/Engine/APK).
* **[ACCEPTANCE_CRITERIA_AND_METRICS.md](./ACCEPTANCE_CRITERIA_AND_METRICS.md)** — Измеримые критерии приёмки, профили производительности (Low 30 FPS / Balanced / High 60 FPS) и нормативные цели.
* **[DELIVERY_AND_SUMMARY.md](./DELIVERY_AND_SUMMARY.md)** — Итоговый отчёт о передаче материалов (без сюжетных спойлеров) и подтверждение доступности файлов.

### 1. Фундамент и Архитектура 3D / 2.5D
* **[PRODUCTION_BIBLE.md](./PRODUCTION_BIBLE.md)** — Единое видение игры, дизайн-столпы, коммерческий профиль Android и нарративные границы.
* **[2_5D_TECHNICAL_ARCHITECTURE.md](./2_5D_TECHNICAL_ARCHITECTURE.md)** — Техническая архитектура 3D рендеринга (MeshInstance3D, Skeleton3D, Camera3D, Godot Compatibility Renderer).

### 2. Игровой Процесс и Искусственный Интеллект
* **[COMPLETE_GAMEPLAY_SYSTEMS.md](./COMPLETE_GAMEPLAY_SYSTEMS.md)** — Переработанный игровой цикл, реестр интерактивных механик (стелс, свет, двери, укрытия, дверные клинья, масленка, бутылки, головоломки) и 15 динамических событий особняка.
* **[ENEMY_AI_V3.md](./ENEMY_AI_V3.md)** — Полная спецификация 3 противников (K-01 Caretaker, K-02 Servant, K-03 Observer): FSM, конусы зрения и слуха, физическое присутствие без телепортации, антизастревание (`StuckDetector`) и кайт-баланс.
* **[MANSION_MEMORY_V3.md](./MANSION_MEMORY_V3.md)** — Архитектура `MansionMemory 2.0`: метрики привычек, коэффициенты затухания (Decay), система блефа и 3 независимых побарьерных правила Fallback для 100% проходимости Уровня 5.

### 3. Дизайн Уровней, Нарратив и Концовки
* **[LEVELS_MASTER_PLAN.md](./LEVELS_MASTER_PLAN.md)** — Мастер-план и спецификация 5 уровней (`The Entrance`, `The Gallery`, `The Bedrooms`, `The Archives`, `The Heart`): планы комнат, ключевые предметы, дуальные решения, безопасные комнаты и критерии приёмки.
* **[STORY_AND_FOUR_ENDINGS.md](./STORY_AND_FOUR_ENDINGS.md)** — Нарративный дизайн, механика эскорта Марка, точки осознанного выбора, защита от случайных сработок и подробные условия 4 сюжетных концовок (*A. The Escape*, *B. The Truth*, *C. The Replacement*, *D. The Last Memory*).

### 4. Производственные Пайплайны, Аудио и Мобильный UX
* **[ART_AND_ASSET_PIPELINE.md](./ART_AND_ASSET_PIPELINE.md)** — Графический 3D-пайплайн: MeshInstance3D, Skeleton3D, полигональный бюджет, модульные 3D-тайлсеты и сжатие VRAM.
* **[AUDIO_AND_CINEMATICS.md](./AUDIO_AND_CINEMATICS.md)** — Звуковой саундскейп: материалы полов, звуковые автографы врагов и план монтажа роликов Google Flow (`.ogv`).
* **[ANDROID_PERFORMANCE_AND_UX.md](./ANDROID_PERFORMANCE_AND_UX.md)** — Мобильное управление (виртуальный джойстик + контекстная кнопка), масштабирование UI (360–390dp), профили графики (Low 30 FPS / Balanced / High 60 FPS) и настройки доступности.

### 5. Качество, Тестирование и Дорожная Карта
* **[QA_ACCEPTANCE_MATRIX.md](./QA_ACCEPTANCE_MATRIX.md)** — Полная QA-матрица сценариев тестирования, защита от soft-lock и объективные критерии готовности к релизу (Definition of Done).
* **[ANTIGRAVITY_IMPLEMENTATION_ROADMAP.md](./ANTIGRAVITY_IMPLEMENTATION_ROADMAP.md)** — Пошаговый практический план разработки с учетом Фаз 9–10 и 11A, включающий приоритезированный backlog (P0, P1, P2).

---

## Предыдущие Спецификации Фазы 7 (Архив / Справочно)
* [GAME_DESIGN_V2.md](./GAME_DESIGN_V2.md) — Исходный геймдизайн Фазы 7.
* [ENEMY_AI_DESIGN.md](./ENEMY_AI_DESIGN.md) — Исходный дизайн ИИ.
* [LEVEL_PROGRESSION.md](./LEVEL_PROGRESSION.md) — Исходный план уровней.
* [ENDINGS_AND_STORY.md](./ENDINGS_AND_STORY.md) — Исходные концовки.
* [IMPLEMENTATION_BACKLOG.md](./IMPLEMENTATION_BACKLOG.md) — Технический аудит компонентов.
* [SYSTEM_DESIGN.md](./SYSTEM_DESIGN.md) — Исходный системный дизайн Godot 4 2D Top-Down.
