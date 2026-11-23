# Data Model & Database Schema

## 📊 Overview

La base de datos está diseñada con Clean Architecture en mente:
- **Room Database** (SQLite) para persistencia local
- **Relaciones normalizadas** para evitar redundancia
- **Índices** para optimizar queries frecuentes
- **Timestamps** para auditoría y seguimiento

---

## 🗄️ Tablas Principales

### 1. **users**
Usuario de la aplicación con sus preferencias.

```sql
CREATE TABLE users (
  id TEXT PRIMARY KEY,
  email TEXT UNIQUE NOT NULL,
  full_name TEXT NOT NULL,
  age INT,
  gender TEXT, -- 'MALE', 'FEMALE', 'OTHER'
  height_cm INT,
  weight_kg DECIMAL(5,2),
  experience_level TEXT NOT NULL, -- 'BEGINNER', 'INTERMEDIATE', 'ADVANCED'
  goal TEXT NOT NULL, -- 'STRENGTH', 'MUSCLE', 'ENDURANCE', 'FLEXIBILITY', 'WEIGHT_LOSS'
  injuries TEXT, -- JSON array de lesiones
  equipment_access TEXT, -- JSON array: 'DUMBBELLS', 'BARBELL', 'CABLE', 'BODYWEIGHT', etc.
  training_days_per_week INT DEFAULT 3,
  session_duration_minutes INT DEFAULT 60,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  last_login TIMESTAMP
);

CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_created_at ON users(created_at);
```

**Relaciones:**
- 1 User → ∞ Routines
- 1 User → ∞ Workout Sessions
- 1 User → ∞ Preferences

---

### 2. **exercises**
Catálogo de ejercicios (data master).

```sql
CREATE TABLE exercises (
  id TEXT PRIMARY KEY, -- e.g., 'EXE001'
  name TEXT NOT NULL UNIQUE, -- 'Push-up', 'Bicep Curl', etc.
  description TEXT,
  muscle_groups TEXT NOT NULL, -- JSON: ['chest', 'triceps']
  difficulty_level TEXT NOT NULL, -- 'BEGINNER', 'INTERMEDIATE', 'ADVANCED'
  instructions TEXT, -- JSON con pasos
  safety_tips TEXT, -- JSON con recomendaciones
  common_mistakes TEXT, -- JSON con errores comunes
  equipment_required TEXT, -- JSON: ['dumbbells', 'bench']
  bodyweight_only BOOLEAN DEFAULT FALSE,
  reps_range TEXT, -- JSON: {min: 8, max: 12}
  sets_range TEXT, -- JSON: {min: 3, max: 4}
  rest_seconds_range TEXT, -- JSON: {min: 60, max: 90}
  form_validation_points TEXT, -- JSON: keypoints para MediaPipe
  video_url TEXT,
  image_url TEXT,
  category TEXT, -- 'PUSH', 'PULL', 'LEGS', 'CORE', 'CARDIO'
  sport_type TEXT NOT NULL, -- 'BODYWEIGHT', 'WEIGHTLIFTING', 'CALISTHENICS'
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_exercises_category ON exercises(category);
CREATE INDEX idx_exercises_difficulty ON exercises(difficulty_level);
CREATE INDEX idx_exercises_name ON exercises(name);
```

**Relaciones:**
- 1 Exercise → ∞ Routine Items
- 1 Exercise → ∞ Rep Records

---

### 3. **routines**
Rutina de entrenamiento personalizada para un usuario.

```sql
CREATE TABLE routines (
  id TEXT PRIMARY KEY, -- e.g., 'RTN001'
  user_id TEXT NOT NULL,
  name TEXT NOT NULL,
  description TEXT,
  difficulty_level TEXT NOT NULL, -- 'BEGINNER', 'INTERMEDIATE', 'ADVANCED'
  goal TEXT NOT NULL, -- Heredado de user pero específico para esta rutina
  duration_days INT, -- Duración de la rutina (ej: 12 semanas)
  workout_days_per_week INT NOT NULL,
  session_duration_minutes INT NOT NULL,
  total_exercises INT NOT NULL,
  total_sets INT NOT NULL,
  rest_between_sets_seconds INT DEFAULT 60,
  equipment_required TEXT, -- JSON
  is_active BOOLEAN DEFAULT TRUE,
  is_custom BOOLEAN DEFAULT FALSE, -- true si fue editada por usuario
  generation_method TEXT, -- 'AI_GENERATED', 'USER_CREATED', 'TEMPLATE'
  generation_parameters TEXT, -- JSON con parámetros usados
  schedule TEXT NOT NULL, -- JSON: {mon: ['day1'], tue: ['day2'], ...}
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  activated_at TIMESTAMP,
  completed_at TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

CREATE INDEX idx_routines_user_id ON routines(user_id);
CREATE INDEX idx_routines_is_active ON routines(is_active);
CREATE INDEX idx_routines_created_at ON routines(created_at);
```

**Relaciones:**
- N Routines → 1 User
- 1 Routine → ∞ Routine Items
- 1 Routine → ∞ Workout Sessions

---

### 4. **routine_items**
Items de ejercicio dentro de una rutina (posición específica).

```sql
CREATE TABLE routine_items (
  id TEXT PRIMARY KEY, -- e.g., 'RTI001'
  routine_id TEXT NOT NULL,
  exercise_id TEXT NOT NULL,
  position INT NOT NULL, -- Orden en la rutina
  sets INT NOT NULL,
  reps INT NOT NULL,
  rest_seconds INT DEFAULT 60,
  weight_kg DECIMAL(5,2), -- Peso sugerido
  difficulty_adjustment TEXT, -- 'EASIER', 'STANDARD', 'HARDER'
  notes TEXT,
  alternatives TEXT, -- JSON: IDs de ejercicios alternativos
  form_cues TEXT, -- JSON: Recordatorios de forma
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (routine_id) REFERENCES routines(id) ON DELETE CASCADE,
  FOREIGN KEY (exercise_id) REFERENCES exercises(id) ON DELETE RESTRICT
);

CREATE INDEX idx_routine_items_routine_id ON routine_items(routine_id);
CREATE INDEX idx_routine_items_exercise_id ON routine_items(exercise_id);
CREATE INDEX idx_routine_items_position ON routine_items(routine_id, position);
```

**Relaciones:**
- N Items → 1 Routine
- N Items → 1 Exercise
- 1 Item → ∞ Rep Records

---

### 5. **workout_sessions**
Sesión de entrenamiento ejecutada (histórico).

```sql
CREATE TABLE workout_sessions (
  id TEXT PRIMARY KEY, -- e.g., 'WKS001'
  user_id TEXT NOT NULL,
  routine_id TEXT NOT NULL,
  session_name TEXT, -- e.g., 'Leg Day - Session 5'
  scheduled_date DATE,
  actual_start_time TIMESTAMP,
  actual_end_time TIMESTAMP,
  actual_duration_minutes INT,
  status TEXT NOT NULL, -- 'PLANNED', 'IN_PROGRESS', 'COMPLETED', 'SKIPPED', 'CANCELLED'
  difficulty_felt TEXT, -- 'EASY', 'MEDIUM', 'HARD'
  notes TEXT,
  calories_burned INT,
  form_quality_score DECIMAL(3,2), -- 0.00 - 1.00 (promedio)
  mood_before TEXT, -- 'GREAT', 'GOOD', 'OKAY', 'BAD', 'TERRIBLE'
  mood_after TEXT,
  energy_level_before INT, -- 1-10
  energy_level_after INT,
  total_reps INT,
  total_weight_kg DECIMAL(8,2),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
  FOREIGN KEY (routine_id) REFERENCES routines(id) ON DELETE SET NULL
);

CREATE INDEX idx_workout_sessions_user_id ON workout_sessions(user_id);
CREATE INDEX idx_workout_sessions_routine_id ON workout_sessions(routine_id);
CREATE INDEX idx_workout_sessions_actual_start_time ON workout_sessions(actual_start_time);
CREATE INDEX idx_workout_sessions_status ON workout_sessions(status);
```

**Relaciones:**
- N Sessions → 1 User
- N Sessions → 1 Routine
- 1 Session → ∞ Rep Records

---

### 6. **rep_records**
Registro de cada repetición ejecutada en una sesión.

```sql
CREATE TABLE rep_records (
  id TEXT PRIMARY KEY, -- e.g., 'REP001'
  workout_session_id TEXT NOT NULL,
  routine_item_id TEXT NOT NULL,
  exercise_id TEXT NOT NULL,
  set_number INT NOT NULL,
  rep_number INT NOT NULL,
  weight_used_kg DECIMAL(5,2),
  duration_seconds INT,
  form_score DECIMAL(3,2), -- 0.00 - 1.00 (% de forma correcta)
  form_issues TEXT, -- JSON: errores detectados por MediaPipe
  range_of_motion_score DECIMAL(3,2), -- 0.00 - 1.00
  speed_score DECIMAL(3,2), -- 0.00 - 1.00 (controlado vs explosivo)
  stability_score DECIMAL(3,2), -- 0.00 - 1.00
  camera_quality TEXT, -- 'POOR', 'FAIR', 'GOOD', 'EXCELLENT'
  keypoints_detected TEXT, -- JSON: keypoints detectados
  video_url TEXT, -- URL a video guardado
  timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (workout_session_id) REFERENCES workout_sessions(id) ON DELETE CASCADE,
  FOREIGN KEY (routine_item_id) REFERENCES routine_items(id) ON DELETE RESTRICT,
  FOREIGN KEY (exercise_id) REFERENCES exercises(id) ON DELETE RESTRICT
);

CREATE INDEX idx_rep_records_workout_session_id ON rep_records(workout_session_id);
CREATE INDEX idx_rep_records_exercise_id ON rep_records(exercise_id);
CREATE INDEX idx_rep_records_set_number ON rep_records(workout_session_id, set_number);
```

**Relaciones:**
- N Records → 1 Workout Session
- N Records → 1 Routine Item
- N Records → 1 Exercise

---

### 7. **personal_records**
Registros personales por ejercicio (PRs).

```sql
CREATE TABLE personal_records (
  id TEXT PRIMARY KEY, -- e.g., 'PRS001'
  user_id TEXT NOT NULL,
  exercise_id TEXT NOT NULL,
  metric_type TEXT NOT NULL, -- 'MAX_WEIGHT', 'MAX_REPS', 'BEST_FORM', 'FASTEST_TIME'
  value DECIMAL(8,2) NOT NULL,
  value_unit TEXT NOT NULL, -- 'kg', 'reps', '%', 'seconds'
  achieved_date DATE,
  achieved_in_session_id TEXT,
  achieved_in_rep_id TEXT,
  notes TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
  FOREIGN KEY (exercise_id) REFERENCES exercises(id) ON DELETE CASCADE,
  FOREIGN KEY (achieved_in_session_id) REFERENCES workout_sessions(id) ON DELETE SET NULL,
  FOREIGN KEY (achieved_in_rep_id) REFERENCES rep_records(id) ON DELETE SET NULL
);

CREATE INDEX idx_personal_records_user_id ON personal_records(user_id);
CREATE INDEX idx_personal_records_exercise_id ON personal_records(exercise_id);
CREATE INDEX idx_personal_records_achieved_date ON personal_records(achieved_date);
```

**Relaciones:**
- N Records → 1 User
- N Records → 1 Exercise
- N Records → 1 Workout Session (opcional)
- N Records → 1 Rep Record (opcional)

---

### 8. **workout_progress**
Progreso agregado por período (semanal/mensual).

```sql
CREATE TABLE workout_progress (
  id TEXT PRIMARY KEY, -- e.g., 'PRG001'
  user_id TEXT NOT NULL,
  period_type TEXT NOT NULL, -- 'WEEKLY', 'MONTHLY'
  period_start_date DATE NOT NULL,
  period_end_date DATE NOT NULL,
  total_sessions INT,
  total_completed_sessions INT,
  total_skipped_sessions INT,
  total_exercises INT,
  total_sets INT,
  total_reps INT,
  total_weight_kg DECIMAL(10,2),
  average_session_duration_minutes DECIMAL(5,2),
  average_form_score DECIMAL(3,2),
  total_calories_burned INT,
  exercises_improved INT, -- Ejercicios con PR
  new_personal_records INT,
  consistency_percentage INT, -- 0-100 (% de sesiones completadas)
  difficulty_trend TEXT, -- JSON: cambio en dificultad
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

CREATE INDEX idx_workout_progress_user_id ON workout_progress(user_id);
CREATE INDEX idx_workout_progress_period_start_date ON workout_progress(period_start_date);
```

---

### 9. **user_preferences**
Preferencias y configuración del usuario.

```sql
CREATE TABLE user_preferences (
  id TEXT PRIMARY KEY, -- e.g., 'PRF001'
  user_id TEXT NOT NULL UNIQUE,
  notification_enabled BOOLEAN DEFAULT TRUE,
  reminder_time TEXT, -- HH:mm
  dark_mode BOOLEAN DEFAULT TRUE,
  language TEXT DEFAULT 'es',
  camera_quality_mode TEXT DEFAULT 'BALANCED', -- 'LOW', 'BALANCED', 'HIGH'
  enable_form_feedback BOOLEAN DEFAULT TRUE,
  enable_camera_recording BOOLEAN DEFAULT FALSE,
  enable_analytics BOOLEAN DEFAULT TRUE,
  privacy_level TEXT DEFAULT 'PRIVATE', -- 'PRIVATE', 'FRIENDS', 'PUBLIC'
  units TEXT DEFAULT 'METRIC', -- 'METRIC', 'IMPERIAL'
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

CREATE INDEX idx_user_preferences_user_id ON user_preferences(user_id);
```

---

### 10. **achievements**
Logros desbloqueados por el usuario.

```sql
CREATE TABLE achievements (
  id TEXT PRIMARY KEY, -- e.g., 'ACH001'
  user_id TEXT NOT NULL,
  achievement_type TEXT NOT NULL, -- 'FIRST_WORKOUT', 'WEEK_STREAK_7', 'PR_BROKEN', etc.
  title TEXT NOT NULL,
  description TEXT,
  icon_url TEXT,
  badge_name TEXT,
  progress_value INT, -- Valor actual de progreso
  progress_target INT, -- Valor objetivo
  percentage_complete INT,
  unlocked BOOLEAN DEFAULT FALSE,
  unlocked_date TIMESTAMP,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

CREATE INDEX idx_achievements_user_id ON achievements(user_id);
CREATE INDEX idx_achievements_unlocked ON achievements(unlocked);
```

---

## 🔗 Diagrama Entidad-Relación (ER)

```
┌─────────────────────────────────────────────────────────────────────────┐
│                           USERS (1)                                     │
│  ┌────────────────────────────────────────────────────────┐             │
│  │ id (PK) | email | full_name | age | gender | height   │             │
│  │ weight | experience_level | goal | injuries | ...     │             │
│  └────────────────────────────────────────────────────────┘             │
│    ↓                                 ↓                                   │
│    │ 1:N                             │ 1:N                              │
│    ├─────────────────────────────────┼─────────────────────────────────┤
│    ↓                                 ↓                                   │
│  ┌──────────────┐          ┌────────────────────┐                       │
│  │ ROUTINES (N) │          │ PREFERENCES (1)    │                       │
│  │ 1:N ↓        │          │ ↑ 1:1              │                       │
│  └──────────────┘          └────────────────────┘                       │
│    ↓                                                                     │
│    │ 1:N                                                                │
│    ↓                                                                     │
│  ┌─────────────────────────┐                                            │
│  │ ROUTINE_ITEMS (N)       │                                            │
│  │ 1:N ↓                   │                                            │
│  └─────────────────────────┘                                            │
│           ↓                                                             │
│           │ N:1 (exercise_id)                                          │
│           ↓                                                             │
│           └────────────────────────────┐                               │
│                                        │                               │
└────────────────────────────────────────┼───────────────────────────────┘
                                         │
                    ┌────────────────────┼─────────────────────────┐
                    ↓                    ↓                         ↓
         ┌──────────────────┐  ┌──────────────────┐   ┌────────────────────┐
         │ EXERCISES (1)    │  │ WORKOUT_SESSIONS │   │ PERSONAL_RECORDS   │
         │ N:1 ↓            │  │ 1:N ↓            │   │ 1:N ↓              │
         └──────────────────┘  └──────────────────┘   └────────────────────┘
                 ↑                       ↓
                 │                       │ 1:N
                 │                       ↓
                 │            ┌──────────────────────┐
                 │            │ REP_RECORDS (N)      │
                 └────────────│ 1:N ↓                │
                              └──────────────────────┘
                                 ↓
                                 │ N:1 (exercise_id)
                                 └──────────────────→ [back to EXERCISES]

ADDITIONAL TABLES:
├─ WORKOUT_PROGRESS (1:N to USERS) - Agregados por período
└─ ACHIEVEMENTS (1:N to USERS) - Logros desbloqueados
```

---

## 📋 Matriz de Relaciones

| Tabla A | Tabla B | Relación | Cardinalidad | Restricción |
|---------|---------|----------|--------------|------------|
| users | routines | user_id | 1:N | CASCADE |
| users | workout_sessions | user_id | 1:N | CASCADE |
| users | user_preferences | user_id | 1:1 | CASCADE |
| users | personal_records | user_id | 1:N | CASCADE |
| users | achievements | user_id | 1:N | CASCADE |
| users | workout_progress | user_id | 1:N | CASCADE |
| routines | routine_items | routine_id | 1:N | CASCADE |
| routines | workout_sessions | routine_id | 1:N | SET NULL |
| routine_items | exercises | exercise_id | N:1 | RESTRICT |
| routine_items | rep_records | routine_item_id | 1:N | RESTRICT |
| exercises | rep_records | exercise_id | 1:N | RESTRICT |
| exercises | personal_records | exercise_id | 1:N | CASCADE |
| workout_sessions | rep_records | workout_session_id | 1:N | CASCADE |
| rep_records | personal_records | rep_id | 1:N | SET NULL |

---

## 🔑 Convenciones de IDs

| Tabla | Prefijo | Formato | Ejemplo |
|-------|---------|---------|---------|
| users | USR | UUID v4 | 550e8400-e29b-41d4-a716-446655440000 |
| exercises | EXE | EXE + 3 dígitos | EXE001 |
| routines | RTN | RTN + 3 dígitos | RTN001 |
| routine_items | RTI | RTI + 3 dígitos | RTI001 |
| workout_sessions | WKS | WKS + 3 dígitos | WKS001 |
| rep_records | REP | REP + 3 dígitos | REP001 |
| personal_records | PRS | PRS + 3 dígitos | PRS001 |
| workout_progress | PRG | PRG + 3 dígitos | PRG001 |
| user_preferences | PRF | PRF + 3 dígitos | PRF001 |
| achievements | ACH | ACH + 3 dígitos | ACH001 |

---

## 🎯 Consultas Importantes

### Rutina activa de usuario
```sql
SELECT r.* FROM routines r
WHERE r.user_id = ? 
  AND r.is_active = TRUE
ORDER BY r.activated_at DESC
LIMIT 1;
```

### Próximo ejercicio en sesión
```sql
SELECT ri.* FROM routine_items ri
WHERE ri.routine_id = ?
ORDER BY ri.position
LIMIT 1 OFFSET ?;
```

### Personal records de usuario
```sql
SELECT pr.*, e.name, pr.value, pr.value_unit
FROM personal_records pr
JOIN exercises e ON pr.exercise_id = e.id
WHERE pr.user_id = ?
ORDER BY pr.achieved_date DESC;
```

### Progreso semanal
```sql
SELECT wp.* FROM workout_progress wp
WHERE wp.user_id = ?
  AND wp.period_type = 'WEEKLY'
ORDER BY wp.period_start_date DESC
LIMIT 4; -- Últimas 4 semanas
```

### Calidad de forma en última sesión
```sql
SELECT AVG(r.form_score) as avg_form_score
FROM rep_records r
WHERE r.workout_session_id = ?;
```

---

## 📱 Mapeo a Clean Architecture

### Domain Layer (Entities)
```
User, Exercise, Routine, RoutineItem, 
WorkoutSession, RepRecord, PersonalRecord, 
WorkoutProgress, UserPreferences, Achievement
```

### Data Layer (DAO - Data Access Objects)
```
UserDao, ExerciseDao, RoutineDao, RoutineItemDao,
WorkoutSessionDao, RepRecordDao, PersonalRecordDao,
WorkoutProgressDao, UserPreferencesDao, AchievementDao
```

### Repository Layer
```
UserRepository, ExerciseRepository, RoutineRepository,
WorkoutRepository, ProgressRepository, PreferencesRepository,
AchievementRepository
```

---

## ✅ Validaciones y Restricciones

### Nivel de Base de Datos
- `NOT NULL` en campos requeridos
- `UNIQUE` en email de usuarios
- `FOREIGN KEY` con restricciones (CASCADE, RESTRICT, SET NULL)
- `CHECK` constraints en valores enumerados (si es necesario)
- Índices en columnas frecuentemente consultadas

### Nivel de Aplicación
- Validar edad (13-120)
- Validar peso y altura (válidos)
- Validar días de entrenamiento (1-7)
- Validar duraciones (30-180 minutos)
- Validar scores de forma (0-100%)
- Validar timestamps (no futuros)

---

## 📈 Estrategia de Escalamiento

**Fase 1 (MVP):** SQLite + Room
**Fase 2 (Growth):** Sincronización con backend (Firebase o GraphQL)
**Fase 3 (Scale):** PostgreSQL backend + Offline-First

---

## 🔄 Versionado de Schema

| Versión | Cambios | Fecha |
|---------|---------|-------|
| 1.0 | Schema inicial (10 tablas) | 2024-01-15 |
| 1.1 | Agregar `camera_quality` a rep_records | Próximo |
| 1.2 | Agregar social features (friends, sharing) | Próximo |

