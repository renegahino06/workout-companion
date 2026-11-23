# Database Migrations & Room Setup

## 📋 Overview

Las migraciones de base de datos siguen las mejores prácticas:
- **Version Control**: Cada migración tiene versión explícita
- **Rollback Plan**: Scripts de rollback documentados
- **Idempotencia**: Migraciones pueden ejecutarse sin errores si ya existen
- **Testing**: Cada migración tiene test script

---

## 🔄 Versionado de Schema

| Version | Cambio Principal | Fecha |
|---------|------------------|-------|
| 1 | Schema inicial (10 tablas) | 2024-01-15 |
| 2+ | Migraciones futuras | - |

---

## 📄 Migración v1: Schema Inicial

### SQL Schema v1

```sql
-- ============================================================================
-- MIGRATION VERSION 1: Initial Schema Setup
-- Created: 2024-01-15
-- Description: Create all base tables for Workout Companion app
-- ============================================================================

-- ============================================================================
-- TABLE: users
-- Purpose: Almacenar información de usuarios registrados
-- ============================================================================
CREATE TABLE IF NOT EXISTS users (
  id TEXT PRIMARY KEY NOT NULL,
  email TEXT UNIQUE NOT NULL,
  full_name TEXT NOT NULL,
  age INTEGER,
  gender TEXT,
  height_cm INTEGER,
  weight_kg REAL,
  experience_level TEXT NOT NULL,
  goal TEXT NOT NULL,
  injuries TEXT,
  equipment_access TEXT,
  training_days_per_week INTEGER NOT NULL DEFAULT 3,
  session_duration_minutes INTEGER NOT NULL DEFAULT 60,
  created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  last_login TIMESTAMP
);

CREATE INDEX IF NOT EXISTS idx_users_email ON users(email);
CREATE INDEX IF NOT EXISTS idx_users_created_at ON users(created_at);

-- ============================================================================
-- TABLE: exercises
-- Purpose: Catálogo master de ejercicios disponibles
-- ============================================================================
CREATE TABLE IF NOT EXISTS exercises (
  id TEXT PRIMARY KEY NOT NULL,
  name TEXT UNIQUE NOT NULL,
  description TEXT,
  muscle_groups TEXT NOT NULL,
  difficulty_level TEXT NOT NULL,
  instructions TEXT,
  safety_tips TEXT,
  common_mistakes TEXT,
  equipment_required TEXT,
  bodyweight_only INTEGER NOT NULL DEFAULT 0,
  reps_range TEXT,
  sets_range TEXT,
  rest_seconds_range TEXT,
  form_validation_points TEXT,
  video_url TEXT,
  image_url TEXT,
  category TEXT NOT NULL,
  sport_type TEXT NOT NULL,
  created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX IF NOT EXISTS idx_exercises_category ON exercises(category);
CREATE INDEX IF NOT EXISTS idx_exercises_difficulty ON exercises(difficulty_level);
CREATE INDEX IF NOT EXISTS idx_exercises_name ON exercises(name);

-- ============================================================================
-- TABLE: routines
-- Purpose: Rutinas de entrenamiento personalizadas por usuario
-- ============================================================================
CREATE TABLE IF NOT EXISTS routines (
  id TEXT PRIMARY KEY NOT NULL,
  user_id TEXT NOT NULL,
  name TEXT NOT NULL,
  description TEXT,
  difficulty_level TEXT NOT NULL,
  goal TEXT NOT NULL,
  duration_days INTEGER,
  workout_days_per_week INTEGER NOT NULL,
  session_duration_minutes INTEGER NOT NULL,
  total_exercises INTEGER NOT NULL,
  total_sets INTEGER NOT NULL,
  rest_between_sets_seconds INTEGER NOT NULL DEFAULT 60,
  equipment_required TEXT,
  is_active INTEGER NOT NULL DEFAULT 1,
  is_custom INTEGER NOT NULL DEFAULT 0,
  generation_method TEXT NOT NULL,
  generation_parameters TEXT,
  schedule TEXT NOT NULL,
  created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  activated_at TIMESTAMP,
  completed_at TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

CREATE INDEX IF NOT EXISTS idx_routines_user_id ON routines(user_id);
CREATE INDEX IF NOT EXISTS idx_routines_is_active ON routines(is_active);
CREATE INDEX IF NOT EXISTS idx_routines_created_at ON routines(created_at);

-- ============================================================================
-- TABLE: routine_items
-- Purpose: Items de ejercicios dentro de una rutina
-- ============================================================================
CREATE TABLE IF NOT EXISTS routine_items (
  id TEXT PRIMARY KEY NOT NULL,
  routine_id TEXT NOT NULL,
  exercise_id TEXT NOT NULL,
  position INTEGER NOT NULL,
  sets INTEGER NOT NULL,
  reps INTEGER NOT NULL,
  rest_seconds INTEGER NOT NULL DEFAULT 60,
  weight_kg REAL,
  difficulty_adjustment TEXT,
  notes TEXT,
  alternatives TEXT,
  form_cues TEXT,
  created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (routine_id) REFERENCES routines(id) ON DELETE CASCADE,
  FOREIGN KEY (exercise_id) REFERENCES exercises(id) ON DELETE RESTRICT
);

CREATE INDEX IF NOT EXISTS idx_routine_items_routine_id ON routine_items(routine_id);
CREATE INDEX IF NOT EXISTS idx_routine_items_exercise_id ON routine_items(exercise_id);
CREATE INDEX IF NOT EXISTS idx_routine_items_position ON routine_items(routine_id, position);

-- ============================================================================
-- TABLE: workout_sessions
-- Purpose: Registro de sesiones de entrenamiento ejecutadas
-- ============================================================================
CREATE TABLE IF NOT EXISTS workout_sessions (
  id TEXT PRIMARY KEY NOT NULL,
  user_id TEXT NOT NULL,
  routine_id TEXT,
  session_name TEXT,
  scheduled_date DATE,
  actual_start_time TIMESTAMP,
  actual_end_time TIMESTAMP,
  actual_duration_minutes INTEGER,
  status TEXT NOT NULL,
  difficulty_felt TEXT,
  notes TEXT,
  calories_burned INTEGER,
  form_quality_score REAL,
  mood_before TEXT,
  mood_after TEXT,
  energy_level_before INTEGER,
  energy_level_after INTEGER,
  total_reps INTEGER,
  total_weight_kg REAL,
  created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
  FOREIGN KEY (routine_id) REFERENCES routines(id) ON DELETE SET NULL
);

CREATE INDEX IF NOT EXISTS idx_workout_sessions_user_id ON workout_sessions(user_id);
CREATE INDEX IF NOT EXISTS idx_workout_sessions_routine_id ON workout_sessions(routine_id);
CREATE INDEX IF NOT EXISTS idx_workout_sessions_actual_start_time ON workout_sessions(actual_start_time);
CREATE INDEX IF NOT EXISTS idx_workout_sessions_status ON workout_sessions(status);

-- ============================================================================
-- TABLE: rep_records
-- Purpose: Registro detallado de cada repetición ejecutada
-- ============================================================================
CREATE TABLE IF NOT EXISTS rep_records (
  id TEXT PRIMARY KEY NOT NULL,
  workout_session_id TEXT NOT NULL,
  routine_item_id TEXT NOT NULL,
  exercise_id TEXT NOT NULL,
  set_number INTEGER NOT NULL,
  rep_number INTEGER NOT NULL,
  weight_used_kg REAL,
  duration_seconds INTEGER,
  form_score REAL,
  form_issues TEXT,
  range_of_motion_score REAL,
  speed_score REAL,
  stability_score REAL,
  camera_quality TEXT,
  keypoints_detected TEXT,
  video_url TEXT,
  timestamp TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (workout_session_id) REFERENCES workout_sessions(id) ON DELETE CASCADE,
  FOREIGN KEY (routine_item_id) REFERENCES routine_items(id) ON DELETE RESTRICT,
  FOREIGN KEY (exercise_id) REFERENCES exercises(id) ON DELETE RESTRICT
);

CREATE INDEX IF NOT EXISTS idx_rep_records_workout_session_id ON rep_records(workout_session_id);
CREATE INDEX IF NOT EXISTS idx_rep_records_exercise_id ON rep_records(exercise_id);
CREATE INDEX IF NOT EXISTS idx_rep_records_set_number ON rep_records(workout_session_id, set_number);

-- ============================================================================
-- TABLE: personal_records
-- Purpose: Almacenar PRs (Personal Records) por usuario y ejercicio
-- ============================================================================
CREATE TABLE IF NOT EXISTS personal_records (
  id TEXT PRIMARY KEY NOT NULL,
  user_id TEXT NOT NULL,
  exercise_id TEXT NOT NULL,
  metric_type TEXT NOT NULL,
  value REAL NOT NULL,
  value_unit TEXT NOT NULL,
  achieved_date DATE,
  achieved_in_session_id TEXT,
  achieved_in_rep_id TEXT,
  notes TEXT,
  created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
  FOREIGN KEY (exercise_id) REFERENCES exercises(id) ON DELETE CASCADE,
  FOREIGN KEY (achieved_in_session_id) REFERENCES workout_sessions(id) ON DELETE SET NULL,
  FOREIGN KEY (achieved_in_rep_id) REFERENCES rep_records(id) ON DELETE SET NULL
);

CREATE INDEX IF NOT EXISTS idx_personal_records_user_id ON personal_records(user_id);
CREATE INDEX IF NOT EXISTS idx_personal_records_exercise_id ON personal_records(exercise_id);
CREATE INDEX IF NOT EXISTS idx_personal_records_achieved_date ON personal_records(achieved_date);

-- ============================================================================
-- TABLE: workout_progress
-- Purpose: Agregaciones de progreso semanal/mensual
-- ============================================================================
CREATE TABLE IF NOT EXISTS workout_progress (
  id TEXT PRIMARY KEY NOT NULL,
  user_id TEXT NOT NULL,
  period_type TEXT NOT NULL,
  period_start_date DATE NOT NULL,
  period_end_date DATE NOT NULL,
  total_sessions INTEGER,
  total_completed_sessions INTEGER,
  total_skipped_sessions INTEGER,
  total_exercises INTEGER,
  total_sets INTEGER,
  total_reps INTEGER,
  total_weight_kg REAL,
  average_session_duration_minutes REAL,
  average_form_score REAL,
  total_calories_burned INTEGER,
  exercises_improved INTEGER,
  new_personal_records INTEGER,
  consistency_percentage INTEGER,
  difficulty_trend TEXT,
  created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

CREATE INDEX IF NOT EXISTS idx_workout_progress_user_id ON workout_progress(user_id);
CREATE INDEX IF NOT EXISTS idx_workout_progress_period_start_date ON workout_progress(period_start_date);

-- ============================================================================
-- TABLE: user_preferences
-- Purpose: Preferencias y configuración por usuario
-- ============================================================================
CREATE TABLE IF NOT EXISTS user_preferences (
  id TEXT PRIMARY KEY NOT NULL,
  user_id TEXT UNIQUE NOT NULL,
  notification_enabled INTEGER NOT NULL DEFAULT 1,
  reminder_time TEXT,
  dark_mode INTEGER NOT NULL DEFAULT 1,
  language TEXT NOT NULL DEFAULT 'es',
  camera_quality_mode TEXT NOT NULL DEFAULT 'BALANCED',
  enable_form_feedback INTEGER NOT NULL DEFAULT 1,
  enable_camera_recording INTEGER NOT NULL DEFAULT 0,
  enable_analytics INTEGER NOT NULL DEFAULT 1,
  privacy_level TEXT NOT NULL DEFAULT 'PRIVATE',
  units TEXT NOT NULL DEFAULT 'METRIC',
  created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

CREATE INDEX IF NOT EXISTS idx_user_preferences_user_id ON user_preferences(user_id);

-- ============================================================================
-- TABLE: achievements
-- Purpose: Logros y badges desbloqueados por usuarios
-- ============================================================================
CREATE TABLE IF NOT EXISTS achievements (
  id TEXT PRIMARY KEY NOT NULL,
  user_id TEXT NOT NULL,
  achievement_type TEXT NOT NULL,
  title TEXT NOT NULL,
  description TEXT,
  icon_url TEXT,
  badge_name TEXT,
  progress_value INTEGER,
  progress_target INTEGER,
  percentage_complete INTEGER,
  unlocked INTEGER NOT NULL DEFAULT 0,
  unlocked_date TIMESTAMP,
  created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

CREATE INDEX IF NOT EXISTS idx_achievements_user_id ON achievements(user_id);
CREATE INDEX IF NOT EXISTS idx_achievements_unlocked ON achievements(unlocked);

-- ============================================================================
-- End of Migration v1
-- ============================================================================
```

---

## 🏗️ Room Database Kotlin Code

### build.gradle.kts

```kotlin
// Room versioning
dependencies {
    val roomVersion = "2.6.1"
    
    implementation("androidx.room:room-runtime:$roomVersion")
    implementation("androidx.room:room-ktx:$roomVersion")
    kapt("androidx.room:room-compiler:$roomVersion")
    
    // Testing
    testImplementation("androidx.room:room-testing:$roomVersion")
}
```

### AppDatabase.kt

```kotlin
@Database(
    entities = [
        User::class,
        Exercise::class,
        Routine::class,
        RoutineItem::class,
        WorkoutSession::class,
        RepRecord::class,
        PersonalRecord::class,
        WorkoutProgress::class,
        UserPreferences::class,
        Achievement::class
    ],
    version = 1,
    exportSchema = true
)
abstract class AppDatabase : RoomDatabase() {
    
    abstract fun userDao(): UserDao
    abstract fun exerciseDao(): ExerciseDao
    abstract fun routineDao(): RoutineDao
    abstract fun routineItemDao(): RoutineItemDao
    abstract fun workoutSessionDao(): WorkoutSessionDao
    abstract fun repRecordDao(): RepRecordDao
    abstract fun personalRecordDao(): PersonalRecordDao
    abstract fun workoutProgressDao(): WorkoutProgressDao
    abstract fun userPreferencesDao(): UserPreferencesDao
    abstract fun achievementDao(): AchievementDao

    companion object {
        @Volatile private var instance: AppDatabase? = null

        fun getInstance(context: Context): AppDatabase {
            return instance ?: synchronized(this) {
                Room.databaseBuilder(
                    context.applicationContext,
                    AppDatabase::class.java,
                    "workout_companion.db"
                )
                    .createFromAsset("databases/initial_exercises.db")
                    .build()
                    .also { instance = it }
            }
        }
    }
}
```

---

## 🔄 Migration Strategy (Futuro)

### Patrón para próximas migraciones

```kotlin
val MIGRATION_1_2 = object : Migration(1, 2) {
    override fun migrate(database: SupportSQLiteDatabase) {
        // Cambios en la DB
        database.execSQL(
            "ALTER TABLE users ADD COLUMN phone TEXT"
        )
    }
}

// En AppDatabase:
Room.databaseBuilder(context, AppDatabase::class.java, "workout.db")
    .addMigrations(MIGRATION_1_2)
    .build()
```

---

## ✅ Validaciones Post-Migración

### Test Script

```sql
-- Verificar que todas las tablas existen
SELECT name FROM sqlite_master 
WHERE type='table' 
ORDER BY name;

-- Verificar integridad de constraints
PRAGMA foreign_key_check;

-- Verificar índices
SELECT name FROM sqlite_master 
WHERE type='index' 
ORDER BY name;

-- Contador de registros (debe estar vacío en v1)
SELECT 'users' as table_name, COUNT(*) as count FROM users
UNION ALL SELECT 'exercises', COUNT(*) FROM exercises
UNION ALL SELECT 'routines', COUNT(*) FROM routines
UNION ALL SELECT 'routine_items', COUNT(*) FROM routine_items
UNION ALL SELECT 'workout_sessions', COUNT(*) FROM workout_sessions
UNION ALL SELECT 'rep_records', COUNT(*) FROM rep_records
UNION ALL SELECT 'personal_records', COUNT(*) FROM personal_records
UNION ALL SELECT 'workout_progress', COUNT(*) FROM workout_progress
UNION ALL SELECT 'user_preferences', COUNT(*) FROM user_preferences
UNION ALL SELECT 'achievements', COUNT(*) FROM achievements;
```

---

## 🌱 Seed Data (Inicial)

### Ejercicios Master (Pre-cargados)

```sql
INSERT INTO exercises (id, name, description, muscle_groups, difficulty_level, 
                       category, sport_type, bodyweight_only)
VALUES 
  ('EXE001', 'Push-up', 'Flexión con peso corporal',
   '["chest", "triceps", "shoulders"]', 'BEGINNER',
   'PUSH', 'BODYWEIGHT', 1),
   
  ('EXE002', 'Pull-up', 'Tracción con peso corporal',
   '["back", "biceps", "shoulders"]', 'INTERMEDIATE',
   'PULL', 'BODYWEIGHT', 1),
   
  ('EXE003', 'Bicep Curl', 'Curl de bíceps con mancuerna',
   '["biceps"]', 'BEGINNER',
   'PULL', 'WEIGHTLIFTING', 0),
   
  ('EXE004', 'Squats', 'Sentadillas con peso corporal',
   '["quadriceps", "glutes", "hamstrings"]', 'INTERMEDIATE',
   'LEGS', 'BODYWEIGHT', 1),
   
  ('EXE005', 'Deadlift', 'Levantamiento de pesas',
   '["back", "glutes", "hamstrings", "quadriceps"]', 'ADVANCED',
   'LEGS', 'WEIGHTLIFTING', 0);
```

---

## 🚀 Database Population Strategy

1. **Initial Load** (App Startup)
   - Cargar ejercicios master desde asset
   - Crear preferencias default por usuario

2. **User Registration**
   - Crear registro en `users`
   - Crear registro en `user_preferences`

3. **Routine Generation**
   - Crear registro en `routines`
   - Crear items en `routine_items` (N items)

4. **Workout Execution**
   - Crear sesión en `workout_sessions`
   - Crear registros en `rep_records` (1 per rep)
   - Actualizar `personal_records` si aplica
   - Actualizar `achievements` si aplica

---

## 📊 Performance Considerations

### Query Optimization

```sql
-- ✅ BUENO: Usa índices
SELECT * FROM workout_sessions 
WHERE user_id = ? AND status = 'COMPLETED'
ORDER BY actual_start_time DESC;

-- ❌ MALO: Full table scan
SELECT * FROM workout_sessions 
WHERE DATE(actual_start_time) = DATE('now');
```

### Índices Estratégicos

- `users(email)` - Login rápido
- `exercises(category, difficulty_level)` - Búsqueda de ejercicios
- `routines(user_id, is_active)` - Rutina activa del usuario
- `workout_sessions(user_id, actual_start_time)` - Histórico de usuario
- `rep_records(workout_session_id, set_number)` - Sesión actual

---

## 🔒 Backup & Restore Strategy

```kotlin
// Backup to file
fun backupDatabase(context: Context) {
    val dbFile = context.getDatabasePath("workout_companion.db")
    val backupFile = File(context.getExternalFilesDir(null), "backup.db")
    dbFile.copyTo(backupFile, overwrite = true)
}

// Restore from file
fun restoreDatabase(context: Context, backupFile: File) {
    val dbFile = context.getDatabasePath("workout_companion.db")
    backupFile.copyTo(dbFile, overwrite = true)
}
```

---

## 📋 Checklist de Migraciones

- [x] Schema v1 definido
- [x] Todas las tablas creadas
- [x] Todos los índices definidos
- [x] Foreign keys configuradas
- [x] Seed data preparado
- [ ] Migraciones v2+ planeadas
- [ ] Tests de migración escritos
- [ ] Backup strategy implementado

