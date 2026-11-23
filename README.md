# 💪 Workout Companion - App de Entrenamientos Personalizados

![Android](https://img.shields.io/badge/Android-Platform-blue)
![Kotlin](https://img.shields.io/badge/Kotlin-Language-purple)
![IDD](https://img.shields.io/badge/IDD-Methodology-green)
![Status](https://img.shields.io/badge/Status-Early%20Development-orange)

## 📋 Descripción

**Workout Companion** es una aplicación Android que permite a usuarios crear **rutinas de fuerza personalizadas** y recibir **retroalimentación en tiempo real sobre su postura** utilizando detección de poses mediante inteligencia artificial.

La aplicación está diseñada con la metodología **Intent Driven Development (IDD)** para garantizar que cada funcionalidad esté alineada con las necesidades reales del usuario.

## 🎯 Características Principales

### ✅ Implementado / En Desarrollo
- [ ] Generación de rutinas personalizadas
- [ ] Detección y corrección de postura en tiempo real (MediaPipe)
- [ ] Rastreo de progreso y análisis
- [ ] Gestión de múltiples rutinas
- [ ] Configuración avanzada de entrenamientos
- [ ] Historial detallado de sesiones

## 🏗️ Arquitectura del Proyecto

```
workout-companion/
├── app/                          # Código de la aplicación Android
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/com/fitnes/companion/
│   │   │   │   ├── ui/                # Pantallas (Composables)
│   │   │   │   ├── viewmodel/         # ViewModels (MVVM)
│   │   │   │   ├── domain/            # Lógica de negocio
│   │   │   │   ├── data/              # Repositorios y BD
│   │   │   │   └── utils/
│   │   │   ├── res/                   # Recursos (layouts, strings, etc)
│   │   │   └── AndroidManifest.xml
│   │   └── test/                      # Tests unitarios
│   └── build.gradle.kts
├── docs/                         # Documentación del proyecto
│   ├── intents/                  # Definición de Intents
│   ├── user-stories/             # User Stories
│   ├── design/                   # Wireframes y diseño
│   ├── architecture/             # Arquitectura técnica
│   └── api/                      # Especificaciones de API
├── tests/                        # Tests de integración y E2E
├── project-plan/                 # Planificación del proyecto
│   ├── roadmap.md
│   ├── sprints/
│   └── milestones.md
├── .gitignore
├── .github/                      # Workflows y configuración de GitHub
│   └── workflows/
├── README.md                     # Este archivo
└── CONTRIBUTING.md              # Guía de contribución

```

## 🚀 Comenzar Rápidamente

### Requisitos Previos
- Android Studio (Flamingo+)
- Kotlin 1.9+
- JDK 11+
- Mínimo SDK: Android 8.0 (API 26)
- Target SDK: Android 14+ (API 34+)

### Instalación

```bash
# 1. Clonar el repositorio
git clone https://github.com/renegahino06/workout-companion.git
cd workout-companion

# 2. Abrir en Android Studio
# File → Open → Seleccionar la carpeta del proyecto

# 3. Sincronizar Gradle (se hace automáticamente)

# 4. Ejecutar la app
# Run → Run 'app' (o presionar Shift+F10)
```

## 📚 Documentación

La documentación completa está en la carpeta `docs/`:

- **[Definición de Intents](docs/intents/intents_definition.md)** - Especificación de requisitos basada en IDD
- **[User Stories](docs/user-stories/user_stories.md)** - Historias de usuario detalladas
- **[Modelos de Datos](docs/architecture/data-model.md)** - Esquema de base de datos
- **[Arquitectura](docs/architecture/architecture.md)** - Patrón MVVM y Clean Architecture
- **[Wireframes](docs/design/wireframes.md)** - Diseño de UI/UX
- **[Roadmap](project-plan/roadmap.md)** - Planificación de fases

## 🏃 Metodología: Intent Driven Development

Este proyecto implementa **IDD (Intent Driven Development)**, una metodología que:

1. **Define Intents** - ¿Qué quiere lograr el usuario?
2. **Especifica Escenarios** - ¿Cómo veremos el éxito?
3. **Escribe Tests** - Basados en criterios de aceptación
4. **Implementa Features** - Solo lo necesario para pasar los tests

### Fases del Proyecto

| Fase | Sprint | Objetivo | Estado |
|------|--------|----------|--------|
| 1 | 1-2 | Setup Android + Modelos de datos | ⏳ Próximo |
| 2 | 3-4 | Generación de rutinas personalizadas | ⏳ Próximo |
| 3 | 5-6 | Integración cámara + MediaPipe | ⏳ Próximo |
| 4 | 7-8 | Validación de postura | ⏳ Próximo |
| 5 | 9-10 | Analytics y progreso | ⏳ Próximo |
| 6 | 11-12 | UI pulida + Testing completo | ⏳ Próximo |

## 🛠️ Stack Tecnológico

### Frontend
- **Jetpack Compose** - UI moderna y declarativa
- **Material Design 3** - Componentes de UI
- **Navigation Compose** - Navegación entre pantallas

### Business Logic
- **ViewModel (MVVM)** - Gestión de estado
- **LiveData/StateFlow** - Reactive programming
- **Coroutines** - Async operations

### Data & Storage
- **Room Database** - Base de datos local SQLite
- **DataStore** - Preferencias del usuario
- **Repositories** - Abstracción de datos

### AI & Vision
- **MediaPipe Pose** - Detección de poses en tiempo real
- **ML Kit** - APIs de machine learning de Google
- **CameraX** - API moderna de cámara

### Testing
- **JUnit 4** - Tests unitarios
- **Mockito** - Mocking
- **Espresso** - Tests de UI

## 📦 Dependencias Principales

```gradle
// Android
implementation 'androidx.core:core:1.10.1'
implementation 'androidx.appcompat:appcompat:1.6.1'

// Jetpack Compose
implementation 'androidx.compose.ui:ui:1.6.0'
implementation 'androidx.compose.material3:material3:1.1.1'

// MVVM & Coroutines
implementation 'androidx.lifecycle:lifecycle-viewmodel-compose:2.6.1'
implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.7.3'

// Room
implementation 'androidx.room:room-runtime:2.5.2'

// Camera & MediaPipe
implementation 'androidx.camera:camera-core:1.2.3'
implementation 'com.google.mediapipe:mediapipe-tasks-vision:0.20230731'

// Testing
testImplementation 'junit:junit:4.13.2'
testImplementation 'org.mockito:mockito-core:5.4.1'
androidTestImplementation 'androidx.test.espresso:espresso-core:3.5.1'
```

## 🧪 Testing

Ejecutar tests:

```bash
# Tests unitarios
./gradlew test

# Tests de instrumentación (UI)
./gradlew connectedAndroidTest

# Tests específicos
./gradlew test --tests "*RoutineGeneratorTest"
```

## 👥 Contribución

¿Quieres contribuir? Ver [CONTRIBUTING.md](CONTRIBUTING.md)

## 📄 Licencia

Este proyecto está bajo licencia **MIT** - ver [LICENSE](LICENSE) para detalles.

## 📞 Contacto

**Autor:** [@renegahino06](https://github.com/renegahino06)  
**Email:** contacto@example.com (actualizar si aplica)  
**Issues:** [Reportar problema](https://github.com/renegahino06/workout-companion/issues)

---

## 📊 Progreso del Proyecto

```
Sprint 1: Setup Inicial                     ████░░░░░░  40%
Sprint 2: Rutinas Personalizadas           ░░░░░░░░░░   0%
Sprint 3: Integración Cámara               ░░░░░░░░░░   0%
Sprint 4: Validación de Postura            ░░░░░░░░░░   0%
Sprint 5: Analytics                        ░░░░░░░░░░   0%
Sprint 6: Polish & Testing                 ░░░░░░░░░░   0%
```

**Última actualización:** 2026-11-23  
**Próxima revisión:** 2026-11-30

