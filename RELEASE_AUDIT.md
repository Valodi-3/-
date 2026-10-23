# THE HOUSE REMEMBERS — INDEPENDENT RELEASE AUDIT REPORT

> **Date of Audit**: October 2026
> **Target Project**: *The House Remembers* (Godot Engine 4.7.2, Godot Compatibility Renderer, Android-First)
> **Auditor**: Jules (Independent Software Engineering Auditor)
> **Audit Status**: Documentary Technical Audit Complete (Документальный технический аудит завершён)

---

## EXECUTIVE SUMMARY & CODEBASE AVAILABILITY NOTICE

### Codebase Availability Check
As instructed in the audit prompt, the availability of the target Godot source code version was evaluated:
* **Requested Target Branch/Commit**: `integrate/manus-ui-stage2`, commit `76da914`.
* **Repository Status**: `Valodi-3/-` contains documentation files, Python zip files, and git history up to commit `bfe967d`.
* **Finding**: Commit `76da914` and branch `integrate/manus-ui-stage2` are **NOT AVAILABLE** in this repository. No Godot source code files (`project.godot`, `.gd`, `.tscn`, `.import`) exist in the working tree.
* **Audit Execution**: Per instructions (*"Если нет, выполни только документальный аудит и явно отметь ограничения"*), an independent documentary audit was conducted based on the technical specifications, architecture blueprints, backlog matrices, and delivery files present in the repository.

---

## CLASSIFIED AUDIT FINDINGS

### Confirmed P0 Issues (Critical Release Blockers)

1. **P0-1: Absence of Executable Automated Test Suite (0% Automated Test Coverage)**
   * **Evidence**: `QA_ACCEPTANCE_MATRIX.md` specifies manual QA steps (TC-LVL-01 to TC-AND-01), but no automated GDScript unit/integration tests (GUT / GdUnit4) exist in the repository or delivery blueprints.
   * **Impact**: Critical user scenarios (saving, MansionMemory decay, AI state transitions, ending triggers) cannot be validated automatically in CI/CD before releasing the APK.
   * **Rule Compliance**: Tests cannot be declared successful without execution (*"Не объявляй тесты успешными без запуска"*). Automated test coverage is currently 0%.

2. **P0-2: Missing Legal Proof of License & Rights for Third-Party Media Assets**
   * **Evidence**: `AUDIO_AND_CINEMATICS.md` lists 16 pilot voice files, Google Flow AI generated `.ogv` video files (`intro_prologue.ogv`, `enemy_reveal_k01.ogv`–`k03.ogv`, 4 ending videos), and sound sample effects. However, no `LICENSE`, `THIRD_PARTY_LICENSES.md`, or copyright grant documents exist in the repository.
   * **Impact**: Commercial paid release (100% Paid Commercial Model) without documented asset licenses introduces legal takedown and copyright liability risks.

---

### Confirmed P1 Issues (High Priority Technical Risks)

1. **P1-1: Architectural Discrepancy Between Technical Specifications (2D Top-Down vs 3D Compatibility)**
   * **Evidence**: `SYSTEM_DESIGN.md` describes the game as a 2D top-down game using `CharacterBody2D`, `NavigationAgent2D`, `Area2D`, and `TileMapLayer`. Conversely, `PRODUCTION_BIBLE.md`, `2_5D_TECHNICAL_ARCHITECTURE.md`, `TEAM_RESPONSIBILITIES_AND_STATUS.md`, and `README.md` state that the game is a real 3D project using `MeshInstance3D`, `Skeleton3D`, `Camera3D`, `NavigationAgent3D`, and Godot Compatibility renderer.
   * **Impact**: High risk of team miscommunication, implementation of mismatched 2D/3D nodes, or broken spatial logic if legacy 2D node references are used during core development.

2. **P1-2: Non-Atomic Save Game Write Mechanism in `SaveManager`**
   * **Evidence**: `MANSION_MEMORY_V3.md` and `TECHNICAL_AUDIT_AND_DEFECTS.md` specify saving game states and MansionMemory weights directly to `user://save_game.json`. No temporary file swapping (`save_game.json.tmp` -> atomic rename) or backup file retention is documented.
   * **Impact**: If a device loses power, suffers an OS kill, or experiences an app crash during the write operation on Android, `save_game.json` will be corrupted/truncated, preventing game loading and breaking save compatibility.

3. **P1-3: Residual Risk B-01 (Unproven Audio Stream Retention in RAM)**
   * **Evidence**: `TECHNICAL_AUDIT_AND_DEFECTS.md` documents Defect B-01 (`AudioStreamWAV` and `AudioStreamOggVorbis` remaining in memory after `AudioStreamPlayer` node deallocation). The specification explicitly notes: *"Первопричина удержания аудиоресурсов пока не доказана"* (Root cause is unproven). Proposed fixes (`stream = null` before `queue_free()`, `SoundPool`) are strictly hypothetical.
   * **Impact**: Unchecked audio sample retention can cause memory consumption to grow during prolonged gameplay on mobile devices, potentially triggering Android OS Out-Of-Memory (OOM) process termination.

4. **P1-4: Extremely Narrow Speed Margins on Nightmare Difficulty & Soft-Lock Potential**
   * **Evidence**: `SYSTEM_DESIGN.md` and `IMPLEMENTATION_BACKLOG.md` list Nightmare player sprint speed as $230\text{ px/s}$ vs K-03 pursuit speed as $215\text{ px/s}$ (only a $15\text{ px/s}$ or $6.5\%$ speed advantage).
   * **Impact**: On touchscreens with virtual joysticks, minor analog input variance or mobile frame drops will cause K-03 to instantly catch the player without counterplay. Furthermore, Mark escort scenarios lack explicit collision recovery if Mark becomes trapped behind doors or AI barriers.

5. **P1-5: Unverified Android Export Presets and VRAM Compression Setup**
   * **Evidence**: `2_5D_TECHNICAL_ARCHITECTURE.md` and `ANDROID_PERFORMANCE_AND_UX.md` target Godot Compatibility on Android ARM64, but `export_presets.cfg` is missing from the repo.
   * **Impact**: Without explicit configuration for ASTC/ETC2 VRAM texture compression, uncompressed or wrong-format textures will inflate APK download size and cause VRAM bus exhaustion on budget Android GPUs (Mali/Adreno).

---

### P2 Issues (Medium Priority / Code Hygiene)

1. **P2-1: Unrelated Archive Files Present in Repository Root**
   * **Evidence**: The file `shipthatcode-python-fundamentals-python.zip` (Python fundamentals course exercise) and `the_house_remembers_documentation_delivery.zip` sit directly in the root directory.
   * **Impact**: If the root folder is exported without `.gdignore` rules or proper export filters, non-game zip archives will be packed into the final APK binary, increasing file size and cluttering the package.

2. **P2-2: Static Subtitle Display Timers in `VoiceManager`**
   * **Evidence**: `TECHNICAL_AUDIT_AND_DEFECTS.md` and `AUDIO_AND_CINEMATICS.md` describe `VoiceManager` handling 16 pilot audio tracks (RU/EN). Subtitle display duration is handled via static timers rather than dynamically reading the duration of `AudioStreamOggVorbis` streams.
   * **Impact**: Potential text desynchronization where subtitles linger after voice playback finishes or vanish before the line completes.

---

### Unconfirmed Risks (Неподтверждённые риски)

As classified in `TECHNICAL_AUDIT_AND_DEFECTS.md`, the following claims lack Godot Profiler logs, memory dumps, or reproducible test traces and remain **unconfirmed hypotheses**:

1. **Hypothesis H-01: RAM Usage Exceeding 1 GB**
   * *Status*: **Unconfirmed**. Requires validation via `Performance.MEMORY_STATIC` or Android Studio Memory Profiler.
2. **Hypothesis H-02: Total Draw Calls Exceeding 180**
   * *Status*: **Unconfirmed**. Requires validation via `Performance.RENDER_TOTAL_DRAW_CALLS_IN_FRAME` in heavy level scenes.
3. **Hypothesis H-03: Enemy AI Physics Sticking in Level Geometry**
   * *Status*: **Unconfirmed**. Architecturally mitigated by the `StuckDetector` safety module; requires video capture and reproducible navigation mesh logs.

---

### Hardware Verification Limits (Что нельзя проверить без физического Android)

The following items **cannot be confirmed or verified** without profiling on physical Android devices:

1. **Actual Frame Rate Stability**: Verifying 30 FPS on Low profile (budget ARM64 devices) and 60 FPS on High profile under thermal throttling conditions.
2. **Multi-Touch Input Latency**: Testing `VirtualJoystick` drag precision, deadzone handling ($10\% - 30\%$), and simultaneous double-tap sprint triggering on physical touchscreens.
3. **Native Driver Audio Memory Behavior**: Confirming whether OpenSL ES / AAudio audio backends in Godot release audio resources upon node destruction on actual Android devices (Defect B-01).
4. **GPU VRAM & Shader Compilation Stutter**: Observing OpenGL ES 3.0 driver behavior, shader compilation hitches on initial light/mesh rendering, and ASTC texture decompression performance on Mali vs Adreno GPUs.

---

## DETAILED AUDIT EVALUATION BY AREA (10 POINTS)

### 1. Godot Project Architecture
* **Status**: Discrepancy identified between legacy 2D docs (`SYSTEM_DESIGN.md`) and current 3D Compatibility specifications (`2_5D_TECHNICAL_ARCHITECTURE.md`).
* **Recommendation**: Formally mark `SYSTEM_DESIGN.md` as legacy reference or update node names to 3D equivalents (`CharacterBody3D`, `NavigationAgent3D`, `Area3D`, `RayCast3D`).

### 2. SaveManager & Save Correctness
* **Status**: Uses `user://save_game.json` for progress and `MansionMemory` habit weights (`hide_wardrobe`, `hide_curtain`, `door_close_always`, `path_left_bias`, `sprint_frequent`, `light_heavy_user`), plus `user://settings.json` for audio/language settings.
* **Recommendation**: Refactor `SaveManager` to write to `save_game.json.tmp`, flush to disk, and atomically rename to `save_game.json`. Add a default fallback dictionary for newly added MansionMemory keys.

### 3. AI States & Signals
* **Status**: FSM states (Patrol, Investigate, Chase, Alert, Stun) defined across K-01, K-02, K-03 with `StuckDetector`.
* **Recommendation**: Ensure signal connections (`on_player_spotted`, `on_noise_heard`) are registered in GDScript `_ready()` with strong typing and explicit parameters. Adjust Nightmare K-03 speed to $200\text{ px/s}$ or increase sprint stamina allowance.

### 4. Soft-Lock & State Transition Protections
* **Status**: Safeguards documented in `QA_ACCEPTANCE_MATRIX.md` (10s lost key reset, 3 fallback Level 5 barriers when $W < 60$, alternate vent routes).
* **Recommendation**: Add a safety timeout (e.g., 5 seconds) to `VideoStreamPlayer` scene transitions (`level_transition_1_to_2.ogv`) so that if video playback fails or hangs, the game forcibly completes the scene load.

### 5. AudioManager, VoiceManager, & SoundIdentity
* **Status**: Surface footstep noise multipliers (Carpet $0.6\times$, Oak $1.0\times$, Squeaky floor $2.5\times$, Stone $1.3\times$, Water $1.5\times$) and enemy audio warning signatures defined.
* **Recommendation**: Connect `VoiceManager` subtitle duration directly to `AudioStream.get_length()`.

### 6. Residual Risk B-01 (Audio Memory Retention)
* **Status**: Confirmed behavior, unproven root cause.
* **Recommendation**: Test `AudioServer` sample reuse or custom `SoundPool` singleton on physical Android debug builds rather than endless speculative code edits.

### 7. Third-Party Assets & Proof of Licenses
* **Status**: P0 Blocker. Missing legal license documentation for Google Flow videos, 16 pilot voice samples, and audio effects.
* **Recommendation**: Compile a `THIRD_PARTY_LICENSES.md` file listing all asset sources, royalty-free licensing terms, and voice actor release permissions before release.

### 8. Android Export Settings & Build Configuration
* **Status**: Profiles specified (Low 30 FPS / Balanced / High 60 FPS).
* **Recommendation**: Verify `export_presets.cfg` includes ASTC texture compression, target SDK 34+, and appropriate permissions (`VIBRATE`).

### 9. Dependencies & APK Package Integrity
* **Status**: Unnecessary zip files in repository root (`shipthatcode-python-fundamentals-python.zip`).
* **Recommendation**: Remove non-project zip archives from the repository root and add `.gdignore` or configure Godot export filters to exclude `.zip` files.

### 10. Automated Test Coverage
* **Status**: P0 Blocker. 0% automated test coverage.
* **Recommendation**: Integrate GUT (Godot Unit Testing) or GdUnit4 to write automated unit tests for `MansionMemory` decay formulas, `SaveManager` JSON parsing, and AI FSM state switches.

---

## NARRATIVE & SPOILER COMPLIANCE STATEMENT

In strict accordance with release audit guidelines, this report contains **NO plot spoilers, story secrets, or ending descriptions**. All references to game systems remain strictly technical and architectural.
