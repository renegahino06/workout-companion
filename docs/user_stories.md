# 👥 User Stories - Workout Companion

**Basadas en Intent Driven Development**  
**Versión:** 1.0  
**Fecha:** 2026-11-23

---

## 📌 Formato de User Stories

Cada User Story sigue el formato:

```
ID: US-XXX
Título: [Breve descripción]
As a [actor/usuario]
I want to [acción]
So that [beneficio/valor]

Acceptance Criteria:
- [ ] Criterio 1
- [ ] Criterio 2
- [ ] Criterio N

Dependencies:
- Otra US si aplica

Effort: X puntos (1-13 escala Fibonacci)
Priority: HIGH/MEDIUM/LOW
```

---

## 🎯 USER STORIES POR INTENT

### INTENT 1: Rutina Personalizada Automática

---

#### **US-101: Completar cuestionario de perfil de usuario**

**ID:** US-101  
**Título:** Crear perfil de usuario nuevo

**Story:**
```
As a first-time user
I want to answer questions about my fitness profile
So that the app can generate a personalized routine
```

**Acceptance Criteria:**
- [ ] Pantalla de cuestionario abre al primer inicio
- [ ] Formulario contiene 7 campos requeridos:
  - Training Goal (radio buttons)
  - Experience Level (radio buttons)
  - Available Equipment (checkboxes)
  - Training Frequency (radio buttons)
  - Session Duration (radio buttons)
  - Body Parts Priority (checkboxes múltiples)
  - Injuries/Limitations (texto opcional)
- [ ] Validación: todos los campos requeridos están completos antes de continuar
- [ ] Botón "Next" deshabilitado hasta completar campos obligatorios
- [ ] Puedo navegar hacia atrás (Back button)
- [ ] Los datos se guardan localmente en case de cancelación

**Definition of Done:**
- [ ] Code reviewed
- [ ] Unit tests: 100% coverage
- [ ] UI tests pasan en ambos orientaciones
- [ ] Validación funciona correctamente

**Effort:** 5 puntos  
**Priority:** HIGH  
**Sprint:** 1

---

#### **US-102: Generar rutina personalizada automáticamente**

**ID:** US-102  
**Título:** Generar rutina basada en perfil

**Story:**
```
As a user with a completed profile
I want the app to generate a customized routine
So that I can start training immediately
```

**Acceptance Criteria:**
- [ ] API/Algoritmo genera rutina en < 5 segundos
- [ ] Rutina contiene 4-6 ejercicios (basado en duración de sesión)
- [ ] Todos los ejercicios coinciden con equipo disponible seleccionado
- [ ] 60%+ de ejercicios trabajan prioridades del usuario
- [ ] Ejercicios excluidos si coinciden con lesiones
- [ ] Rutina tiene nombre auto-generado (ej: "Beginner Push Day")
- [ ] Se muestra descripción de la rutina
- [ ] Se calcula duración estimada de sesión (±5% de preferencia)
- [ ] Cada ejercicio muestra:
  - Nombre, Series x Reps, Tiempo de descanso
  - Músculos primarios y secundarios
  - Thumbnail de imagen
  - Dificultad (1-10)

**Definition of Done:**
- [ ] Algoritmo probado con 10+ perfiles diferentes
- [ ] Tests de rendimiento: generación < 5s
- [ ] Code reviewed
- [ ] DB schema validado

**Effort:** 8 puntos  
**Priority:** HIGH  
**Sprint:** 2  
**Dependencies:** US-101

---

#### **US-103: Ver detalles completos de ejercicio**

**ID:** US-103  
**Título:** Expandir ejercicio para ver instrucciones completas

**Story:**
```
As a user viewing a generated routine
I want to click on an exercise and see detailed instructions
So that I can understand proper form before performing it
```

**Acceptance Criteria:**
- [ ] Click en ejercicio abre modal/bottom sheet
- [ ] Modal muestra:
  - Nombre del ejercicio
  - Descripción paso-a-paso (3-5 pasos)
  - 3-5 tips de forma correcta
  - 3-5 errores comunes a evitar
  - Variantes alternativas (2-3 opciones)
  - Link a video (si disponible)
  - Equipo necesario
  - Músculos trabajados
- [ ] Puedo cerrar modal con botón X o swipe down
- [ ] Puedo compartir instrucción
- [ ] Letra legible en dispositivos pequeños

**Definition of Done:**
- [ ] Base de datos de ejercicios con descripciones completas
- [ ] UI responsive en phones 5-6.7"
- [ ] Tests de accesibilidad

**Effort:** 5 puntos  
**Priority:** HIGH  
**Sprint:** 2  
**Dependencies:** US-102

---

#### **US-104: Editar rutina generada**

**ID:** US-104  
**Título:** Personalizar rutina generada

**Story:**
```
As a user viewing a generated routine
I want to edit exercises, sets, reps, and rest times
So that I can customize the routine to my preferences
```

**Acceptance Criteria:**
- [ ] Botón "Edit Routine" en página de rutina
- [ ] Puedo cambiar:
  - Nombre de rutina
  - Días de entrenamiento
  - Duración de sesión
  - Ejercicios individuales (editar directamente)
  - Sets/Reps/Rest times
- [ ] Al cambiar un ejercicio, se abre selector de alternativas
- [ ] Validación: no permitir combinaciones inválidas
  - (ej: más de 90 min con 5 ejercicios simples)
- [ ] Preview de rutina actualizado en tiempo real
- [ ] Botón "Save" guarda cambios en BD
- [ ] Confirmación visual: "Routine Updated"
- [ ] Puedo "Cancel" para descartar cambios

**Definition of Done:**
- [ ] Validaciones implementadas
- [ ] Edge cases probados
- [ ] Code reviewed

**Effort:** 8 puntos  
**Priority:** MEDIUM  
**Sprint:** 2  
**Dependencies:** US-102

---

#### **US-105: Ver alternativas de ejercicios**

**ID:** US-105  
**Título:** Reemplazar ejercicio con alternativa

**Story:**
```
As a user editing a routine
I want to see alternative exercises for any exercise
So that I can choose one that I prefer or have equipment for
```

**Acceptance Criteria:**
- [ ] Botón "Show Alternatives" en cada ejercicio
- [ ] Muestra 3-5 alternativas
- [ ] Cada alternativa:
  - Trabaja los mismos músculos
  - Requiere el mismo equipo
  - Mantiene sets/reps/rest sugeridos
  - Muestra dificultad y descripción breve
- [ ] Puedo seleccionar alternativa con 1 click
- [ ] Ejercicio se reemplaza inmediatamente
- [ ] Resto de rutina mantiene su estructura

**Definition of Done:**
- [ ] Base de datos de alternativas por ejercicio
- [ ] Tests: cada ejercicio tiene mínimo 2 alternativas

**Effort:** 5 puntos  
**Priority:** MEDIUM  
**Sprint:** 2  
**Dependencies:** US-102

---

#### **US-106: Guardar y activar rutina**

**ID:** US-106  
**Título:** Guardar rutina personalizada

**Story:**
```
As a user with a customized routine
I want to save it and start using it immediately
So that my next workouts follow this routine
```

**Acceptance Criteria:**
- [ ] Botón "Save Routine" en página de rutina
- [ ] Validación: rutina debe tener mínimo 3 ejercicios
- [ ] Validación: duración total debe estar dentro de rango (20-120 min)
- [ ] Rutina se guarda en BD con:
  - ID único
  - Nombre, descripción
  - Lista de ejercicios con sets/reps/rest
  - Fecha de creación
  - Estado (active/inactive)
- [ ] Se crea schedule semanal por defecto
  - Ej: Lunes, Miércoles, Viernes si es 3x/week
- [ ] Confirmación: "✓ Routine Saved"
- [ ] Redirige a pantalla de rutina activa
- [ ] Próximo workout usa esta rutina

**Definition of Done:**
- [ ] Schema de BD completado
- [ ] Migraciones de BD probadas

**Effort:** 5 puntos  
**Priority:** HIGH  
**Sprint:** 2  
**Dependencies:** US-102, US-104

---

### INTENT 2: Validación de Postura en Tiempo Real

---

#### **US-201: Habilitar cámara para validación de postura**

**ID:** US-201  
**Título:** Abrir cámara para feedback de postura

**Story:**
```
As a user starting an exercise
I want to enable the camera to get real-time posture feedback
So that I can ensure I'm using correct form
```

**Acceptance Criteria:**
- [ ] Botón "Start Exercise" o "Enable Camera" en página de ejercicio
- [ ] Click requiere permisos de cámara:
  - Pedir permisos si no están otorgados
  - Recordar permisos para futuro
- [ ] Pantalla de cámara abre con:
  - Preview live de cámara (back camera por defecto)
  - Indicador "Initializing Pose Detection..."
  - Overlay de esqueleto/joints (deshabilitado hasta listo)
  - Instrucciones: "Get ready to perform the movement"
  - Countdown: 3-2-1
- [ ] Usar cámara en orientación vertical (portrait)
- [ ] Puedo cerrar con botón X (sin perder sesión)
- [ ] Debe funcionar en dispositivos sin iluminación perfecta

**Definition of Done:**
- [ ] MediaPipe integrado
- [ ] Permisos configurados en AndroidManifest
- [ ] Tests en 5+ dispositivos diferentes

**Effort:** 8 puntos  
**Priority:** HIGH  
**Sprint:** 3  
**Dependencies:** US-102

---

#### **US-202: Detectar y visualizar esqueleto del cuerpo**

**ID:** US-202  
**Título:** Mostrar pose skeleton en video en tiempo real

**Story:**
```
As a user with camera enabled
I want to see my body skeleton overlaid on the camera feed
So that I understand what the app is detecting
```

**Acceptance Criteria:**
- [ ] Esqueleto muestra 17 joints principales:
  - Cabeza, hombros, codos, muñecas
  - Caderas, rodillas, tobillos
  - Conexiones entre joints (líneas)
- [ ] Joints dibujados como círculos de colores:
  - Verde: detectado correctamente
  - Amarillo: confianza media (50-70%)
  - Rojo: confianza baja o error
  - Gris: no detectado
- [ ] Líneas conectan joints (skeleton)
- [ ] Confidence score mostrado (0-100%) en esquina
- [ ] Actualización de skeleton: 30+ FPS (suave)
- [ ] Esqueleto se mueve con el usuario en tiempo real
- [ ] Puedo ver indicador "Ready" cuando detección es estable

**Definition of Done:**
- [ ] Rendering optimizado
- [ ] Performance test: 30+ FPS en Snapdragon 870+
- [ ] Tests visuales

**Effort:** 8 puntos  
**Priority:** HIGH  
**Sprint:** 3  
**Dependencies:** US-201

---

#### **US-203: Detectar errores de forma durante ejercicio (Squat)**

**ID:** US-203  
**Título:** Validar forma en sentadillas

**Story:**
```
As a user performing a squat with camera enabled
I want real-time feedback about my form
So that I can correct mistakes immediately
```

**Acceptance Criteria:**
- [ ] Sistema detecta y valida:
  
  | Error | Detection | Feedback |
  |-------|-----------|----------|
  | Knees caving in | Rodillas < 170° o no paralelas | "Keep knees parallel!" |
  | Back rounding | Spine angle > 45° | "Keep back straight!" |
  | Heels off | Ankle position | "Heels on ground!" |
  | Shallow depth | Hip angle < 90° | "Go deeper!" |
  | Asymmetric weight | Shoulder/hip misalignment | "Weight balanced!" |

- [ ] Cuando error detectado:
  - Esqueleto vira ROJO
  - Mensaje de warning en pantalla
  - Joint problemático resaltado (círculo rojo)
  - Vibración feedback (opcional, configurable)
  - Sonido warning (opcional, configurable)
  - Rep registrada como "INCORRECT FORM"
- [ ] Cuando forma correcta:
  - Esqueleto vira VERDE
  - Mensaje: "Perfect Form!"
  - Contador de reps incrementa
  - Rep registrada como "CORRECT FORM"
- [ ] Revert a neutro después de cada rep

**Definition of Done:**
- [ ] Ángulos y thresholds validados con experto en fitness
- [ ] Tests en 10+ usuarios diferentes
- [ ] Precision ≥ 85%

**Effort:** 13 puntos  
**Priority:** HIGH  
**Sprint:** 4  
**Dependencies:** US-202

---

#### **US-204: Rastrear reps completadas con feedback**

**ID:** US-204  
**Título:** Contar reps y mostrar resumen de set

**Story:**
```
As a user completing a set of squats
I want the app to count my reps and show form accuracy
So that I know how many good reps I did
```

**Acceptance Criteria:**
- [ ] Rep counter mostrado en grande (top center)
  - Número actual: "5" (tamaño 48pt)
  - Target: "5/10" (12pt gris)
- [ ] Cada rep completada incrementa contador automáticamente
- [ ] Cuando completo la serie (ej: 10 reps):
  - Pantalla congela
  - Muestra resumen:
    ```
    SET COMPLETE
    ✓ Correct: 7/10 (70%)
    ✗ Incorrect: 2/10 (20%)
    ~ Partial: 1/10 (10%)
    
    Form Score: 70/100
    
    Issues:
    - Knees caving (2 reps)
    - Shallow depth (1 rep)
    ```
  - 3 botones:
    - "Continue" → Próximo set
    - "Redo Set" → Repetir set
    - "Skip" → Próximo ejercicio

**Definition of Done:**
- [ ] Rep detection algorithm validado
- [ ] Edge cases probados (reps rápidas, lentas, pausadas)

**Effort:** 8 puntos  
**Priority:** HIGH  
**Sprint:** 4  
**Dependencies:** US-203

---

#### **US-205: Manejar condiciones de baja iluminación**

**ID:** US-205  
**Título:** Detectar y alertar sobre mala iluminación

**Story:**
```
As a user training in a room with poor lighting
I want the app to tell me
So that I can move to a brighter area
```

**Acceptance Criteria:**
- [ ] Sistema detecta iluminación insuficiente:
  - Confidence score < 50% consistentemente
  - Frame oscuro (histogram analysis)
- [ ] Warning mostrado:
  - Modal: "⚠️ Poor Lighting"
  - Mensaje: "Move to a brighter area"
  - Botón: "Use Flashlight" o "Continue Anyway"
- [ ] Cuando se usa flashlight:
  - Habilita back camera flash
  - Continúa con detección
- [ ] Cuando se "Continue Anyway":
  - Skeleton gris (deshabilitado)
  - Mensaje: "Form feedback disabled"
  - No registra reps con feedback
- [ ] Cuando iluminación mejora:
  - Auto-resume
  - Mensaje: "Lighting improved. Ready."
  - Vuelve a color normal

**Definition of Done:**
- [ ] Iluminación testing en <100 lux, >500 lux
- [ ] Flashlight integration con CameraX

**Effort:** 5 puntos  
**Priority:** MEDIUM  
**Sprint:** 4  
**Dependencies:** US-202

---

#### **US-206: Validaciones específicas por ejercicio (Bench Press)**

**ID:** US-206  
**Título:** Validar forma en press de banca

**Story:**
```
As a user performing a bench press
I want exercise-specific form feedback
So that I can ensure proper shoulder safety
```

**Acceptance Criteria:**
- [ ] Bench Press validaciones:
  
  | Check | Validation | Feedback |
  |-------|-----------|----------|
  | Shoulder Position | 60-100° from body | "Elbows too wide/narrow" |
  | Elbow Depth | Not fully locked (170°) | "Don't lock elbows!" |
  | Back Position | Flat on bench (< 20° arch) | "Back off bench!" |
  | Bar Path | Vertical line over chest | "Bar path: straight up!" |
  | Hand Position | Neutral wrist (180°) | "Keep wrists neutral!" |

- [ ] Rep es "GOOD" solo si TODOS los checks pasan
- [ ] Si hay 1+ error: "POOR FORM"
- [ ] Form score = (checks passed / total checks) × 100

**Definition of Done:**
- [ ] Validaciones definidas para: Deadlift, Push-up, Squat, Bench Press, Pull-up
- [ ] Tests de precisión con video de referencia

**Effort:** 13 puntos  
**Priority:** HIGH  
**Sprint:** 4  
**Dependencies:** US-203

---

### INTENT 3: Rastreo de Progreso y Analytics

---

#### **US-301: Registrar datos de sesión completada**

**ID:** US-301  
**Título:** Guardar sets/reps/peso de ejercicio

**Story:**
```
As a user finishing an exercise
I want to record sets, reps, and weight used
So that my progress is tracked
```

**Acceptance Criteria:**
- [ ] Después de completar ejercicio, formulario muestra:
  - Nombre del ejercicio (read-only)
  - Sets completados (input)
  - Reps por set (input array)
  - Peso usado (input)
  - Form score (auto, de cámara)
  - Difficulty rating (1-10 slider)
  - Notes (texto opcional)
  - Tiempo empleado (auto)
- [ ] Validación:
  - Sets: 1-10
  - Reps: 1-50 cada una
  - Peso: 0.5-500 kg (configurable unidad)
- [ ] Botón "Save Set" guarda en BD con timestamp
- [ ] Muestra: "✓ Saved"
- [ ] Próximo ejercicio o resumen

**Definition of Done:**
- [ ] DB schema validado
- [ ] Migraciones de BD

**Effort:** 5 puntos  
**Priority:** HIGH  
**Sprint:** 5  
**Dependencies:** US-204

---

#### **US-302: Ver historial de entrenamientos**

**ID:** US-302  
**Título:** Revisar entrenamientos pasados

**Story:**
```
As a user who has completed multiple workouts
I want to view my workout history
So that I can track my training over time
```

**Acceptance Criteria:**
- [ ] Pantalla "Workout History" muestra:
  - Calendar view o lista ordenada (date desc)
  - Cada entrada: Date | Routine | Exercises | Duration
  - Completion % (reps completed / planned)
  - Total volume (Σ weight × reps)
  - Avg form score
- [ ] Click en workout abre detalles:
  - Ejercicios realizados
  - Sets/reps/peso para cada uno
  - Form score
  - Notas
  - Duración total
  - Calorías estimadas
- [ ] Filtros:
  - Por rango de fechas
  - Por rutina
  - Por ejercicio específico
- [ ] Exportar como PDF/CSV

**Definition of Done:**
- [ ] Queries optimizadas (<500ms)
- [ ] Tests en 1000+ registros

**Effort:** 8 puntos  
**Priority:** HIGH  
**Sprint:** 5  
**Dependencies:** US-301

---

#### **US-303: Ver registros personales (PRs)**

**ID:** US-303  
**Título:** Mostrar personal records por ejercicio

**Story:**
```
As a user training regularly
I want to see my personal records
So that I can celebrate milestones and set new goals
```

**Acceptance Criteria:**
- [ ] Pantalla "Personal Records" muestra:
  - Lista de ejercicios con:
    - Nombre ejercicio
    - Max weight
    - Date achieved
    - Reps at max weight
  - Ordenado por date (most recent)
  - Badges "NEW PR" si es reciente
- [ ] Click en PR abre histórico:
  - Top 5 intentos anteriores
  - Gráfico: weight vs date
  - Trend analysis
  - Form score en ese día
- [ ] Cuando usuario logra nuevo PR:
  - Notificación inmediata
  - Animation celebratory
  - Option para compartir

**Definition of Done:**
- [ ] PR detection algorithm implementado
- [ ] Notificaciones probadas

**Effort:** 8 puntos  
**Priority:** MEDIUM  
**Sprint:** 5  
**Dependencies:** US-301

---

#### **US-304: Visualizar gráficos de progreso**

**ID:** US-304  
**Título:** Mostrar charts de progreso

**Story:**
```
As a user with weeks of training data
I want to see graphs of my progress
So that I can visualize my improvements
```

**Acceptance Criteria:**
- [ ] Pantalla "Analytics" muestra 5 gráficos:

  **1. Strength Progress (por ejercicio)**
  - X: dates, Y: max weight
  - Line graph con trend line
  - Toggle: 1m, 3m, 6m, 1y
  
  **2. Total Volume (semanal)**
  - Bar chart: Σ(weight × reps) per week
  - Compare week-to-week
  
  **3. Workout Consistency**
  - Calendar heatmap
  - Verde: completed, Gris: missed
  - Streak counter
  
  **4. Form Quality Trend**
  - Line graph: avg form score over time
  - Best/worst exercises
  
  **5. Time per Muscle**
  - Pie/bar chart: time spent per group
  - Breakdown by exercise

- [ ] Interactivo:
  - Tap bar/point para ver detalles
  - Swipe para cambiar period
  - Export imagen

**Definition of Done:**
- [ ] Charts librería integrada (ej: MPAndroidChart)
- [ ] Performance: render <1s
- [ ] Responsive en 5-7"

**Effort:** 13 puntos  
**Priority:** MEDIUM  
**Sprint:** 5  
**Dependencies:** US-301

---

#### **US-305: Recibir insights inteligentes**

**ID:** US-305  
**Título:** AI-generated recommendations

**Story:**
```
As a user with consistent training data
I want to receive insights about my progress
So that I can make informed decisions about my training
```

**Acceptance Criteria:**
- [ ] "Insights" panel muestra:
  - Histórico de observaciones inteligentes
  - Cada insight contiene:
    - Observación clara
    - Datos de soporte (números/gráficos)
    - Acción sugerida (opcional)
  
  Ejemplos:
  - "Your deadlift improved 10% this month!"
  - "Most consistent on Mon & Thu"
  - "Squat form improved significantly"
  - "Consider increasing bench press weight"
  - "Missing Wed workouts - reminder?"
  
- [ ] Insights generados por:
  - Cambios de peso (trend detection)
  - Consistency patterns
  - Form improvements
  - Volume increase
  
- [ ] Frecuencia:
  - 1-2 nuevos insights por semana
  - Push notification opcionalmente

**Definition of Done:**
- [ ] Logic algorithms implementados
- [ ] Edge cases manejados (< 2 semanas data, etc)

**Effort:** 8 puntos  
**Priority:** MEDIUM  
**Sprint:** 5  
**Dependencies:** US-304

---

## 🗂️ MATRIZ DE TRAZABILIDAD

| US | Título | Intent | Priority | Sprint | Effort | Status |
|----|--------|--------|----------|--------|--------|--------|
| US-101 | Cuestionario perfil | INT-1 | HIGH | 1 | 5 | ⏳ |
| US-102 | Generar rutina | INT-1 | HIGH | 2 | 8 | ⏳ |
| US-103 | Detalles de ejercicio | INT-1 | HIGH | 2 | 5 | ⏳ |
| US-104 | Editar rutina | INT-1 | MEDIUM | 2 | 8 | ⏳ |
| US-105 | Alternativas | INT-1 | MEDIUM | 2 | 5 | ⏳ |
| US-106 | Guardar rutina | INT-1 | HIGH | 2 | 5 | ⏳ |
| US-201 | Habilitar cámara | INT-2 | HIGH | 3 | 8 | ⏳ |
| US-202 | Skeleton visual | INT-2 | HIGH | 3 | 8 | ⏳ |
| US-203 | Validar postura Squat | INT-2 | HIGH | 4 | 13 | ⏳ |
| US-204 | Contar reps | INT-2 | HIGH | 4 | 8 | ⏳ |
| US-205 | Iluminación | INT-2 | MEDIUM | 4 | 5 | ⏳ |
| US-206 | Validar Bench Press | INT-2 | HIGH | 4 | 13 | ⏳ |
| US-301 | Registrar sesión | INT-3 | HIGH | 5 | 5 | ⏳ |
| US-302 | Historial entrenamientos | INT-3 | HIGH | 5 | 8 | ⏳ |
| US-303 | Personal Records | INT-3 | MEDIUM | 5 | 8 | ⏳ |
| US-304 | Gráficos progreso | INT-3 | MEDIUM | 5 | 13 | ⏳ |
| US-305 | Insights inteligentes | INT-3 | MEDIUM | 5 | 8 | ⏳ |

**Total Effort:** 143 puntos  
**Sprints:** 5-6 (28 puntos/sprint)

---

## 🚀 Próximos Pasos

1. ✅ Definición de Intents (DONE)
2. ✅ User Stories (DONE)
3. ⏳ Wireframes/Mockups
4. ⏳ Modelos de datos
5. ⏳ Inicializar proyecto Android
6. ⏳ Escribir tests (TDD)
7. ⏳ Implementar features

