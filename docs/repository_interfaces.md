# Repository Interfaces & Clean Architecture

## 📐 Arquitectura en Capas

```
┌─────────────────────────────────────────────────┐
│          PRESENTATION LAYER (UI)                │
│  Composables, ViewModels, State Management     │
└─────────────────────────────────────────────────┘
              ↓ (depende de)
┌─────────────────────────────────────────────────┐
│        DOMAIN LAYER (Business Logic)            │
│  Use Cases, Repositories Interfaces, Entities  │
└─────────────────────────────────────────────────┘
              ↓ (implementa)
┌─────────────────────────────────────────────────┐
│        DATA LAYER (Persistence & API)           │
│  DAOs, Database, Network, Repository Impl      │
└─────────────────────────────────────────────────┘
              ↓ (accede)
┌─────────────────────────────────────────────────┐
│      DATA SOURCES (Room DB, API, Cache)         │
└─────────────────────────────────────────────────┘
```

---

## 🏗️ Repository Interface Definitions

### 1. UserRepository

**Ubicación:** `domain/repositories/UserRepository.kt`

```kotlin
interface UserRepository {
    
    // Read Operations
    suspend fun getUserById(userId: String): Result<User>
    suspend fun getUserByEmail(email: String): Result<User>
    suspend fun getAllUsers(): Result<List<User>>
    
    // Write Operations
    suspend fun createUser(user: User): Result<String>  // Returns userId
    suspend fun updateUser(user: User): Result<Unit>
    suspend fun deleteUser(userId: String): Result<Unit>
    
    // Authentication
    suspend fun getUserLastLogin(userId: String): Result<LocalDateTime?>
    suspend fun updateLastLogin(userId: String): Result<Unit>
    
    // Statistics
    suspend fun getUserCount(): Result<Int>
    suspend fun getActiveUsersCount(): Result<Int>
    
    // Preferences
    suspend fun getUserProfile(userId: String): Result<UserProfile>
    suspend fun updateUserProfile(userId: String, profile: UserProfile): Result<Unit>
}
```

---

### 2. ExerciseRepository

**Ubicación:** `domain/repositories/ExerciseRepository.kt`

```kotlin
interface ExerciseRepository {
    
    // Read Operations
    suspend fun getExerciseById(exerciseId: String): Result<Exercise>
    suspend fun getExercisesByCategory(category: ExerciseCategory): Result<List<Exercise>>
    suspend fun getExercisesByMuscleGroup(muscleGroup: String): Result<List<Exercise>>
    suspend fun getExercisesByDifficulty(difficulty: DifficultyLevel): Result<List<Exercise>>
    suspend fun getAllExercises(): Result<List<Exercise>>
    suspend fun searchExercises(query: String): Result<List<Exercise>>
    
    // Filter Operations
    suspend fun getExercisesWithEquipment(equipment: List<String>): Result<List<Exercise>>
    suspend fun getBodyweightExercises(): Result<List<Exercise>>
    suspend fun getExercisesForUserGoal(goal: UserGoal): Result<List<Exercise>>
    
    // Statistics
    suspend fun getExerciseCount(): Result<Int>
    suspend fun getExerciseCountByCategory(category: ExerciseCategory): Result<Int>
    
    // Admin Operations (future)
    suspend fun createExercise(exercise: Exercise): Result<String>
    suspend fun updateExercise(exercise: Exercise): Result<Unit>
    suspend fun deleteExercise(exerciseId: String): Result<Unit>
}
```

---

### 3. RoutineRepository

**Ubicación:** `domain/repositories/RoutineRepository.kt`

```kotlin
interface RoutineRepository {
    
    // Read Operations
    suspend fun getRoutineById(routineId: String): Result<Routine>
    suspend fun getUserRoutines(userId: String): Result<List<Routine>>
    suspend fun getUserActiveRoutine(userId: String): Result<Routine?>
    suspend fun getUserRoutineHistory(userId: String, limit: Int = 10): Result<List<Routine>>
    
    // Create & Update
    suspend fun createRoutine(routine: Routine): Result<String>  // Returns routineId
    suspend fun updateRoutine(routine: Routine): Result<Unit>
    suspend fun deleteRoutine(routineId: String): Result<Unit>
    
    // Activation & Status
    suspend fun activateRoutine(routineId: String): Result<Unit>
    suspend fun deactivateRoutine(routineId: String): Result<Unit>
    suspend fun completeRoutine(routineId: String): Result<Unit>
    
    // Items Management
    suspend fun getRoutineItems(routineId: String): Result<List<RoutineItem>>
    suspend fun addRoutineItem(item: RoutineItem): Result<String>
    suspend fun updateRoutineItem(item: RoutineItem): Result<Unit>
    suspend fun removeRoutineItem(itemId: String): Result<Unit>
    suspend fun reorderRoutineItems(routineId: String, itemIds: List<String>): Result<Unit>
    
    // Customization
    suspend fun customizeRoutineItem(
        itemId: String,
        weight: Double?,
        sets: Int?,
        reps: Int?
    ): Result<Unit>
    
    suspend fun getRoutineAlternativeExercises(
        routineItemId: String,
        limit: Int = 5
    ): Result<List<Exercise>>
    
    // Validation
    suspend fun validateRoutineComplete(routineId: String): Result<Boolean>
}
```

---

### 4. WorkoutRepository

**Ubicación:** `domain/repositories/WorkoutRepository.kt`

```kotlin
interface WorkoutRepository {
    
    // Session Management
    suspend fun createWorkoutSession(
        userId: String,
        routineId: String,
        scheduledDate: LocalDate?
    ): Result<String>  // Returns sessionId
    
    suspend fun getWorkoutSession(sessionId: String): Result<WorkoutSession>
    suspend fun getUserWorkoutSessions(userId: String, limit: Int = 20): Result<List<WorkoutSession>>
    suspend fun getWorkoutSessionsInDateRange(
        userId: String,
        startDate: LocalDate,
        endDate: LocalDate
    ): Result<List<WorkoutSession>>
    
    // Session Status Updates
    suspend fun startWorkoutSession(sessionId: String): Result<Unit>
    suspend fun pauseWorkoutSession(sessionId: String): Result<Unit>
    suspend fun resumeWorkoutSession(sessionId: String): Result<Unit>
    suspend fun completeWorkoutSession(sessionId: String): Result<Unit>
    suspend fun cancelWorkoutSession(sessionId: String): Result<Unit>
    
    // Rep Recording
    suspend fun recordRep(rep: RepRecord): Result<String>  // Returns repId
    suspend fun recordBulkReps(reps: List<RepRecord>): Result<List<String>>
    suspend fun updateRep(rep: RepRecord): Result<Unit>
    suspend fun deleteRep(repId: String): Result<Unit>
    
    // Rep Retrieval
    suspend fun getSessionReps(sessionId: String): Result<List<RepRecord>>
    suspend fun getSetReps(sessionId: String, setNumber: Int): Result<List<RepRecord>>
    suspend fun getCurrentRep(sessionId: String): Result<RepRecord?>
    
    // Session Analytics
    suspend fun getSessionFormScore(sessionId: String): Result<Double>
    suspend fun getSessionCalories(sessionId: String): Result<Int>
    suspend fun getSessionDuration(sessionId: String): Result<Int>  // minutes
    
    // Session Update
    suspend fun updateSessionMetadata(
        sessionId: String,
        difficulty: DifficultyFelt?,
        mood: MoodLevel?,
        notes: String?
    ): Result<Unit>
    
    // Completion Data
    suspend fun markSessionCompleted(
        sessionId: String,
        moodAfter: MoodLevel,
        energyAfter: Int,
        calories: Int
    ): Result<Unit>
}
```

---

### 5. ProgressRepository

**Ubicación:** `domain/repositories/ProgressRepository.kt`

```kotlin
interface ProgressRepository {
    
    // Personal Records
    suspend fun getPersonalRecord(
        userId: String,
        exerciseId: String,
        metricType: PRMetricType
    ): Result<PersonalRecord?>
    
    suspend fun getUserPersonalRecords(userId: String): Result<List<PersonalRecord>>
    suspend fun getExercisePersonalRecords(
        userId: String,
        exerciseId: String
    ): Result<List<PersonalRecord>>
    
    suspend fun createOrUpdatePersonalRecord(
        pr: PersonalRecord
    ): Result<Unit>
    
    suspend fun deletePersonalRecord(prId: String): Result<Unit>
    
    // Form Score Tracking
    suspend fun getAverageFormScore(
        userId: String,
        exerciseId: String,
        days: Int = 30
    ): Result<Double>
    
    suspend fun getFormScoreTrend(
        userId: String,
        exerciseId: String,
        days: Int = 30
    ): Result<List<Pair<LocalDate, Double>>>
    
    // Progress Aggregation
    suspend fun getWeeklyProgress(
        userId: String,
        week: Int? = null  // null = current week
    ): Result<WorkoutProgress>
    
    suspend fun getMonthlyProgress(
        userId: String,
        month: Int? = null  // null = current month
    ): Result<WorkoutProgress>
    
    suspend fun getProgressHistory(
        userId: String,
        periodType: PeriodType,
        limit: Int = 12
    ): Result<List<WorkoutProgress>>
    
    // Statistics
    suspend fun getTotalVolume(
        userId: String,
        exerciseId: String,
        days: Int? = null
    ): Result<Double>  // kg
    
    suspend fun getTotalReps(
        userId: String,
        exerciseId: String,
        days: Int? = null
    ): Result<Int>
    
    suspend fun getConsistencyScore(
        userId: String,
        days: Int = 30
    ): Result<Int>  // 0-100 percentage
    
    suspend fun getStreak(userId: String): Result<Int>  // consecutive workout days
    
    // Comparison
    suspend fun compareUserProgress(
        userId: String,
        exerciseId: String,
        startDate: LocalDate,
        endDate: LocalDate
    ): Result<ProgressComparison>
}
```

---

### 6. PreferencesRepository

**Ubicación:** `domain/repositories/PreferencesRepository.kt`

```kotlin
interface PreferencesRepository {
    
    // Read
    suspend fun getUserPreferences(userId: String): Result<UserPreferences>
    
    // Update Individual Settings
    suspend fun updateNotificationEnabled(userId: String, enabled: Boolean): Result<Unit>
    suspend fun updateReminderTime(userId: String, time: LocalTime): Result<Unit>
    suspend fun updateDarkMode(userId: String, enabled: Boolean): Result<Unit>
    suspend fun updateLanguage(userId: String, language: String): Result<Unit>
    suspend fun updateCameraQualityMode(userId: String, mode: CameraQualityMode): Result<Unit>
    suspend fun updateFormFeedback(userId: String, enabled: Boolean): Result<Unit>
    suspend fun updateCameraRecording(userId: String, enabled: Boolean): Result<Unit>
    suspend fun updateAnalytics(userId: String, enabled: Boolean): Result<Unit>
    suspend fun updatePrivacyLevel(userId: String, level: PrivacyLevel): Result<Unit>
    suspend fun updateUnits(userId: String, units: UnitSystem): Result<Unit>
    
    // Update All
    suspend fun updateAllPreferences(preferences: UserPreferences): Result<Unit>
    
    // Defaults
    suspend fun resetToDefaults(userId: String): Result<Unit>
    
    // Reactive
    fun getPreferencesFlow(userId: String): Flow<UserPreferences>
    fun getDarkModeFlow(userId: String): Flow<Boolean>
    fun getLanguageFlow(userId: String): Flow<String>
}
```

---

### 7. AchievementRepository

**Ubicación:** `domain/repositories/AchievementRepository.kt`

```kotlin
interface AchievementRepository {
    
    // Retrieval
    suspend fun getUserAchievements(userId: String): Result<List<Achievement>>
    suspend fun getUnlockedAchievements(userId: String): Result<List<Achievement>>
    suspend fun getLockedAchievements(userId: String): Result<List<Achievement>>
    suspend fun getAchievementById(achievementId: String): Result<Achievement>
    suspend fun getAchievementsByType(type: AchievementType): Result<List<Achievement>>
    
    // Progress Tracking
    suspend fun updateAchievementProgress(
        userId: String,
        achievementType: AchievementType,
        progressValue: Int
    ): Result<Unit>
    
    suspend fun getAchievementProgress(
        userId: String,
        achievementId: String
    ): Result<AchievementProgress>
    
    // Unlocking
    suspend fun unlockAchievement(
        userId: String,
        achievementId: String
    ): Result<Boolean>  // true if newly unlocked
    
    suspend fun checkAndUnlockEligible(userId: String): Result<List<String>>  // unlocked IDs
    
    // Statistics
    suspend fun getTotalAchievements(): Result<Int>
    suspend fun getUserAchievementCount(userId: String): Result<Int>
    suspend fun getUserUnlockedCount(userId: String): Result<Int>
    suspend fun getUserCompletionPercentage(userId: String): Result<Int>  // 0-100
    
    // Creation (admin)
    suspend fun createAchievement(achievement: Achievement): Result<String>
}
```

---

## 🔄 Data Flow Pattern

### Patrón Típico de Lectura

```
UI Layer
   ↓
ViewModel
   ↓ (calls use case)
UseCase
   ↓ (depends on repository)
Repository Interface (domain)
   ↓ (implemented by)
Repository Implementation (data)
   ↓ (uses DAO or API)
Room DAO / Remote API
   ↓
Database / Backend
```

**Ejemplo:**

```kotlin
// UI
val activeRoutine by viewModel.activeRoutine.collectAsState()

// ViewModel
private val _activeRoutine = MutableStateFlow<Routine?>(null)
init {
    viewModelScope.launch {
        getActiveRoutineUseCase(userId).collect {
            _activeRoutine.value = it
        }
    }
}

// UseCase
class GetActiveRoutineUseCase(private val routineRepo: RoutineRepository) {
    suspend operator fun invoke(userId: String): Flow<Routine?> =
        routineRepo.getUserActiveRoutine(userId).asFlow()
}

// Repository Implementation
class RoutineRepositoryImpl(private val routineDao: RoutineDao) : RoutineRepository {
    override suspend fun getUserActiveRoutine(userId: String): Result<Routine?> {
        return try {
            val routine = routineDao.getUserActiveRoutine(userId)
            Result.success(routine)
        } catch (e: Exception) {
            Result.failure(e)
        }
    }
}

// DAO
@Dao
interface RoutineDao {
    @Query("SELECT * FROM routines WHERE user_id = :userId AND is_active = 1 LIMIT 1")
    suspend fun getUserActiveRoutine(userId: String): Routine?
}
```

---

## 🚀 Dependency Injection Setup

### Hilt Modules

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object RepositoryModule {
    
    @Singleton
    @Provides
    fun provideUserRepository(
        userDao: UserDao,
        userPreferencesDao: UserPreferencesDao
    ): UserRepository = UserRepositoryImpl(userDao, userPreferencesDao)
    
    @Singleton
    @Provides
    fun provideExerciseRepository(
        exerciseDao: ExerciseDao
    ): ExerciseRepository = ExerciseRepositoryImpl(exerciseDao)
    
    @Singleton
    @Provides
    fun provideRoutineRepository(
        routineDao: RoutineDao,
        routineItemDao: RoutineItemDao,
        exerciseDao: ExerciseDao
    ): RoutineRepository = RoutineRepositoryImpl(routineDao, routineItemDao, exerciseDao)
    
    @Singleton
    @Provides
    fun provideWorkoutRepository(
        workoutSessionDao: WorkoutSessionDao,
        repRecordDao: RepRecordDao,
        personalRecordDao: PersonalRecordDao,
        achievementDao: AchievementDao
    ): WorkoutRepository = WorkoutRepositoryImpl(
        workoutSessionDao, 
        repRecordDao, 
        personalRecordDao,
        achievementDao
    )
    
    @Singleton
    @Provides
    fun provideProgressRepository(
        personalRecordDao: PersonalRecordDao,
        repRecordDao: RepRecordDao,
        workoutProgressDao: WorkoutProgressDao,
        workoutSessionDao: WorkoutSessionDao
    ): ProgressRepository = ProgressRepositoryImpl(
        personalRecordDao,
        repRecordDao,
        workoutProgressDao,
        workoutSessionDao
    )
    
    @Singleton
    @Provides
    fun providePreferencesRepository(
        userPreferencesDao: UserPreferencesDao
    ): PreferencesRepository = PreferencesRepositoryImpl(userPreferencesDao)
    
    @Singleton
    @Provides
    fun provideAchievementRepository(
        achievementDao: AchievementDao,
        workoutSessionDao: WorkoutSessionDao,
        repRecordDao: RepRecordDao
    ): AchievementRepository = AchievementRepositoryImpl(
        achievementDao,
        workoutSessionDao,
        repRecordDao
    )
}
```

---

## 📦 Result Type Wrapper

```kotlin
sealed class Result<out T> {
    data class Success<T>(val data: T) : Result<T>()
    data class Error(val exception: Throwable) : Result<Nothing>()
    object Loading : Result<Nothing>()
    
    suspend inline fun <R> mapSuspend(transform: suspend (T) -> R): Result<R> =
        when (this) {
            is Success -> try {
                Success(transform(data))
            } catch (e: Exception) {
                Error(e)
            }
            is Error -> this
            Loading -> Loading
        }
    
    fun getOrNull(): T? = (this as? Success)?.data
    
    fun exceptionOrNull(): Throwable? = (this as? Error)?.exception
}

// Extension functions
suspend inline fun <T, R> Result<T>.fold(
    onSuccess: suspend (T) -> R,
    onError: suspend (Throwable) -> R,
    onLoading: suspend () -> R = { throw IllegalStateException() }
): R = when (this) {
    is Result.Success -> onSuccess(data)
    is Result.Error -> onError(exception)
    Result.Loading -> onLoading()
}
```

---

## ✅ Testing Repositories

### Mock Repository

```kotlin
class MockUserRepository : UserRepository {
    private val users = mutableMapOf<String, User>()
    
    override suspend fun getUserById(userId: String): Result<User> {
        return users[userId]?.let { Result.success(it) }
            ?: Result.failure(Exception("User not found"))
    }
    
    override suspend fun createUser(user: User): Result<String> {
        users[user.id] = user
        return Result.success(user.id)
    }
    
    // ... rest of methods
}
```

### Repository Test Example

```kotlin
class UserRepositoryTest {
    private lateinit var repository: UserRepository
    private lateinit var userDao: UserDao
    
    @Before
    fun setup() {
        userDao = mockk()
        repository = UserRepositoryImpl(userDao)
    }
    
    @Test
    fun getUserById_success() = runTest {
        val userId = "user123"
        val expectedUser = User(id = userId, email = "test@example.com")
        coEvery { userDao.getUserById(userId) } returns expectedUser
        
        val result = repository.getUserById(userId)
        
        assertTrue(result.isSuccess)
        assertEquals(expectedUser, result.getOrNull())
    }
}
```

---

## 📋 Checklist de Implementación

- [ ] UserRepository implementado
- [ ] ExerciseRepository implementado
- [ ] RoutineRepository implementado
- [ ] WorkoutRepository implementado
- [ ] ProgressRepository implementado
- [ ] PreferencesRepository implementado
- [ ] AchievementRepository implementado
- [ ] Hilt modules configurados
- [ ] Tests escritos para cada repository
- [ ] CI/CD pipeline ajustado

