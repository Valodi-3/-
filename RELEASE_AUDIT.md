# THE HOUSE REMEMBERS — INDEPENDENT RELEASE AUDIT REPORT (REVISED)

> **Date of Audit**: October 2026
> **Target Project**: *The House Remembers* (Godot Engine 4.7.2, Godot Compatibility Renderer, Android-First)
> **Auditor**: Jules (Independent Software Engineering Auditor)
> **Audit Status**: Documentary Technical Audit & Delivery Verification (Документальный технический аудит и аудит поставок)

---

## 1. EXECUTIVE SUMMARY & CODEBASE AVAILABILITY NOTICE

### Codebase Availability Check
As instructed, the availability of the target Godot source code version was evaluated:
* **Requested Target Branch/Commit**: `integrate/manus-ui-stage2`, commit `76da914`.
* **Repository Status**: The current workspace `Valodi-3/-` is a documentation and delivery repository containing specification documents, reports, and archive files.
* **Finding**: Commit `76da914` and branch `integrate/manus-ui-stage2` are **NOT AVAILABLE** in this repository. No executable Godot source files (`project.godot`, `.gd`, `.tscn`) exist in the working tree.
* **Audit Execution**: Per instructions (*"Если нет, выполни только документальный аудит и явно отметь ограничения"*), an independent documentary audit was performed based on specification documents, Codex delivery reports, and delivery files.

---

## 2. STRICTLY CATEGORIZED AUDIT FINDINGS

### A. Подтверждённые Дефекты Документации (Confirmed Documentation Defects)

1. **DOC-01: Архитектурный Конфликт Документации (2D Top-Down vs 3D Compatibility)**
   * **Описание**: В репозитории присутствует устаревшая спецификация `SYSTEM_DESIGN.md`, описывающая игру как 2D Top-Down на нодах `CharacterBody2D`, `NavigationAgent2D`, `Area2D` и `TileMapLayer`. В то же время актуальные руководящие документы (`PRODUCTION_BIBLE.md`, `2_5D_TECHNICAL_ARCHITECTURE.md`, `TEAM_RESPONSIBILITIES_AND_STATUS.md`, `README.md`) утверждают, что игра выстроена на полноценных 3D-сценах (`MeshInstance3D`, `Skeleton3D`, `Camera3D`, `NavigationAgent3D`, Godot Compatibility renderer).
   * **Риск**: Вводит в заблуждение разработчиков и QA при сверке нод и контроле спецификаций.

---

### B. Факты из Отчётов Codex (Независимо Не Проверенные Jules)

Следующие положения зафиксированы в отчётах команд Codex/Stage 7 и приняты к сведению, но **не могут быть независимо подтверждены Jules** из-за отсутствия исходного кода Godot в данном репозитории:

1. **CODEX-01: Автоматическое Тестирование и 12/12 Маршрутов**
   * *Статус Codex*: В актуальном проекте Stage 7 созданы и запущены автоматические Godot-тесты, подтверждающие 12/12 производственных маршрутов QA-матрицы.
   * *Примечание Jules*: Отсутствие GDScript-тестов в репозитории `Valodi-3/-` относится только к данному документационному репозиторию и не означает отсутствие тестов в целевом игровом проекте.

2. **CODEX-02: Подписанная Сборка Android APK (Stage 7)**
   * *Статус Codex*: Подготовлена подписанная сборка Android APK со свойствами: `minSdk 24`, `targetSdk 36`, архитектуры `arm64-v8a` и `armeabi-v7a`.
   * *Примечание Jules*: Отсутствие файла `export_presets.cfg` в репозитории `Valodi-3/-` является особенностью документационной структуры и не свидетельствует об отсутствии конфигурации экспорта в актуальном игровом репозитории.

3. **CODEX-03: Согласованная Очистка Аудиоресурсов (Stage 4A/4B)**
   * *Статус Codex*: В рамках работ Stage 4A/4B проведена проверка и реализована явная выгрузка аудиосэмплов и сброс потоков (`stream = null`).
   * *Примечание Jules*: Остаточный риск B-01 сохраняется исключительно как специфический краевой случай при принудительном завершении процесса в headless-режиме (forced headless shutdown).

4. **CODEX-04: Реестр Лицензий Ассетов (Stage 7)**
   * *Статус Codex*: В Stage 7 сформирован учет сторонних ассетов и прав на медиаконтент.
   * *Примечание Jules*: В документационном репозитории сохраняются открытыми отдельные вопросы по явной документальной фиксации коммерческих прав на некоторые сэмплы и AI-генерированные OGV-видеоролики Google Flow, что требует финального подтверждения перед релизом.

5. **CODEX-05: Посторонние ZIP-Архивы в Документационном Репозитории**
   * *Статус Codex*: Файлы `shipthatcode-python-fundamentals-python.zip` и `the_house_remembers_documentation_delivery.zip` расположены в корню документационного репозитория `Valodi-3/-`.
   * *Примечание Jules*: Наличие этих файлов в `Valodi-3/-` не означает их попадания в итоговый APK-пакет при сборке в Godot.

---

### C. Гипотезы, Требующие Доступа к Актуальной Игре (Requires Verification)

Следующие пункты **не являются подтверждёнными дефектами**, а представляют собой технические гипотезы, требующие аудита и проверки непосредственно в исходном коде Godot (коммит `76da914`):

1. **HYP-01: Атомность Записи в `SaveManager`**
   * *Описание*: Требуется проверка в коде `SaveManager.gd`, используется ли временный файл (`save_game.json.tmp`) с последующим атомарным переименованием для защиты от сбоев питания на Android.

2. **HYP-02: Баланс Скоростей на Сложности Nightmare**
   * *Описание*: Требуется проверка в коде противника K-03, обеспечен ли достаточный запас скорости игрока при спринте ($230\text{ px/s}$) относительно K-03 ($215\text{ px/s}$) для комфортного ввода с виртуального джойстика на мобильных тачскринах.

3. **HYP-03: Динамическая Синхронизация Субтитров в `VoiceManager`**
   * *Описание*: Требуется проверка в `VoiceManager.gd`, рассчитывается ли длительность показа субтитров динамически по длине `AudioStream.get_length()` или используются статические таймеры.

4. **HYP-04: Метрики Памяти (RAM > 1 GB) и Draw Calls (> 180)**
   * *Описание*: Заявления о повышенном RAM/Draw Calls переведены в статус гипотез H-01 и H-02, требующих снятия дампов с Godot Profiler.

---

### D. Проверки, Возможные Только на Физическом Android (Physical Hardware Required)

Следующие характеристики невозможно подтвердить или опровергнуть без проведения натурных тестов на реальных мобильных устройствах:

1. **Стабильность Кадровой Частоты (FPS)**: Валидация профиля Low ($30\text{ FPS}$ на бюджетных ARM64 устройствах) и профиля High ($60\text{ FPS}$) при нагреве и троттлинге процессора.
2. **Отклик Сенсорного Управления**: Проверка мертвых зон `VirtualJoystick` ($10\% - 30\%$), динамического позиционирования `ContextButton` и отсутствия фантомных нажатий при мультитаче.
3. **Поведение Аудиодрайвера Android**: Проверка выгрузки ресурсов `AudioStreamWAV`/`AudioStreamOggVorbis` через драйверы OpenSL ES / AAudio на уровне ОС Android.
4. **Компиляция Шейдеров и Загрузка VRAM**: Проверка отсутствия фризов при первой загрузке материалов на GPU Mali и Adreno с текстурным сжатием ASTC/ETC2.

---

## 3. ДЕТАЛЬНАЯ ОЦЕНКА ПО 10 ПУНКТАМ ТЕХНИЧЕСКОГО ЗАДАНИЯ

1. **Архитектура Godot-проекта**: Зафиксирован подтверждённый документальный дефект (DOC-01) из-за наличия устаревшего 2D `SYSTEM_DESIGN.md` при актуальной 3D Compatibility спецификации.
2. **SaveManager и корректность сохранений**: Механизм сохранения `user://save_game.json` и весов `MansionMemory` перенесен в гипотезы (HYP-01), требующие сверки кода.
3. **Состояния AI и связанные сигналы**: FSM противников K-01, K-02, K-03 и предохранитель `StuckDetector` описаны корректно; баланс Nightmare K-03 помещен в HYP-02.
4. **Возможные soft-lock и ошибки переходов**: В спецификациях заложены правила Fallback (3 дефолтных барьера на Уровне 5, сброс ключей через 10с).
5. **AudioManager, VoiceManager и SoundIdentity**: Звуковые автографы и коэффициенты шума покрытий пола зафиксированы; синхронизация субтитров помещена в HYP-03.
6. **Остаточный риск B-01**: Согласованная очистка аудиоресурсов проверена Codex в Stage 4A/4B; B-01 остаётся краевым риском только при принудительном завершении headless-процесса.
7. **Список сторонних ассетов и лицензии**: Учтены результаты Stage 7, открытые вопросы по отдельным ассетам сохранены для итоговой проверки.
8. **Android export settings**: По отчетам Stage 7 подготовлен подписанный APK (minSdk 24, targetSdk 36, arm64-v8a/armeabi-v7a).
9. **Зависимости и файлы в APK**: Посторонние ZIP-файлы присутствуют только в документационном репозитории `Valodi-3/-`.
10. **Наличие тестов**: По отчетам Codex в игровом проекте проведены автоматические Godot-тесты по 12/12 маршрутам.

---

## 4. STATEMENT ON SPOILER & NARRATIVE COMPLIANCE

Данный отчёт составлен в полном соответствии с требованиями конфиденциальности: в нём **отсутствуют сюжетные спойлеры, секреты, описания концовок и ключевые повороты истории**.
