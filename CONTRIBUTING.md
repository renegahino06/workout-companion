# 🤝 Contribuir a Workout Companion

¡Gracias por tu interés en contribuir! Este documento describe cómo participar en el desarrollo.

## 📋 Código de Conducta

Todos los participantes deben seguir un comportamiento respetuoso y profesional.

## 🏗️ Proceso de Contribución

### 1. Fork & Branch

```bash
# Fork el repositorio en GitHub
# Clonar tu fork
git clone https://github.com/YOUR_USERNAME/workout-companion.git
cd workout-companion

# Crear rama para tu feature
git checkout -b feature/nombre-descriptivo
# O para bugs:
git checkout -b bugfix/descripcion-bug
# O para docs:
git checkout -b docs/descripcion-cambio
```

### 2. Convenciones de Naming

**Branches:**
- `feature/user-authentication` - Nueva feature
- `bugfix/camera-crash` - Bug fix
- `docs/setup-instructions` - Documentación
- `test/posture-detection` - Tests

**Commits:**
```
[TYPE] descripción breve (máx 50 caracteres)

descripción más detallada si es necesario (máx 72 caracteres por línea)

Fixes #123
Co-authored-by: Nombre <email>
```

Tipos:
- `feat:` - Nueva feature
- `fix:` - Bug fix
- `docs:` - Documentación
- `style:` - Formato, no cambios lógicos
- `refactor:` - Refactoring de código
- `test:` - Tests nuevos o mejorados
- `chore:` - Cambios de build, deps, etc

### 3. Configurar Desarrollo Local

```bash
# Instalar dependencias
./gradlew build

# Ejecutar app
./gradlew installDebug

# Tests
./gradlew test
./gradlew connectedAndroidTest
```

### 4. Código & Estilo

**Kotlin Style Guide:**
- Seguir [Kotlin Official Style Guide](https://kotlinlang.org/docs/coding-conventions.html)
- Usar `ktlint` para formateo
- Máximo 120 caracteres por línea

```bash
# Formatear código
./gradlew ktlintFormat

# Validar
./gradlew ktlintCheck
```

**Estructura de Archivos:**
```
src/main/java/com/fitness/companion/
├── ui/              # Composables & Screens
├── viewmodel/       # ViewModels
├── domain/          # Use Cases & Business Logic
├── data/            # Repositories, DAOs
│   ├── local/       # Room Database
│   └── remote/      # API (future)
└── utils/           # Helpers, Extensions
```

### 5. Testing Requirements

**Minimum Coverage:** 80%

```kotlin
// Tests deben cubrir:
// 1. Happy path (caso normal)
// 2. Edge cases (límites)
// 3. Error cases (manejo de errores)
// 4. Null safety (nullability)

@Test
fun `when valid input, then returns expected output`() {
    // Arrange
    val input = "test"
    
    // Act
    val result = function(input)
    
    // Assert
    assertThat(result).isEqualTo("expected")
}
```

### 6. Pull Request Process

1. **Push a tu rama:**
   ```bash
   git push origin feature/nombre-feature
   ```

2. **Crear Pull Request:**
   - Título descriptivo
   - Referencia a issue: `Fixes #123`
   - Descripción detallada de cambios
   - Screenshots si es UI
   - Checklist:
     ```markdown
     - [ ] Tests escritos y pasando
     - [ ] Code review completado
     - [ ] Documentación actualizada
     - [ ] No breaking changes
     - [ ] Seguí style guide
     ```

3. **Esperar Review:**
   - Mínimo 1 aprobación
   - Resolver comentarios
   - Mantener branch actualizado

4. **Merge:**
   - Preferir "Squash and Merge" para features pequeñas
   - "Create a merge commit" para features grandes

### 7. Reportar Bugs

**Crear Issue con template:**
```markdown
## 🐛 Descripción del Bug
Descripción clara de qué está mal

## 🔄 Pasos para Reproducir
1. Paso 1
2. Paso 2
3. Paso 3

## ✅ Comportamiento Esperado
Qué debería pasar

## ❌ Comportamiento Actual
Qué pasa realmente

## 📱 Ambiente
- Device: Pixel 6, Samsung S21
- OS: Android 13, Android 14
- App version: 0.1.0
- Build: debug, release

## 🖼️ Screenshots/Videos
(si aplica)
```

### 8. Sugerir Features

**Crear Issue:**
```markdown
## ✨ Descripción de Feature
Descripción clara de la idea

## 🎯 Problema que Resuelve
Por qué es necesario

## 💡 Solución Propuesta
Cómo debería funcionar

## 🔄 Alternativas Consideradas
Otras formas de resolver

## 📋 Criterios de Aceptación
- [ ] Criterio 1
- [ ] Criterio 2
```

## 📚 Documentación

**Mantener actualizado:**
- README.md si cambias setup
- docs/ si cambias arquitectura o design
- CHANGELOG.md si es release significativo
- Código comentado solo cuando necesario

## 🧪 Development Workflow

```bash
# 1. Crear rama
git checkout -b feature/posture-validation

# 2. Hacer cambios
# - Escribir tests primero (TDD)
# - Implementar feature
# - Validar tests pasan
# - Formatear código

# 3. Validaciones
./gradlew ktlintFormat  # Formatear
./gradlew test          # Tests unitarios
./gradlew lint          # Android Lint

# 4. Commit
git add .
git commit -m "feat: add posture validation for squats"

# 5. Push
git push origin feature/posture-validation

# 6. Crear PR en GitHub
```

## 🎓 Aprendiendo el Código

**Empezar por:**
1. Leer [Architecture docs](../docs/architecture/)
2. Ver [User Stories](../docs/user_stories.md)
3. Revisar ejemplos de código existente
4. Hacer preguntas en Discussions

**Ayuda:**
- 📖 [Kotlin Docs](https://kotlinlang.org/docs)
- 🏗️ [Android Architecture](https://developer.android.com/topic/architecture)
- 🧩 [Jetpack Compose](https://developer.android.com/jetpack/compose)
- 📱 [CameraX](https://developer.android.com/training/camerax)

## 🚀 Release Checklist

Antes de release, verificar:
- [ ] Todos tests pasan
- [ ] Code coverage ≥90%
- [ ] Zero critical bugs
- [ ] Performance optimizado
- [ ] Documentación actualizada
- [ ] Version bumped
- [ ] Changelog actualizado

## 📊 Project Status

Ver [Roadmap](../project-plan/roadmap.md) para estado actual y próximos sprints.

---

**¡Gracias por contribuir! 🙌**

Questions? Open an [issue](https://github.com/renegahino06/workout-companion/issues)

