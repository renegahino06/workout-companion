# 📚 Índice de Documentación

Bienvenido a la documentación de **Workout Companion**. Aquí encontrarás toda la información sobre el proyecto.

## 🚀 Comenzar

- **[README.md](../README.md)** - Descripción general del proyecto
- **[CONTRIBUTING.md](../CONTRIBUTING.md)** - Guía para contribuidores
- **[Quick Start](quick-start.md)** - Setup inicial (próximamente)

## 📋 Especificaciones del Proyecto

### Intent Driven Development (IDD)

- **[Definición de Intents](intents_definition.md)** ✅
  - 5 Intents principales
  - Escenarios Gherkin
  - Criterios de aceptación detallados
  - Matriz de trazabilidad

### User Stories

- **[User Stories](user_stories.md)** ✅
  - 17 User Stories completas
  - Formato Agile (As a... I want... So that...)
  - Acceptance Criteria por US
  - Effort estimado (Fibonacci)
  - Sprint asignado
  - Dependencias entre US

## 🏗️ Arquitectura y Diseño

### Arquitectura Técnica

- **[Arquitectura](architecture/architecture.md)** ⏳
  - Patrón MVVM + Clean Architecture
  - Capas: UI, Domain, Data
  - Componentes principales
  - Dependency Injection

### Modelos de Datos

- **[Modelo de Datos](architecture/data-model.md)** ⏳
  - Esquema de base de datos
  - Entidades (Users, Routines, Exercises, Sessions)
  - Relaciones
  - Migraciones

### Diseño de UI/UX

- **[Wireframes](design/wireframes.md)** ⏳
  - Pantallas principales
  - Flujos de usuario
  - Componentes reutilizables
  - Mockups

## 📊 Planificación

- **[Roadmap](../project-plan/roadmap.md)** ✅
  - 12 Sprints (12-15 semanas)
  - 8 Milestones principales
  - Capacidad de sprints
  - KPIs de éxito
  - Timeline detallado

- **[Sprints](../project-plan/sprints/)** ⏳
  - Sprint Backlog
  - Daily Standups
  - Retrospectives

- **[Milestones](../project-plan/milestones.md)** ⏳
  - M1: Setup Proyecto
  - M2: MVP Rutinas
  - M3-M8: Features progresivas

## 🧪 Testing

- **[Test Strategy](testing/strategy.md)** ⏳
  - Unit tests
  - Integration tests
  - E2E tests
  - Performance tests

- **[Test Cases](testing/test-cases.md)** ⏳
  - Por User Story
  - Casos positivos y negativos

## 🔧 Configuración Técnica

### Dependencias

```gradle
// Android Core
implementation 'androidx.core:core:1.10.1'
implementation 'androidx.appcompat:appcompat:1.6.1'

// Jetpack Compose
implementation 'androidx.compose.ui:ui:1.6.0'
implementation 'androidx.compose.material3:material3:1.1.1'

// MVVM & State Management
implementation 'androidx.lifecycle:lifecycle-viewmodel-compose:2.6.1'
implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.7.3'

// Database
implementation 'androidx.room:room-runtime:2.5.2'
kapt 'androidx.room:room-compiler:2.5.2'

// Camera & Vision
implementation 'androidx.camera:camera-core:1.2.3'
implementation 'androidx.camera:camera-camera2:1.2.3'
implementation 'androidx.camera:camera-lifecycle:1.2.3'
implementation 'androidx.camera:camera-view:1.2.3'

// MediaPipe
implementation 'com.google.mediapipe:mediapipe-tasks-vision:0.20230731'

// Networking (future)
implementation 'com.squareup.retrofit2:retrofit:2.9.0'
implementation 'com.squareup.okhttp3:okhttp:4.11.0'

// JSON Serialization
implementation 'com.google.code.gson:gson:2.10.1'

// Logging
implementation 'com.jakewharton.timber:timber:5.0.1'

// Testing
testImplementation 'junit:junit:4.13.2'
testImplementation 'org.mockito:mockito-core:5.4.1'
testImplementation 'org.mockito.kotlin:mockito-kotlin:5.1.0'
androidTestImplementation 'androidx.test.espresso:espresso-core:3.5.1'
androidTestImplementation 'androidx.compose.ui:ui-test-junit4:1.6.0'

// Code Quality
implementation 'com.pinterest.ktlint:ktlint-core:1.0.1'
```

### Build Configuration

- `build.gradle.kts` - Build script
- `gradle.properties` - Propiedades de Gradle
- `settings.gradle.kts` - Configuración de módulos

## 🎯 Key Concepts

### Intent Driven Development (IDD)

```
Intent → User Goal → Feature → Implementation → Tests → Validation
```

Cada feature se desarrolla alrededor de lo que el usuario QUIERE lograr (Intent).

### MVVM Architecture

```
UI Layer (Composables)
       ↓
ViewModel (State Management)
       ↓
Repository (Data Abstraction)
       ↓
Data Sources (Room DB, API)
```

### Clean Architecture

```
Presentation Layer (UI + ViewModel)
       ↓
Domain Layer (Use Cases + Entities)
       ↓
Data Layer (Repositories + Sources)
```

## 📱 Funcionalidades Principales

### 1. Rutinas Personalizadas

- Cuestionario de perfil (goal, level, equipment, etc)
- Generador de rutinas automático (<5s)
- Visualización de ejercicios con detalles
- Edición y customización
- Búsqueda de alternativas

**US:** US-101 a US-106  
**Sprint:** 1-2

### 2. Validación de Postura

- Integración MediaPipe Pose
- Detección en tiempo real (30+ FPS)
- Validaciones por ejercicio (Squat, Bench Press, Deadlift, etc)
- Feedback visual (green/red)
- Rep counting automático
- Form scoring (0-100%)

**US:** US-201 a US-206  
**Sprint:** 3-4

### 3. Rastreo de Progreso

- Registro de sesiones (sets/reps/peso/form)
- Historial de entrenamientos
- Personal Records (PRs)
- Gráficos avanzados (5 tipos)
- Insights inteligentes
- Exportar datos

**US:** US-301 a US-305  
**Sprint:** 5

### 4. Gestión Avanzada

- Múltiples rutinas
- Switching entre rutinas
- Periodización (4-12 semanas)
- Deload weeks automáticas
- Configuración por ejercicio

**Sprint:** 6

## 🔗 Enlaces Útiles

**Android & Jetpack:**
- [Android Developers](https://developer.android.com)
- [Jetpack Compose](https://developer.android.com/jetpack/compose)
- [Architecture Components](https://developer.android.com/topic/architecture)

**MediaPipe:**
- [MediaPipe Tasks](https://developers.google.com/mediapipe/solutions/vision/pose_detector)
- [ML Kit Vision](https://developers.google.com/ml-kit/vision)

**Kotlin:**
- [Kotlin Official](https://kotlinlang.org)
- [Coroutines](https://kotlinlang.org/docs/coroutines-overview.html)

**Testing:**
- [JUnit 4](https://junit.org/junit4/)
- [Mockito](https://github.com/mockito/mockito)
- [Espresso](https://developer.android.com/training/testing/espresso)

## 📞 Soporte

**Problemas o preguntas?**
- 📌 [Issues](https://github.com/renegahino06/workout-companion/issues)
- 💬 [Discussions](https://github.com/renegahino06/workout-companion/discussions)
- 📧 contacto@example.com (actualizar)

## 📊 Estado de Documentación

| Documento | Status | Responsable |
|-----------|--------|------------|
| Intents | ✅ Completo | @renegahino06 |
| User Stories | ✅ Completo | @renegahino06 |
| Roadmap | ✅ Completo | @renegahino06 |
| Arquitectura | ⏳ Próximo | @team |
| Data Model | ⏳ Próximo | @team |
| Wireframes | ⏳ Próximo | @team |
| Test Strategy | ⏳ Próximo | @team |
| API Specs | ⏳ Futuro | @team |

---

**Última actualización:** 2026-11-23  
**Próxima revisión:** 2026-11-30

