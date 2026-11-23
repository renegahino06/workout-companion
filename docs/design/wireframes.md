# 📐 Wireframes - Workout Companion

**Versión:** 1.0  
**Fecha:** 2026-11-23  
**Formato:** Wireframe (ASCII + descripción)  
**Próximo paso:** Figma Design

---

## 🎯 Visión General de Pantallas

Workout Companion tiene **8 pantallas principales**:

1. ✅ Splash/Onboarding
2. ✅ Questionnaire (Perfil)
3. ✅ Routine Detail (Ver rutina)
4. ✅ Camera/Postura
5. ✅ Exercise Detail
6. ✅ Workout Summary
7. ✅ Progress Analytics
8. ✅ Home Dashboard

---

## 📄 PANTALLA 1: SPLASH / ONBOARDING

Pantalla inicial (2-3 segundos)

```
╔═══════════════════════════════╗
║                               ║
║          SPLASH               ║
║                               ║
║      💪 WORKOUT               ║
║      COMPANION                ║
║                               ║
║   Loading indicator           ║
║      ⟳                        ║
║                               ║
║   "Preparing your workout"    ║
║                               ║
╚═══════════════════════════════╝
```

**Componentes:**
- Logo + App Name (48pt, centered)
- Loading spinner (animated)
- Status text (12pt, gray)

**Flow:** Splash → Check perfil → Questionnaire O Home

---

## 📋 PANTALLA 2: QUESTIONNAIRE (Creación de Perfil)

Formulario 7 screens con preguntas

```
╔═══════════════════════════════╗
║  ◀️  PERSONALIZE YOUR          ║
║      ROUTINE                  ║
├─────────────────────────────── ┤
║                               ║
║  Question 1 / 7               ║
║  ▯▯▯▯▯▯▯▯▯▯▯▯░░░░           ║  Progress bar
║                               ║
║  What's your main goal?       ║  Pregunta
║                               ║
║  ⊕ Muscle Gain                ║  Option 1
║  ⊕ Fat Loss                   ║  Option 2
║  ⊕ Strength                   ║  Option 3
║  ⊕ Endurance                  ║  Option 4
║                               ║
├─────────────────────────────── ┤
║  BACK          NEXT →         ║
╚═══════════════════════════════╝
```

**Preguntas por orden:**
1. Training Goal (radio)
2. Experience Level (radio)
3. Available Equipment (checkboxes)
4. Training Frequency (radio)
5. Session Duration (radio)
6. Body Parts Priority (checkboxes)
7. Injuries/Limitations (text)

**Validación:** Next deshabilitado si campos vacíos

---

## 🎯 PANTALLA 3: ROUTINE DETAIL (Ver Rutina Generada)

Muestra rutina con lista de ejercicios

```
╔═══════════════════════════════╗
║  ◀️  My Routine               ║
├─────────────────────────────── ┤
║                               ║
║  📋 BEGINNER PUSH DAY         ║
║                               ║
║  Monday, Wednesday, Friday     ║  Schedule
║  Est. 50 min per session      ║  Duration
║                               ║
║  Difficulty: ⭐⭐☆            ║
║  Focus: Chest, Shoulders      ║
║                               ║
├─────────────────────────────── ┤
║  EXERCISES (6)                ║
├─────────────────────────────── ┤
║                               ║
║  ┌────────────────────────┐   ║
║  │ 1. Bench Press         │   ║
║  │ 4 sets × 8 reps        │   ║
║  │ Rest: 90s              │   ║
║  │ ⭐⭐⭐ Difficulty        │   ║
║  │ 🔽 More Details        │   ║
║  └────────────────────────┘   ║
║                               ║
║  (más ejercicios...)          ║
║                               ║
├─────────────────────────────── ┤
║  🖊️  EDIT      ✅ SAVE        ║
╚═══════════════════════════════╝
```

**Componentes:**
- Routine info card
- Exercise list (scrollable)
- Edit/Save buttons

---

## 📸 PANTALLA 4: CAMERA / POSTURA

Durante el ejercicio con feedback en vivo

```
╔═══════════════════════════════╗
║  SQUAT - Set 1/3              ║
├─────────────────────────────── ┤
║                               ║
║   📷 CAMERA FEED              ║
║   ┌─────────────────────────┐ ║
║   │                         │ ║
║   │  ◯  ◯  ◯                │ ║  Skeleton
║   │   \ | /                │ ║  (joints)
║   │    \|/                 │ ║
║   │     ◯                  │ ║
║   │    / \                 │ ║
║   │   ◯   ◯                │ ║
║   │                         │ ║
║   │  Confidence: 95%        │ ║
║   │  Status: Ready ✓        │ ║
║   └─────────────────────────┘ ║
║                               ║
║  Rep Counter: 3 / 10          ║
║  Form Quality: 85% ▯▯▯▯▯░░  ║
║                               ║
├─────────────────────────────── ┤
║  🛑 STOP              ⏸️  PAUSE ║
╚═══════════════════════════════╝

Estados:
- GREEN: Forma correcta
- RED: Forma incorrecta
- YELLOW: Detectando
```

**Componentes:**
- Camera preview (full screen)
- Skeleton overlay
- Rep counter
- Form quality bar
- Control buttons

---

## 💪 PANTALLA 5: EXERCISE DETAIL / INSTRUCTIONS

Detalles paso-a-paso del ejercicio

```
╔═══════════════════════════════╗
║  ◀️  BENCH PRESS               ║
├─────────────────────────────── ┤
║                               ║
║  IMAGE / VIDEO                ║
║  ┌─────────────────────────┐ ║
║  │                         │ ║
║  │                         │ ║
║  │                         │ ║
║  └─────────────────────────┘ ║
║                               ║
║  ⭐⭐⭐ Difficulty             ║
║  Chest · Shoulders · Triceps  ║
║                               ║
├─────────────────────────────── ┤
║  INSTRUCTIONS                 ║
├─────────────────────────────── ┤
║  1. Lie flat on bench         ║
║  2. Grab bar slightly wider   ║
║  3. Lower bar to chest        ║
║  4. Press bar up              ║
║                               ║
├─────────────────────────────── ┤
║  COMMON MISTAKES              ║
├─────────────────────────────── ┤
║  ✗ Not using full range       ║
║  ✗ Bouncing bar off chest     ║
║  ✗ Flaring elbows too much    ║
║                               ║
├─────────────────────────────── ┤
║  🎥 Watch Video    ✓ Got it   ║
╚═══════════════════════════════╝
```

**Componentes:**
- Image/Video placeholder
- Difficulty y músculos
- Instructions (numbered)
- Common mistakes
- Alternatives
- Action buttons

---

## 📊 PANTALLA 6: WORKOUT SESSION SUMMARY

Modal después de completar set

```
╔═══════════════════════════════╗
║  ✓ SET COMPLETE!              ║
├─────────────────────────────── ┤
║  BENCH PRESS - Set 1          ║
║                               ║
║  Total Reps: 10               ║
║  ✓ Correct: 8/10 (80%)       ║
║  ✗ Incorrect: 2/10 (20%)     ║
║                               ║
║  Form Score: 80/100           ║
║  ▯▯▯▯▯▯▯▯░░                 ║
║                               ║
├─────────────────────────────── ┤
║  ISSUES DETECTED              ║
├─────────────────────────────── ┤
║  ⚠️  Shallow depth             ║
║     → Go deeper on squat      ║
║                               ║
║  ⚠️  Knees inward              ║
║     → Keep knees parallel     ║
║                               ║
├─────────────────────────────── ┤
║  SAVE RESULTS                 ║
║  Weight Used:  [100] kg       ║
║  Difficulty:   ★★★★☆         ║
║  Notes:        ___________    ║
║                               ║
├─────────────────────────────── ┤
║  NEXT EXERCISE  ⟳  REDO SET   ║
╚═══════════════════════════════╝
```

**Componentes:**
- Success header
- Rep breakdown
- Form score
- Issues found
- Data entry form
- Action buttons

---

## 📈 PANTALLA 7: PROGRESS / ANALYTICS

Gráficos y estadísticas

```
╔═══════════════════════════════╗
║  ◀️  PROGRESS                  ║
├─────────────────────────────── ┤
║  Time: [Last 3 months]        ║
├─────────────────────────────── ┤
║                               ║
║  CHART 1: STRENGTH PROGRESS    ║
║  ┌─────────────────────────┐  ║
║  │                    ╱│   │  ║
║  │              ╱────│────│  ║
║  │          ╱───     │    │  ║
║  │      ╱───         │    │  ║
║  │  ╱───             │    │  ║
║  │ ├────────────────│────│  ║
║  │ W1 W2 W3 W4 W5 W6│W7  │  ║
║  └─────────────────────────┘  ║
║  Bench Press: +10 kg (↑ 10%)   ║
║                               ║
├─────────────────────────────── ┤
║  CHART 2: VOLUME (WEEKLY)     ║
║  ┌─────────────────────────┐  ║
║  │ ▯▯▯                     │  ║
║  │ ▯▯▯ ▯▯▯                 │  ║
║  │ ▯▯▯ ▯▯▯ ▯▯▯             │  ║
║  │ ▯▯▯ ▯▯▯ ▯▯▯ ▯▯▯         │  ║
║  │ ├──┼──┼──┼──┤           │  ║
║  │ W1 W2 W3 W4             │  ║
║  └─────────────────────────┘  ║
║  Avg Volume: 15,250 kg         ║
║                               ║
├─────────────────────────────── ┤
║  PERSONAL RECORDS             ║
║  ✓ NEW! Bench Press: 100kg    ║
║  Deadlift: 150kg              ║
║  Squat: 120kg                 ║
║                               ║
╚═══════════════════════════════╝
```

**Componentes:**
- Time period selector
- Multiple charts (5 tipos)
- Personal records
- Refresh button

---

## 🏠 PANTALLA 8: HOME / DASHBOARD

Pantalla principal

```
╔═══════════════════════════════╗
║  💪 WORKOUT COMPANION          ║
║  Good morning, John! 👋        ║
├─────────────────────────────── ┤
║  TODAY'S WORKOUT              ║
║                               ║
║  ┌────────────────────────┐   ║
║  │ PUSH DAY               │   ║
║  │ 🕐 50 min              │   ║
║  │ 📋 6 exercises         │   ║
║  │ Difficulty: ⭐⭐⭐      │   ║
║  │  [START WORKOUT]       │   ║
║  └────────────────────────┘   ║
║                               ║
├─────────────────────────────── ┤
║  QUICK STATS                  ║
║  🔥 Streak: 7 days            ║
║  📊 This Week: 4/5 workouts   ║
║  💪 Current PR: Squat 150kg    ║
║                               ║
├─────────────────────────────── ┤
║  RECENT ACTIVITY              ║
║  Today                         ║
║  ✓ Upper Body (completed)      ║
║                               ║
║  Yesterday                     ║
║  ✓ Lower Body (completed)      ║
║                               ║
├─────────────────────────────── ┤
║  🏋️  My Routines               ║
║  📊  Progress                  ║
║  ⚙️  Settings                  ║
╚═══════════════════════════════╝
```

**Componentes:**
- Header (greeting)
- Today's workout card
- Quick stats
- Recent activity
- Bottom navigation

---

## 🗺️ NAVEGACIÓN

```
Splash (2s)
    ↓
¿Perfil?
├─ NO → Questionnaire (7) → Home
└─ SÍ → Home
           ├─ START → Camera → Summary → Next
           ├─ My Routines → Detail → Edit
           └─ Progress → Analytics
```

**Bottom Nav Tabs:**
- Home
- My Routines
- Progress
- Settings

---

## 🎨 DESIGN SYSTEM

**Colores:**
- Primary: #6200EE (Purple)
- Secondary: #03DAC6 (Teal)
- Accent: #BB86FC
- Success: #4CAF50 (Green)
- Error: #F44336 (Red)
- Warning: #FFC107 (Yellow)

**Typography:**
- Display: 48pt Bold
- Heading: 32pt Bold
- Title: 20pt Medium
- Body: 16pt Regular
- Caption: 12pt Regular

**Spacing:** 8px, 16px, 24px, 32px

---

**Próximo:** Convertir a Figma para diseño visual

