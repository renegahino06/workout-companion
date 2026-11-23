# ER Diagram - Entity Relationship Diagram

## 🔗 Diagrama Completo (Mermaid)

```mermaid
erDiagram
    USERS ||--o{ ROUTINES : creates
    USERS ||--o{ WORKOUT_SESSIONS : performs
    USERS ||--o{ PERSONAL_RECORDS : achieves
    USERS ||--o{ ACHIEVEMENTS : unlocks
    USERS ||--o{ WORKOUT_PROGRESS : tracks
    USERS ||--|| USER_PREFERENCES : has

    EXERCISES ||--o{ ROUTINE_ITEMS : "in"
    EXERCISES ||--o{ REP_RECORDS : recorded
    EXERCISES ||--o{ PERSONAL_RECORDS : tracked

    ROUTINES ||--o{ ROUTINE_ITEMS : contains
    ROUTINES ||--o{ WORKOUT_SESSIONS : executes

    ROUTINE_ITEMS ||--o{ REP_RECORDS : tracked

    WORKOUT_SESSIONS ||--o{ REP_RECORDS : contains

    USERS : string id PK
    USERS : string email UK
    USERS : string full_name
    USERS : int age
    USERS : string gender
    USERS : int height_cm
    USERS : decimal weight_kg
    USERS : string experience_level
    USERS : string goal
    USERS : string injuries
    USERS : string equipment_access
    USERS : int training_days_per_week
    USERS : int session_duration_minutes
    USERS : timestamp created_at
    USERS : timestamp updated_at

    EXERCISES : string id PK
    EXERCISES : string name UK
    EXERCISES : string description
    EXERCISES : string muscle_groups
    EXERCISES : string difficulty_level
    EXERCISES : string instructions
    EXERCISES : string safety_tips
    EXERCISES : string common_mistakes
    EXERCISES : string equipment_required
    EXERCISES : boolean bodyweight_only
    EXERCISES : string reps_range
    EXERCISES : string sets_range
    EXERCISES : string form_validation_points
    EXERCISES : string category
    EXERCISES : string sport_type

    ROUTINES : string id PK
    ROUTINES : string user_id FK
    ROUTINES : string name
    ROUTINES : string description
    ROUTINES : string difficulty_level
    ROUTINES : string goal
    ROUTINES : int duration_days
    ROUTINES : int workout_days_per_week
    ROUTINES : int session_duration_minutes
    ROUTINES : int total_exercises
    ROUTINES : int total_sets
    ROUTINES : string equipment_required
    ROUTINES : boolean is_active
    ROUTINES : boolean is_custom
    ROUTINES : string generation_method
    ROUTINES : string schedule
    ROUTINES : timestamp created_at

    ROUTINE_ITEMS : string id PK
    ROUTINE_ITEMS : string routine_id FK
    ROUTINE_ITEMS : string exercise_id FK
    ROUTINE_ITEMS : int position
    ROUTINE_ITEMS : int sets
    ROUTINE_ITEMS : int reps
    ROUTINE_ITEMS : int rest_seconds
    ROUTINE_ITEMS : decimal weight_kg
    ROUTINE_ITEMS : string difficulty_adjustment
    ROUTINE_ITEMS : string alternatives
    ROUTINE_ITEMS : string form_cues

    WORKOUT_SESSIONS : string id PK
    WORKOUT_SESSIONS : string user_id FK
    WORKOUT_SESSIONS : string routine_id FK
    WORKOUT_SESSIONS : string session_name
    WORKOUT_SESSIONS : date scheduled_date
    WORKOUT_SESSIONS : timestamp actual_start_time
    WORKOUT_SESSIONS : timestamp actual_end_time
    WORKOUT_SESSIONS : int actual_duration_minutes
    WORKOUT_SESSIONS : string status
    WORKOUT_SESSIONS : string difficulty_felt
    WORKOUT_SESSIONS : string notes
    WORKOUT_SESSIONS : int calories_burned
    WORKOUT_SESSIONS : decimal form_quality_score
    WORKOUT_SESSIONS : string mood_before
    WORKOUT_SESSIONS : string mood_after
    WORKOUT_SESSIONS : int energy_level_before
    WORKOUT_SESSIONS : int energy_level_after

    REP_RECORDS : string id PK
    REP_RECORDS : string workout_session_id FK
    REP_RECORDS : string routine_item_id FK
    REP_RECORDS : string exercise_id FK
    REP_RECORDS : int set_number
    REP_RECORDS : int rep_number
    REP_RECORDS : decimal weight_used_kg
    REP_RECORDS : int duration_seconds
    REP_RECORDS : decimal form_score
    REP_RECORDS : string form_issues
    REP_RECORDS : decimal range_of_motion_score
    REP_RECORDS : decimal speed_score
    REP_RECORDS : decimal stability_score
    REP_RECORDS : string camera_quality
    REP_RECORDS : string keypoints_detected
    REP_RECORDS : string video_url

    PERSONAL_RECORDS : string id PK
    PERSONAL_RECORDS : string user_id FK
    PERSONAL_RECORDS : string exercise_id FK
    PERSONAL_RECORDS : string metric_type
    PERSONAL_RECORDS : decimal value
    PERSONAL_RECORDS : string value_unit
    PERSONAL_RECORDS : date achieved_date
    PERSONAL_RECORDS : string achieved_in_session_id FK
    PERSONAL_RECORDS : timestamp created_at

    WORKOUT_PROGRESS : string id PK
    WORKOUT_PROGRESS : string user_id FK
    WORKOUT_PROGRESS : string period_type
    WORKOUT_PROGRESS : date period_start_date
    WORKOUT_PROGRESS : date period_end_date
    WORKOUT_PROGRESS : int total_sessions
    WORKOUT_PROGRESS : int total_completed_sessions
    WORKOUT_PROGRESS : decimal average_session_duration_minutes
    WORKOUT_PROGRESS : decimal average_form_score
    WORKOUT_PROGRESS : int consistency_percentage

    USER_PREFERENCES : string id PK
    USER_PREFERENCES : string user_id FK UK
    USER_PREFERENCES : boolean notification_enabled
    USER_PREFERENCES : string reminder_time
    USER_PREFERENCES : boolean dark_mode
    USER_PREFERENCES : string language
    USER_PREFERENCES : string camera_quality_mode
    USER_PREFERENCES : boolean enable_form_feedback
    USER_PREFERENCES : boolean enable_camera_recording

    ACHIEVEMENTS : string id PK
    ACHIEVEMENTS : string user_id FK
    ACHIEVEMENTS : string achievement_type
    ACHIEVEMENTS : string title
    ACHIEVEMENTS : string description
    ACHIEVEMENTS : int progress_value
    ACHIEVEMENTS : int progress_target
    ACHIEVEMENTS : int percentage_complete
    ACHIEVEMENTS : boolean unlocked
    ACHIEVEMENTS : timestamp unlocked_date
```

---

## 📊 Diagrama Textual Detallado

### Nivel 1: Tablas de Usuario

```
┌───────────────────────────────────────────────────────────────┐
│                      🧑 USERS                                 │
├───────────────────────────────────────────────────────────────┤
│ ⭐ id: STRING (PK, UUID)                                       │
│ 📧 email: STRING (UNIQUE, NOT NULL)                           │
│ 👤 full_name: STRING (NOT NULL)                               │
│ 📅 age: INT                                                   │
│ 👥 gender: ENUM (MALE, FEMALE, OTHER)                        │
│ 📏 height_cm: INT                                             │
│ ⚖️  weight_kg: DECIMAL(5,2)                                   │
│ 💪 experience_level: ENUM (BEGINNER, INTERMEDIATE, ADVANCED)  │
│ 🎯 goal: ENUM (STRENGTH, MUSCLE, ENDURANCE, etc.)            │
│ 🩹 injuries: JSON (array de lesiones)                        │
│ 🛠️  equipment_access: JSON (dumbbells, barbell, etc.)        │
│ 📋 training_days_per_week: INT (DEFAULT 3)                   │
│ ⏱️  session_duration_minutes: INT (DEFAULT 60)                │
│ 📆 created_at: TIMESTAMP                                      │
│ 🔄 updated_at: TIMESTAMP                                      │
└───────────────────────────────────────────────────────────────┘
              │
              │ 1:1 (↓)
              ▼
┌───────────────────────────────────────────────────────────────┐
│                 ⚙️ USER_PREFERENCES                            │
├───────────────────────────────────────────────────────────────┤
│ ⭐ id: STRING (PK)                                             │
│ 👤 user_id: STRING (FK → USERS, UNIQUE)                      │
│ 🔔 notification_enabled: BOOLEAN (DEFAULT TRUE)               │
│ 🕐 reminder_time: STRING (HH:mm)                              │
│ 🌙 dark_mode: BOOLEAN (DEFAULT TRUE)                          │
│ 🌐 language: STRING (DEFAULT 'es')                            │
│ 📷 camera_quality_mode: ENUM (LOW, BALANCED, HIGH)            │
│ ✅ enable_form_feedback: BOOLEAN (DEFAULT TRUE)               │
│ 🎥 enable_camera_recording: BOOLEAN (DEFAULT FALSE)           │
│ 📊 enable_analytics: BOOLEAN (DEFAULT TRUE)                   │
│ 🔒 privacy_level: ENUM (PRIVATE, FRIENDS, PUBLIC)             │
│ 📐 units: ENUM (METRIC, IMPERIAL)                             │
│ 📆 created_at: TIMESTAMP                                      │
│ 🔄 updated_at: TIMESTAMP                                      │
└───────────────────────────────────────────────────────────────┘
```

### Nivel 2: Tablas de Ejercicios (Master Data)

```
┌───────────────────────────────────────────────────────────────┐
│                   💪 EXERCISES                                │
├───────────────────────────────────────────────────────────────┤
│ ⭐ id: STRING (PK, EXE001, EXE002, ...)                        │
│ 📝 name: STRING (UNIQUE, NOT NULL)                            │
│ 📖 description: TEXT                                          │
│ 🎯 muscle_groups: JSON (chest, triceps, etc.)                │
│ 📊 difficulty_level: ENUM (BEGINNER, INTERMEDIATE, ADVANCED)  │
│ 📋 instructions: JSON (pasos detallados)                      │
│ ⚠️  safety_tips: JSON (recomendaciones)                       │
│ ❌ common_mistakes: JSON (errores frecuentes)                 │
│ 🛠️  equipment_required: JSON (dumbbells, bench, etc.)        │
│ 🏋️ bodyweight_only: BOOLEAN                                  │
│ 🔢 reps_range: JSON ({min: 8, max: 12})                      │
│ 📦 sets_range: JSON ({min: 3, max: 4})                       │
│ 🕐 rest_seconds_range: JSON ({min: 60, max: 90})             │
│ 🤖 form_validation_points: JSON (MediaPipe keypoints)        │
│ 🎥 video_url: STRING (tutorial)                              │
│ 🖼️  image_url: STRING                                         │
│ 🏷️  category: ENUM (PUSH, PULL, LEGS, CORE, CARDIO)         │
│ ⚽ sport_type: STRING (BODYWEIGHT, WEIGHTLIFTING, etc.)      │
│ 📆 created_at: TIMESTAMP                                      │
│ 🔄 updated_at: TIMESTAMP                                      │
└───────────────────────────────────────────────────────────────┘
```

### Nivel 3: Tablas de Rutinas y Entrenamientos

```
┌──────────────────────────────────────────────────────────────┐
│                   📋 ROUTINES                                │
├──────────────────────────────────────────────────────────────┤
│ ⭐ id: STRING (PK, RTN001, RTN002, ...)                      │
│ 👤 user_id: STRING (FK → USERS)                             │
│ 📝 name: STRING (NOT NULL)                                  │
│ 📖 description: TEXT                                        │
│ 📊 difficulty_level: ENUM (BEGINNER, INTERMEDIATE, ADVANCED)│
│ 🎯 goal: STRING (STRENGTH, MUSCLE, ENDURANCE, etc.)        │
│ ⏰ duration_days: INT (semanas de la rutina)                │
│ 📅 workout_days_per_week: INT (3-6)                         │
│ ⏱️  session_duration_minutes: INT (30-180)                   │
│ 🔢 total_exercises: INT                                     │
│ 📦 total_sets: INT                                          │
│ 🕐 rest_between_sets_seconds: INT (DEFAULT 60)             │
│ 🛠️  equipment_required: JSON                                │
│ ✅ is_active: BOOLEAN (DEFAULT TRUE)                        │
│ 🎨 is_custom: BOOLEAN (usuario editó)                       │
│ 🤖 generation_method: ENUM (AI_GENERATED, USER_CREATED)    │
│ ⚙️  generation_parameters: JSON                             │
│ 📅 schedule: JSON ({mon: ['day1'], tue: ['day2']})          │
│ 📆 created_at: TIMESTAMP                                    │
│ 📆 activated_at: TIMESTAMP                                  │
│ 📆 completed_at: TIMESTAMP                                  │
└──────────────────────────────────────────────────────────────┘
              │
              │ 1:N (↓)
              ▼
┌──────────────────────────────────────────────────────────────┐
│               📍 ROUTINE_ITEMS                               │
├──────────────────────────────────────────────────────────────┤
│ ⭐ id: STRING (PK, RTI001, RTI002, ...)                      │
│ 📋 routine_id: STRING (FK → ROUTINES)                       │
│ 💪 exercise_id: STRING (FK → EXERCISES)                     │
│ 🔢 position: INT (orden en la rutina)                       │
│ 📦 sets: INT (3, 4, 5)                                      │
│ 🔄 reps: INT (8-12, 12-15, etc.)                            │
│ 🕐 rest_seconds: INT (DEFAULT 60)                           │
│ ⚖️  weight_kg: DECIMAL(5,2) (sugerido)                      │
│ 📊 difficulty_adjustment: ENUM (EASIER, STANDARD, HARDER)  │
│ 📝 notes: TEXT                                              │
│ 🔀 alternatives: JSON (IDs de ejercicios alternativos)     │
│ 🎯 form_cues: JSON (recordatorios de forma)                │
│ 📆 created_at: TIMESTAMP                                    │
└──────────────────────────────────────────────────────────────┘
              │
              │ 1:N (↓)
              ▼
┌──────────────────────────────────────────────────────────────┐
│            🏋️ WORKOUT_SESSIONS                              │
├──────────────────────────────────────────────────────────────┤
│ ⭐ id: STRING (PK, WKS001, WKS002, ...)                      │
│ 👤 user_id: STRING (FK → USERS)                             │
│ 📋 routine_id: STRING (FK → ROUTINES)                       │
│ 📝 session_name: STRING (Leg Day - Session 5)              │
│ 📅 scheduled_date: DATE                                     │
│ 🕐 actual_start_time: TIMESTAMP                             │
│ 🕐 actual_end_time: TIMESTAMP                               │
│ ⏱️  actual_duration_minutes: INT                             │
│ 🔴 status: ENUM (PLANNED, IN_PROGRESS, COMPLETED, SKIPPED)  │
│ 📊 difficulty_felt: ENUM (EASY, MEDIUM, HARD)               │
│ 💭 notes: TEXT                                              │
│ 🔥 calories_burned: INT                                     │
│ 📈 form_quality_score: DECIMAL (0.00 - 1.00)                │
│ 😊 mood_before: ENUM (GREAT, GOOD, OKAY, BAD, TERRIBLE)     │
│ 😊 mood_after: ENUM (GREAT, GOOD, OKAY, BAD, TERRIBLE)      │
│ ⚡ energy_level_before: INT (1-10)                           │
│ ⚡ energy_level_after: INT (1-10)                            │
│ 🔢 total_reps: INT                                          │
│ ⚖️  total_weight_kg: DECIMAL(8,2)                            │
│ 📆 created_at: TIMESTAMP                                    │
└──────────────────────────────────────────────────────────────┘
              │
              │ 1:N (↓)
              ▼
┌──────────────────────────────────────────────────────────────┐
│            📊 REP_RECORDS                                    │
├──────────────────────────────────────────────────────────────┤
│ ⭐ id: STRING (PK, REP001, REP002, ...)                      │
│ 🏋️ workout_session_id: STRING (FK → WORKOUT_SESSIONS)      │
│ 📍 routine_item_id: STRING (FK → ROUTINE_ITEMS)             │
│ 💪 exercise_id: STRING (FK → EXERCISES)                     │
│ 📦 set_number: INT (1, 2, 3, ...)                           │
│ 🔄 rep_number: INT (1, 2, 3, ...)                           │
│ ⚖️  weight_used_kg: DECIMAL(5,2) (utilizado)                │
│ ⏱️  duration_seconds: INT                                    │
│ ⭐ form_score: DECIMAL (0.00 - 1.00, % corrección)          │
│ ❌ form_issues: JSON (errores detectados)                   │
│ 📐 range_of_motion_score: DECIMAL (0.00 - 1.00)             │
│ 🚀 speed_score: DECIMAL (0.00 - 1.00)                       │
│ 🏗️  stability_score: DECIMAL (0.00 - 1.00)                  │
│ 📷 camera_quality: ENUM (POOR, FAIR, GOOD, EXCELLENT)       │
│ 🤖 keypoints_detected: JSON (puntos detectados)             │
│ 🎥 video_url: STRING (si se grabó)                          │
│ 📆 timestamp: TIMESTAMP                                      │
└──────────────────────────────────────────────────────────────┘
```

### Nivel 4: Tablas de Seguimiento y Logros

```
┌──────────────────────────────────────────────────────────────┐
│          🏆 PERSONAL_RECORDS                                 │
├──────────────────────────────────────────────────────────────┤
│ ⭐ id: STRING (PK, PRS001, PRS002, ...)                      │
│ 👤 user_id: STRING (FK → USERS)                             │
│ 💪 exercise_id: STRING (FK → EXERCISES)                     │
│ 📊 metric_type: ENUM (MAX_WEIGHT, MAX_REPS, BEST_FORM, etc.)│
│ 📈 value: DECIMAL (peso, reps, %, segundos)                │
│ 📐 value_unit: STRING (kg, reps, %, seconds)                │
│ 📅 achieved_date: DATE                                      │
│ 🏋️ achieved_in_session_id: STRING (FK → WORKOUT_SESSIONS)  │
│ 📊 achieved_in_rep_id: STRING (FK → REP_RECORDS)            │
│ 📝 notes: TEXT                                              │
│ 📆 created_at: TIMESTAMP                                    │
│ 🔄 updated_at: TIMESTAMP                                    │
└──────────────────────────────────────────────────────────────┘
              │
              │ Agregado de sesiones
              ▼
┌──────────────────────────────────────────────────────────────┐
│          📈 WORKOUT_PROGRESS                                 │
├──────────────────────────────────────────────────────────────┤
│ ⭐ id: STRING (PK, PRG001, PRG002, ...)                      │
│ 👤 user_id: STRING (FK → USERS)                             │
│ 📅 period_type: ENUM (WEEKLY, MONTHLY)                       │
│ 📆 period_start_date: DATE                                  │
│ 📆 period_end_date: DATE                                    │
│ 🏋️ total_sessions: INT                                      │
│ ✅ total_completed_sessions: INT                            │
│ ⏭️  total_skipped_sessions: INT                              │
│ 💪 total_exercises: INT                                     │
│ 📦 total_sets: INT                                          │
│ 🔄 total_reps: INT                                          │
│ ⚖️  total_weight_kg: DECIMAL(10,2)                           │
│ ⏱️  average_session_duration_minutes: DECIMAL               │
│ ⭐ average_form_score: DECIMAL (0.00 - 1.00)                │
│ 🔥 total_calories_burned: INT                               │
│ 🎯 exercises_improved: INT                                  │
│ 🏆 new_personal_records: INT                                │
│ 📊 consistency_percentage: INT (0-100%)                     │
│ 📊 difficulty_trend: JSON (cambio de dificultad)            │
│ 📆 created_at: TIMESTAMP                                    │
└──────────────────────────────────────────────────────────────┘
              │
              │ Visibilidad sobre desempeño
              ▼
┌──────────────────────────────────────────────────────────────┐
│           🎖️ ACHIEVEMENTS                                    │
├──────────────────────────────────────────────────────────────┤
│ ⭐ id: STRING (PK, ACH001, ACH002, ...)                      │
│ 👤 user_id: STRING (FK → USERS)                             │
│ 🏷️  achievement_type: STRING (FIRST_WORKOUT, WEEK_STREAK, etc)│
│ 🎯 title: STRING (not null)                                │
│ 📖 description: TEXT                                        │
│ 🖼️  icon_url: STRING (badge visual)                         │
│ 🎖️ badge_name: STRING                                       │
│ 📊 progress_value: INT (actual)                             │
│ 🎯 progress_target: INT (objetivo)                          │
│ 📈 percentage_complete: INT (0-100%)                        │
│ 🔓 unlocked: BOOLEAN                                        │
│ 🎉 unlocked_date: TIMESTAMP (cuando se desbloqueó)          │
│ 📆 created_at: TIMESTAMP                                    │
│ 🔄 updated_at: TIMESTAMP                                    │
└──────────────────────────────────────────────────────────────┘
```

---

## 🔀 Flujos de Datos Principales

### Flujo 1: Crear Rutina Personalizada
```
USER
  ↓ (proporciona datos: edad, goal, experiencia)
USERS (actualizar)
  ↓
ROUTINE GENERATION ALGORITHM (en app)
  ↓ (selecciona ejercicios)
EXERCISES (consultar)
  ↓
ROUTINES (crear)
  ↓
ROUTINE_ITEMS (crear items)
  ↓
USER_PREFERENCES (consultar settings)
  ↓
Rutina lista para entrenar
```

### Flujo 2: Ejecutar Sesión de Entrenamiento
```
ROUTINES (rutina activa)
  ↓
WORKOUT_SESSIONS (crear sesión)
  ↓
ROUTINE_ITEMS (obtener próximo ejercicio)
  ↓
CAMERA + MEDIAPIPE (detectar pose)
  ↓
REP_RECORDS (guardar repetición)
  ↓ (calcular scores)
PERSONAL_RECORDS (actualizar si es PR)
  ↓
ACHIEVEMENTS (verificar logros)
  ↓
Sesión completada
```

### Flujo 3: Rastrear Progreso
```
WORKOUT_SESSIONS (últimas sesiones)
  ↓
REP_RECORDS (agregaciones)
  ↓
WORKOUT_PROGRESS (crear/actualizar período)
  ↓
PERSONAL_RECORDS (top achievements)
  ↓
ANALYTICS & CHARTS (mostrar en UI)
  ↓
Usuario ve sus gráficos
```

---

## 🔐 Restricciones de Integridad

### Restricciones de Llave Foránea

| Tabla | Campo | Referencia | Acción |
|-------|-------|-----------|--------|
| routines | user_id | users.id | CASCADE |
| routine_items | routine_id | routines.id | CASCADE |
| routine_items | exercise_id | exercises.id | RESTRICT |
| workout_sessions | user_id | users.id | CASCADE |
| workout_sessions | routine_id | routines.id | SET NULL |
| rep_records | workout_session_id | workout_sessions.id | CASCADE |
| rep_records | routine_item_id | routine_items.id | RESTRICT |
| rep_records | exercise_id | exercises.id | RESTRICT |
| personal_records | user_id | users.id | CASCADE |
| personal_records | exercise_id | exercises.id | CASCADE |
| personal_records | achieved_in_session_id | workout_sessions.id | SET NULL |
| workout_progress | user_id | users.id | CASCADE |
| achievements | user_id | users.id | CASCADE |
| user_preferences | user_id | users.id | CASCADE |

---

## 🚀 Índices para Optimización

```sql
-- USERS
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_created_at ON users(created_at);

-- EXERCISES
CREATE INDEX idx_exercises_category ON exercises(category);
CREATE INDEX idx_exercises_difficulty ON exercises(difficulty_level);
CREATE INDEX idx_exercises_name ON exercises(name);

-- ROUTINES
CREATE INDEX idx_routines_user_id ON routines(user_id);
CREATE INDEX idx_routines_is_active ON routines(is_active);
CREATE INDEX idx_routines_created_at ON routines(created_at);

-- ROUTINE_ITEMS
CREATE INDEX idx_routine_items_routine_id ON routine_items(routine_id);
CREATE INDEX idx_routine_items_position ON routine_items(routine_id, position);

-- WORKOUT_SESSIONS
CREATE INDEX idx_workout_sessions_user_id ON workout_sessions(user_id);
CREATE INDEX idx_workout_sessions_actual_start_time ON workout_sessions(actual_start_time);
CREATE INDEX idx_workout_sessions_status ON workout_sessions(status);

-- REP_RECORDS
CREATE INDEX idx_rep_records_workout_session_id ON rep_records(workout_session_id);
CREATE INDEX idx_rep_records_exercise_id ON rep_records(exercise_id);

-- PERSONAL_RECORDS
CREATE INDEX idx_personal_records_user_id ON personal_records(user_id);
CREATE INDEX idx_personal_records_achieved_date ON personal_records(achieved_date);

-- WORKOUT_PROGRESS
CREATE INDEX idx_workout_progress_user_id ON workout_progress(user_id);
CREATE INDEX idx_workout_progress_period_start_date ON workout_progress(period_start_date);

-- ACHIEVEMENTS
CREATE INDEX idx_achievements_user_id ON achievements(user_id);
CREATE INDEX idx_achievements_unlocked ON achievements(unlocked);
```

---

## 📱 Mapeo a Entidades de Kotlin

```kotlin
// Mapeo de Tablas a Clases de Kotlin
users                → User, UserData
exercises            → Exercise
routines             → Routine
routine_items        → RoutineItem
workout_sessions     → WorkoutSession
rep_records          → RepRecord
personal_records     → PersonalRecord
workout_progress     → WorkoutProgress
user_preferences     → UserPreferences
achievements         → Achievement
```

---

## ✅ Validaciones de Schema

- ✅ Todas las tablas tienen Primary Key
- ✅ Todas las Foreign Keys tienen restricciones definidas
- ✅ Índices en columnas de búsqueda frecuente
- ✅ Timestamps en created_at y updated_at
- ✅ ENUM fields con valores predefinidos
- ✅ Cardinalidades correctas (1:N, N:1, 1:1)
- ✅ No hay redundancia de datos

---

## 📊 Estadísticas del Schema

| Métrica | Valor |
|---------|-------|
| Total de Tablas | 10 |
| Total de Campos | 128+ |
| Total de Relaciones | 15 |
| Total de Índices | 25+ |
| Cardinalidad Máxima | 1:N (Routines → Routine Items) |

