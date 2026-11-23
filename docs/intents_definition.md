# 📋 DEFINICIÓN DE INTENTS - App de Ejercicios Personalizados

**Proyecto:** App Android de Rutinas de Fuerza Personalizadas + Corrección de Postura  
**Metodología:** Intent Driven Development (IDD)  
**Fecha:** 2026-11-23

---

## 🎯 INTENTS PRINCIPALES

### **INTENT 1: Usuario Principiante Quiere Rutina Personalizada**

```gherkin
Feature: Crear Rutina Personalizada Automática
  As a novice user
  I want to get a customized workout routine without having to decide exercises
  So that I can start training immediately with confidence

  Background:
    Given the app is installed and opened for the first time
    And no routine has been created yet

  Scenario: Complete profile and receive personalized routine
    When I open the "Create Routine" section
    Then I should see a questionnaire with the following fields:
      | Field                  | Type       | Options/Format                              |
      | Training Goal          | Radio      | Muscle Gain, Fat Loss, Strength, Endurance  |
      | Experience Level       | Radio      | Beginner, Intermediate, Advanced            |
      | Available Equipment    | Checkbox   | Dumbbells, Barbell, Machines, None (Calisthenics) |
      | Training Frequency     | Radio      | 3 days/week, 4 days/week, 5 days/week       |
      | Session Duration       | Radio      | 30 min, 45 min, 60 min, 90 min              |
      | Body Parts Priority    | Checkbox   | Chest, Back, Legs, Arms, Shoulders, Core    |
      | Injuries/Limitations   | Text       | (Optional) Any physical limitations          |
    
    When I fill all required fields with valid data
    And I click "Generate Routine"
    Then the system should:
      ✓ Validate all inputs
      ✓ Generate routine in < 5 seconds
      ✓ Return a routine with 4-6 exercises (based on frequency and duration)
      ✓ Ensure all exercises match selected equipment
      ✓ Prioritize selected body parts (60% of exercises)
      ✓ Exclude exercises related to injuries
      ✓ Display routine name and description
    
    And I should see routine details:
      | Detail                 | Expected                                    |
      | Routine Name           | Auto-generated (e.g., "Beginner Push Day")  |
      | Weekly Schedule        | Show which days to train                     |
      | Exercises per Session  | 4-6 exercises with sets, reps, rest time    |
      | Difficulty Badge       | Visual indicator (Beginner/Inter/Advanced)  |
      | Est. Session Duration  | Calculated time (match user preference ±5%) |
    
    When I scroll through the exercises
    Then each exercise should display:
      ✓ Exercise name
      ✓ Sets x Repetitions
      ✓ Rest time between sets (in seconds)
      ✓ Primary and secondary muscles worked
      ✓ Difficulty level (1-10 scale)
      ✓ Brief description/technique tips
      ✓ Equipment needed
      ✓ Thumbnail image (if available)
    
    When I click on an exercise
    Then a detail modal should show:
      ✓ Full description with step-by-step instructions
      ✓ Proper form tips (3-5 key points)
      ✓ Common mistakes to avoid
      ✓ Alternative variations
      ✓ Video link (if available)
    
    When I click "Save Routine"
    Then the system should:
      ✓ Store routine in local database
      ✓ Create a default weekly schedule
      ✓ Show success message
      ✓ Redirect to routine detail page

  Scenario: Modify generated routine
    Given a personalized routine has been generated
    When I click "Edit Routine"
    Then I should be able to:
      ✓ Change routine name
      ✓ Modify training days
      ✓ Replace individual exercises with alternatives
      ✓ Adjust sets/reps/rest times
      ✓ Change session duration
    
    When I make changes and click "Save"
    Then the system should:
      ✓ Validate changes (don't allow invalid combinations)
      ✓ Update routine in database
      ✓ Show "Routine Updated" confirmation

  Scenario: View routine alternatives
    Given I'm viewing a generated routine
    When I see an exercise I don't like
    And I click "Show Alternatives"
    Then the system should:
      ✓ Display 3-5 alternative exercises
      ✓ Each alternative works same muscle groups
      ✓ Each requires same equipment
      ✓ Maintain routine structure (sets/reps/rest)
    
    When I select an alternative
    Then it should:
      ✓ Replace the original exercise
      ✓ Update routine preview
      ✓ Maintain all other exercises

---

### **INTENT 2: Usuario Quiere Verificar Postura en Tiempo Real**

```gherkin
Feature: Validación de Postura con Cámara en Tiempo Real
  As a user training alone
  I want real-time feedback on my exercise form/posture
  So that I avoid injuries and perform exercises correctly

  Background:
    Given a routine is active/being executed
    And user has granted camera permissions
    And device has sufficient lighting

  Scenario: Enable posture detection for an exercise
    Given I'm on an exercise detail page (e.g., "Squat")
    When I click "Start Exercise" or "Enable Camera"
    Then the system should:
      ✓ Request camera permission (if not granted)
      ✓ Open camera preview (back camera by default)
      ✓ Initialize pose detection model
      ✓ Show "Analyzing posture..." indicator
      ✓ Display skeleton/joint visualization overlay
      ✓ Show current status: "Ready", "Detecting", or "Analyzing"
    
    When pose detection is ready
    Then I should see:
      ✓ Live camera feed with 2D skeleton overlay
      ✓ Joint circles at: head, shoulders, elbows, wrists, hips, knees, ankles
      ✓ Connection lines between joints
      ✓ Confidence score for detection (0-100%)
      ✓ Instructions panel: "Perform the movement now"
      ✓ Countdown timer (3-2-1 to start recording)

  Scenario: Detect incorrect posture during squat
    Given pose detection is active for "Squat" exercise
    When I perform a squat incorrectly:
      | Issue                      | Detection                                   |
      | Knees caving inward        | Knee angle < 170° or not parallel           |
      | Back rounding              | Spine not vertical (torso angle > 45°)      |
      | Heels off ground           | Ankle/foot position deviation detected      |
      | Not reaching depth         | Hip-knee angle > 90° (insufficient depth)   |
      | Asymmetric weight          | Shoulder/hip misalignment detected          |
    
    Then the system should:
      ✓ Detect the issue in real-time
      ✓ Change skeleton color to RED
      ✓ Display warning message on screen: "Knees collapsing inward!"
      ✓ Highlight the problematic joint (red circle)
      ✓ Show correction tip: "Keep knees parallel to toes"
      ✓ Play vibration feedback (optional)
      ✓ Log this rep as "INCORRECT FORM"

  Scenario: Detect correct posture during squat
    Given pose detection is active for "Squat" exercise
    When I perform a squat correctly:
      ✓ Knees parallel to toes (angle 160-175°)
      ✓ Back straight (spine angle 0-30°)
      ✓ Heels on ground
      ✓ Hip depth reaches 90°+ angle
      ✓ Symmetric weight distribution
    
    Then the system should:
      ✓ Keep skeleton color GREEN
      ✓ Display "Perfect Form!" message
      ✓ Show "Rep Count: 1" at top
      ✓ Play success sound (optional)
      ✓ Accumulate rep counter
      ✓ Log this rep as "CORRECT FORM"
    
    When I complete the rep and return to starting position
    Then the system should:
      ✓ Reset for next rep
      ✓ Ready for next repetition

  Scenario: Complete set with form tracking
    Given I'm performing a set of 10 squats with posture detection
    When I complete 10 repetitions:
      ✓ 7 reps with correct form (GREEN)
      ✓ 2 reps with incorrect form (RED)
      ✓ 1 rep partially correct (YELLOW)
    
    Then after completing the set, show summary:
      ✓ Total reps: 10
      ✓ Correct reps: 7 (70%)
      ✓ Incorrect reps: 2 (20%)
      ✓ Form score: 70/100
      ✓ Detailed breakdown by issue type
      ✓ Option to: Continue, Redo Set, Next Exercise

  Scenario: Handle poor lighting conditions
    Given pose detection is active
    When lighting conditions are insufficient
    Then the system should:
      ✓ Detect low confidence in pose detection
      ✓ Show warning: "Poor lighting. Move to a brighter area."
      ✓ Freeze form feedback (don't give false corrections)
      ✓ Reduce skeleton visibility (gray color)
      ✓ Offer option to use flashlight
    
    When lighting improves
    Then the system should:
      ✓ Automatically resume detection
      ✓ Show "Lighting improved. Ready." message

  Scenario: Exercise-specific posture checks
    Feature: Different exercises have different validations
    
    Given user is performing "Bench Press"
    Then system validates:
      ✓ Shoulder angle (not impingement position)
      ✓ Elbow depth (not locked)
      ✓ Back position (flat on bench)
      ✓ Bar path (vertical line)
    
    Given user is performing "Deadlift"
    Then system validates:
      ✓ Spine alignment (neutral, not rounded)
      ✓ Hip hinge pattern (not squat)
      ✓ Shoulder position (over bar)
      ✓ Knee path (tracking toes)
    
    Given user is performing "Push-up"
    Then system validates:
      ✓ Body alignment (straight line head to heels)
      ✓ Elbow position (45° from body)
      ✓ Core engagement (no sagging hips)
      ✓ Full range of motion (chest near ground)

---

### **INTENT 3: Usuario Quiere Rastrear Progreso y Mejora**

```gherkin
Feature: Rastrear Progreso y Visualizar Mejora
  As a user training regularly
  I want to track my workout performance and see my progress over time
  So that I stay motivated and can adjust my routine accordingly

  Background:
    Given user has completed at least one workout session
    And all session data is stored in database

  Scenario: Record completed workout
    Given I'm executing an active routine
    When I complete all sets for an exercise
    Then I should see a data entry form:
      | Field                  | Input Type | Requirement           |
      | Exercise Name          | Auto-filled| (read-only)           |
      | Sets Completed         | Number     | 1 to max_sets         |
      | Reps per Set           | Input List | Enter reps for each   |
      | Weight Used            | Decimal    | In kg or lbs (setting) |
      | Form Score             | Auto       | From camera detection |
      | Difficulty Rating      | 1-10 scale | User subjective input |
      | Notes                  | Text       | Optional observations |
      | Time Taken             | Auto       | Auto-recorded         |
    
    When I click "Save Set"
    Then the system should:
      ✓ Validate all required fields
      ✓ Store data in database with timestamp
      ✓ Show "✓ Saved" confirmation
      ✓ Move to next exercise or show set summary

  Scenario: View personal records
    Given user has completed multiple workouts
    When I navigate to "Progress" or "Stats" section
    Then I should see:
      ✓ Personal Records (PRs) for each exercise
      ✓ Display: Exercise Name | Weight | Date | Reps
      ✓ Sort by date (most recent first)
      ✓ Highlight new PRs
      ✓ Show previous PR for comparison
    
    When I click on an exercise PR
    Then show details:
      ✓ PR history (top 5 previous attempts)
      ✓ Trend graph (weight progression over time)
      ✓ Date achieved
      ✓ Form score on that day

  Scenario: View workout history
    Given user has multiple completed workouts
    When I click "Workout History"
    Then I should see:
      ✓ List of all completed workouts (calendar view)
      ✓ Each entry shows: Date | Routine | Exercises | Duration
      ✓ Completion percentage (reps completed vs planned)
      ✓ Total volume (sum of weight × reps)
      ✓ Average form score
    
    When I click on a past workout
    Then show detailed recap:
      ✓ All exercises performed
      ✓ Sets, reps, weight for each
      ✓ Form score per exercise
      ✓ Notes taken during session
      ✓ Total session duration
      ✓ Calories burned (estimate)

  Scenario: View progress charts
    Given user has 4+ weeks of data
    When I navigate to "Analytics"
    Then I should see charts for:
    
    Chart 1: Strength Progress (per exercise)
      ✓ X-axis: dates
      ✓ Y-axis: weight/max weight used
      ✓ Line graph showing progression
      ✓ Show weekly average
      ✓ Trend line (upward/downward)
      ✓ Allow filtering by time period (1m, 3m, 6m, 1y)
    
    Chart 2: Total Volume (weekly)
      ✓ Bar graph: total weight × reps per week
      ✓ Compare week-to-week
      ✓ Show average volume
    
    Chart 3: Workout Consistency
      ✓ Calendar heatmap
      ✓ Each day shows completed/missed/partial
      ✓ Streak counter (consecutive days)
      ✓ Weekly goal progress
    
    Chart 4: Form Quality Trend
      ✓ Line graph: average form score over time
      ✓ Show best and worst performing exercises
      ✓ Identify improvements
    
    Chart 5: Time Per Exercise
      ✓ Bar or pie chart: time spent per muscle group
      ✓ Per-exercise breakdown

  Scenario: Compare performance periods
    Given user has data from at least 2 different months
    When I click "Compare Periods"
    Then I can:
      ✓ Select 2 date ranges to compare
      ✓ View side-by-side metrics:
        - Total volume
        - Avg weight per exercise
        - Consistency (days trained)
        - Form improvement
        - Calories burned
      ✓ Show % change between periods
      ✓ Identify which exercises improved most

  Scenario: Receive progress insights
    Given user has 2+ weeks of consistent data
    When I open "Insights" section
    Then show AI-generated recommendations:
      ✓ "Your deadlift improved 10% this month!"
      ✓ "You've been most consistent on Mondays and Thursdays"
      ✓ "Your squat form has improved significantly"
      ✓ "Consider increasing weight on bench press (easy last session)"
      ✓ "You're missing Wednesday workouts - want a reminder?"
    
    Each insight should include:
      ✓ Clear statement of observation
      ✓ Supporting data/metrics
      ✓ Optional: suggested action

---

### **INTENT 4: Usuario Quiere Gestionar Múltiples Rutinas**

```gherkin
Feature: Gestión de Múltiples Rutinas
  As a intermediate user
  I want to create and manage multiple routines for different goals
  So that I can switch between them based on my training phase

  Background:
    Given user has completed initial setup
    And at least one routine exists

  Scenario: Create multiple routines for different goals
    Given I'm on the "Routines" home screen
    When I click "+ New Routine"
    Then I should:
      ✓ See option to: "Quick Create" or "Detailed Setup"
      ✓ For Quick Create: re-answer questionnaire (new routine)
      ✓ For Detailed Setup: manual exercise selection with full control
    
    When I create "Strength Phase" routine (goal: strength, 5 days/week)
    Then system should suggest:
      ✓ Compound movements (Squats, Deadlifts, Bench Press)
      ✓ Lower volume, higher intensity
      ✓ Longer rest periods (3-5 min)
    
    When I create "Hypertrophy Phase" routine (goal: muscle gain, 4 days/week)
    Then system should suggest:
      ✓ Compound + isolation exercises
      ✓ Moderate volume, moderate intensity
      ✓ Medium rest periods (60-90 sec)
    
    When I create "Conditioning Phase" routine (goal: fat loss, 3 days/week)
    Then system should suggest:
      ✓ Compound movements + cardio
      ✓ Moderate volume, higher reps
      ✓ Shorter rest periods (30-45 sec)

  Scenario: Switch between active routines
    Given I have 3 routines created
    When I go to "Routines" overview
    Then I should see:
      ✓ List of all routines with thumbnails
      ✓ Current active routine (highlighted)
      ✓ Each routine shows: name, weekly schedule, exercise count, goal
      ✓ Quick start buttons
    
    When I click on a different routine's "Make Active" button
    Then:
      ✓ Confirm dialog: "Switch to Hypertrophy Phase?"
      ✓ Upon confirmation, update active routine
      ✓ Next scheduled workout uses new routine
      ✓ Show success message

  Scenario: Archive completed routines
    Given a routine has been used for 8+ weeks
    When I click menu on routine → "Archive"
    Then:
      ✓ Move routine out of active list
      ✓ Keep all historical data
      ✓ Allow easy recall (restore from archive)
      ✓ Show "No longer active" badge

---

### **INTENT 5: Usuario Quiere Configurar Entrenamientos Personalizados**

```gherkin
Feature: Configuración Avanzada de Entrenamientos
  As an advanced user
  I want to manually configure exercise parameters and create custom routines
  So that I have full control over my training

  Background:
    Given user has intermediate or advanced experience level
    And user has permission to access advanced settings

  Scenario: Manually configure exercise parameters
    Given I'm editing an exercise in a routine
    When I click "Advanced Settings"
    Then I should be able to modify:
      | Parameter              | Current | Range         | Unit    |
      | Sets                   | 3       | 1-6           | sets    |
      | Reps                   | 8       | 1-30          | reps    |
      | Rest Between Sets      | 120     | 30-300        | seconds |
      | Rest After Exercise    | 60      | 30-180        | seconds |
      | Tempo (Eccentric)      | 3       | 1-5           | seconds |
      | Tempo (Concentric)     | 1       | 1-5           | seconds |
      | Weight Increase Plan   | +2.5kg  | +/-0.5-5      | kg/lbs  |
      | Exercise Notes         | (empty) | Custom text   | text    |
    
    When I save changes
    Then:
      ✓ Validate parameter combinations (no invalid combos)
      ✓ Update exercise with new values
      ✓ Show preview of updated routine
      ✓ Ask: "Apply to future workouts?"

  Scenario: Add exercise alternatives / deload options
    Given I'm configuring a routine
    When I click "Set Alternatives" on an exercise
    Then I can:
      ✓ Select 2-3 alternative exercises
      ✓ Choose which one is default
      ✓ Other alternatives available on demand
    
    When I mark an exercise as "Deload Variant"
    Then during deload weeks:
      ✓ Automatically switch to lighter variant
      ✓ Reduce weight by 30-40%
      ✓ Or reduce reps by 2-3

  Scenario: Create weekly periodization
    Given I'm on the routine detail page
    When I click "Periodization"
    Then I can:
      ✓ Define 4-12 week cycles
      ✓ Each week assigns a training phase:
        - Strength (Low Reps, High Weight)
        - Hypertrophy (Medium Reps/Weight)
        - Endurance (High Reps, Low Weight)
        - Deload (60-70% normal volume)
      ✓ System adjusts sets/reps/weight automatically
      ✓ Show cycle timeline visualization
    
    When week 4 is a deload week
    Then:
      ✓ All exercises reduced by 30-40% volume
      ✓ Rest periods shortened
      ✓ User is notified: "Deload week - focus on recovery"

---

## 📊 MATRIZ DE TRAZABILIDAD DE INTENTS

| Intent ID | Intent Name | Priority | UI Component | Database Tables | External APIs |
|-----------|-------------|----------|--------------|-----------------|----------------|
| INT-1 | Create Personalized Routine | HIGH | QuestionnaireScreen, RoutineDetailScreen | routines, exercises, user_profiles | None |
| INT-2 | Real-time Posture Validation | HIGH | CameraScreen, FormAnalysisScreen | workout_sessions, rep_records | MediaPipe, ML Kit |
| INT-3 | Track Progress & Analytics | HIGH | ProgressScreen, ChartsScreen, HistoryScreen | workout_history, rep_records, analytics | None |
| INT-4 | Manage Multiple Routines | MEDIUM | RoutinesListScreen, ArchiveScreen | routines, routine_history | None |
| INT-5 | Advanced Configuration | MEDIUM | AdvancedSettingsScreen, PeriodizationScreen | exercise_config, periodization_plans | None |

---

## 🔄 FLUJO DE USUARIO PRINCIPAL

```
App Open
    ↓
[First Time?] 
    ├─→ YES → INT-1: Questionnaire → Personalized Routine → Schedule
    └─→ NO  → Home Screen
                ↓
        [Ready to Workout?]
            ├─→ YES → INT-2: Start Exercise
            │         ├─ Enable Camera for Posture Check
            │         ├─ Record Sets/Reps/Weight
            │         └─ Log Form Score
            │             ↓
            │         [Workout Complete?]
            │             └─→ YES → INT-3: Show Session Summary
            │
            └─→ NO  → INT-3: View Progress
                     ├─ Charts & Analytics
                     ├─ PRs & History
                     └─ [Want to manage routines?]
                         └─→ INT-4: Manage Routines
                         └─→ INT-5: Advanced Config
```

---

## ✅ CRITERIOS DE ÉXITO GLOBALES (ACCEPTANCE CRITERIA)

| Criterio | Métrica | Target |
|----------|---------|--------|
| Generación de rutina | Tiempo de respuesta | < 5 segundos |
| Detección de postura | Latencia de feedback | < 500ms |
| Precisión de forma | Reps detectados correctamente | ≥ 85% accuracy |
| Cobertura de ejercicios | Ejercicios en base de datos | ≥ 100 exercises |
| Remanso de datos | Sesiones almacenadas | 100% de intentos |
| Satisfacción de usuario | App Rating | ≥ 4.0/5.0 |
| Retención | Usuarios activos 30 días | ≥ 60% |

---

## 🚀 PRÓXIMOS PASOS

1. **Validar Intents con stakeholders** (usuario final, si aplica)
2. **Crear User Stories basadas en cada Intent**
3. **Diseñar Wireframes/Mockups** para cada pantalla
4. **Especificar modelos de datos** (ERD)
5. **Comenzar desarrollo con tests (TDD)**

