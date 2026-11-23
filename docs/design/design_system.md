# 🎨 Diseño Visual & Components

**Versión:** 1.0  
**Fecha:** 2026-11-23  
**Referencia:** Material Design 3

---

## 📐 DIMENSIONES & LAYOUT

### Dispositivos Android:
- **Small phones:** <360px width
- **Regular phones:** 360-540px (PRIMARY TARGET)
- **Large phones:** 540-760px
- **Tablets:** >760px

### Safes Areas:
- **Status bar:** 24dp (top)
- **Navigation bar:** 56dp (bottom)
- **Screen padding:** 16dp (left/right)

---

## 🎨 PALETA DE COLORES

### Material Design 3 (Dark Mode - recomendado)

```
PRIMARY (Purple)
├─ #6200EE    Main color
├─ #7C3DFA    Container
└─ #EDE7F6    Light

SECONDARY (Teal)
├─ #03DAC6    Main color
├─ #72F1EA    Container
└─ #E0F7F6    Light

TERTIARY (Accent)
├─ #BB86FC    Main color
├─ #EDE7F6    Container
└─ #F3E5FF    Light

BACKGROUND
├─ #121212    Surface (darkest)
├─ #1E1E1E    Container
└─ #FFFFFF    Text (white)

ERROR
├─ #F44336    Red (errors)
├─ #FFEBEE    Light red
└─ #C62828    Dark red

SUCCESS
├─ #4CAF50    Green (success)
├─ #E8F5E9    Light green
└─ #2E7D32    Dark green

WARNING
├─ #FFC107    Amber (warning)
├─ #FFF8E1    Light amber
└─ #F57F17    Dark amber
```

### Mapa de Uso:

| Uso | Color | Código |
|-----|-------|--------|
| Botones principales | Primary | #6200EE |
| Botones secundarios | Secondary | #03DAC6 |
| Form correct | Success | #4CAF50 |
| Form error | Error | #F44336 |
| Texto principal | White | #FFFFFF |
| Texto secundario | Gray | #9E9E9E |
| Separadores | Divider | #37373F |

---

## 🔤 TIPOGRAFÍA

### Fuente recomendada:
- **Font Family:** Roboto (Google Fonts)
- **Fallback:** System fonts

### Scale:

| Uso | Size | Weight | Line Height | Letter Spacing |
|-----|------|--------|------------|-----------------|
| Display | 48pt | Bold | 56pt | -0.5px |
| Heading | 32pt | Bold | 40pt | 0px |
| Subheading | 20pt | Medium | 28pt | 0.1px |
| Body | 16pt | Regular | 24pt | 0.5px |
| Body Small | 14pt | Regular | 20pt | 0.25px |
| Label | 12pt | Medium | 16pt | 0.4px |
| Caption | 12pt | Regular | 16px | 0.4px |

### Ejemplos en Pantalla:

```
┌─────────────────────────────────┐
│  DISPLAY (48pt Bold)            │  ← Título principal
│                                 │
│  Heading (32pt Bold)            │  ← Título de sección
│  Subheading (20pt Medium)       │  ← Subtítulo
│                                 │
│  Body text (16pt Regular)       │  ← Texto principal del contenido
│  Label (12pt Medium)            │  ← Etiquetas y campos
│  Caption (12pt Regular)         │  ← Información adicional
└─────────────────────────────────┘
```

---

## 📦 COMPONENTES REUTILIZABLES

### 1. BOTONES

#### Primary Button (High Emphasis)
```
┌─────────────────────────────┐
│   START WORKOUT             │  ← Texto (body, white)
│   height: 56dp              │  ← Altura estándar
│   width: match_parent       │  ← Ancho pantalla
│   bg_color: #6200EE         │  ← Primary color
│   radius: 28dp              │  ← Corner radius
│   elevation: 4dp            │  ← Shadow
│   onClick: → Next Screen    │
└─────────────────────────────┘

Padding: 16dp (left/right), 12dp (top/bottom)
State: Normal → Pressed (opacity 0.8) → Disabled (opacity 0.5)
```

#### Secondary Button
```
┌─────────────────────────────┐
│   EDIT ROUTINE              │
│   bg_color: transparent     │  ← Outline style
│   border: 1.5dp #03DAC6     │  ← Secondary color
│   text_color: #03DAC6       │
└─────────────────────────────┘
```

#### Icon Button
```
  ⭕                          ← 48 × 48 dp
  bg_color: transparent
  icon_size: 24dp
  On click: ripple effect
```

### 2. TEXT INPUTS

```
┌────────────────────────────────┐
│ Email Address              ✗   │  ← Label + Clear button
│ [user@example.com          ]   │
│ ────────────────────────────── │  ← Underline
│ 23 / 50 characters             │  ← Helper text
└────────────────────────────────┘

Altura: 56dp
Padding: 12dp (vertical), 16dp (horizontal)
Focus state: Underline color → #6200EE
Error state: Text color → #F44336
Helper text: 12pt gray
```

### 3. CARDS

```
┌─────────────────────────────────┐
│                                 │
│  Bench Press                    │  ← Title (20pt medium)
│  4 sets × 8 reps                │  ← Content
│  Rest: 90s                      │
│                                 │
│  ⭐⭐⭐ Difficulty               │
│  🔽 More Details                │  ← Action
│                                 │
├─────────────────────────────────┤
│  EDIT          DELETE           │  ← Buttons (optional)
└─────────────────────────────────┘

Corner radius: 12dp
Elevation: 1dp
Margin: 8dp
Padding: 16dp
On click: → Card elevation 8dp
```

### 4. CHIPS

```
┌─────────────┐
│ ✓ Muscle Gain │  ← Selected
└─────────────┘

┌─────────────┐
│ ○ Fat Loss    │  ← Unselected
└─────────────┘

Height: 32dp
Padding: 8dp (horizontal)
Corner radius: 16dp
Icon: 18dp (left)
Texto: 14pt medium
```

### 5. PROGRESS BARS

```
Determinate:
┌───────────────────────┐
│████████████░░░░░░░░░  │  ← 60% filled
│ 60%                   │
└───────────────────────┘
Height: 4dp
Corner radius: 2dp

Indeterminate (Loading):
┌───────────────────────┐
│  ▓▓▓▓░░░░░░░░░░░░░░  │  ← Animated
└───────────────────────┘
```

### 6. BOTTOM SHEET

```
╔═══════════════════════════════╗
║  ▯▯▯  Drag handle (white bar) ║
│                               │
│  Content area                 │
│  (Scrollable if needed)       │
│                               │
│  ┌─────────────────────────┐  │
│  │  ACTION BUTTON          │  │
│  └─────────────────────────┘  │
╚═══════════════════════════════╝

Corner radius: 16dp (top)
Padding: 16dp (all sides)
Height: Flexible (wrap_content)
Elevation: 8dp
Drag handle height: 4dp
```

### 7. DIALOGS / ALERTS

```
╔═══════════════════════════════╗
│  ⚠️  Confirm Action           │  ← Icon + Title
│                               │
│  Are you sure you want to     │  ← Message
│  delete this routine?         │
│                               │
│  🛑 CANCEL    ✓ DELETE       │  ← Buttons
╚═══════════════════════════════╝

Padding: 24dp (all sides)
Corner radius: 12dp
Text title: 24pt bold
Text body: 16pt regular
Width: 90% pantalla (máx 500dp)
```

### 8. SNACKBARS / TOASTS

```
╔═══════════════════════════════╗
│  ✓ Routine saved successfully │  ← Message + icon
│                               UNDO  ← Action (optional)
╚═══════════════════════════════╝

Position: Bottom, above nav bar
Padding: 12dp
Height: 48dp
Duration: 3-4 segundos
Corner radius: 4dp
Elevation: 6dp
```

### 9. CARDS EXPANDIBLE

```
Normal State:
┌───────────────────────────┐
│ Exercise 1                │
│ 4 sets × 10 reps          │
│ 🔽                        │  ← Expand icon
└───────────────────────────┘

Expanded State:
┌───────────────────────────┐
│ Exercise 1                │
│ 4 sets × 10 reps          │
│ ▲                         │  ← Collapse icon
├───────────────────────────┤
│ Instructions:             │
│ 1. Stand with feet...     │  ← Extra content
│ 2. Hold dumbbells...      │
│ 3. Perform movement...    │
└───────────────────────────┘

Animation: Expand/collapse 200ms
```

---

## 🎬 ANIMACIONES

### Timing:
- **Quick:** 150ms (micro interactions)
- **Standard:** 200ms (screen transitions)
- **Slow:** 300ms (important transitions)

### Curva de Easing:
- **Material Standard:** Cubic Bezier (0.4, 0.0, 0.2, 1.0)
- **Material Decelerate:** Cubic Bezier (0.0, 0.0, 0.2, 1.0)
- **Material Accelerate:** Cubic Bezier (0.4, 0.0, 1.0, 1.0)

### Ejemplos:

**1. Button Click Ripple**
```
Material Ripple effect
Duration: 150ms
Radius: Grows from touch point
Color: White with 20% opacity
```

**2. Screen Transition**
```
Fade in: 200ms
Slide up: Y: 100dp → 0dp (200ms)
Easing: Material Standard
```

**3. Card Expansion**
```
Height: Current → Expanded (200ms)
Opacity: Content fade in (200ms)
Easing: Material Standard
```

**4. Skeleton Loading**
```
Shimmer effect: Slide left → right (1000ms)
Color: Gray → Lighter gray → Gray
Repeat: Until data loads
```

---

## 🌙 DARK MODE

Workout Companion usa **Dark Mode por defecto** (Material Dark Theme)

### Colores en Dark Mode:
```
Background:      #121212  (Very dark gray)
Surface:         #1E1E1E  (Dark gray)
Overlay:         #37373F  (Darker gray)
On Background:   #FFFFFF  (White text)
On Surface:      #FFFFFF  (White text)
Outline:         #79747E  (Gray outline)
```

### Cambios en Componentes:
- **Text:** Blanco (#FFFFFF) en fondos oscuros
- **Icons:** Blanco (#FFFFFF)
- **Dividers:** Gris oscuro (#37373F)
- **Input backgrounds:** #37373F en lugar de blanco

---

## ♿ ACCESIBILIDAD

### Requisitos WCAG 2.1 AA:

**1. Contraste de Color**
```
Mínimo 4.5:1 para texto normal
Mínimo 3:1 para texto grande (18pt+)
```

**2. Tamaño de Touch**
```
Mínimo 48dp × 48dp para botones
Espaciado: 8dp entre elementos
```

**3. Labels & Descriptions**
```
<Button
  android:contentDescription="@string/button_start"
/>

Todos los ImageButtons necesitan description
```

**4. Text Size**
```
Mínimo 14pt para cuerpo
Permitir escalado hasta 200%
```

**5. Colors Alone**
```
No usar solo color para información
✓ Green checkmark + "Success"
✗ Just green background
```

---

## 📱 RESPONSIVE GRID

### 12-Column Grid System:

```
Small (360px):
├─ 2 columns (gap: 8dp)

Regular (540px):
├─ 4 columns (gap: 12dp)

Large (720px):
├─ 6 columns (gap: 16dp)

Tablet (760px+):
├─ 12 columns (gap: 16dp)
```

### Ejemplo Layout:
```
Small (360px):       Regular (540px):     Large (720px+):
┌─────────────┐      ┌──────────────┐    ┌────────────────┐
│   Card 1    │      │  Card 1  │   │    │ Card 1 │ Card 2│
│   Card 2    │      │  Card 2  │   │    │────────────────│
│   Card 3    │      │  Card 3  │   │    │ Card 3 │ Card 4│
└─────────────┘      │────────────   │    └────────────────┘
                     │  Card 4      │
                     └──────────────┘
```

---

## 🎯 ESTADOS DE COMPONENTES

### Button States:
```
1. Enabled:        Color normal, cursor pointer
2. Hovered:        Overlay 8% black
3. Focused:        Outline 3dp color
4. Pressed:        Overlay 16% black
5. Disabled:        Opacity 38%, no interaction
```

### Form Input States:
```
1. Default:        Border + hint text
2. Focused:        Border color → primary, cursor visible
3. Filled:         Border + text filled
4. Error:          Border → error color, error message below
5. Success:        Green checkmark + success message
6. Disabled:       Opacity 38%, no interaction
```

### Loading States:
```
1. Skeleton:       Gray placeholder cards
2. Loading:        Spinner + "Loading..." message
3. Empty State:    Icon + message "No data"
4. Error:          Error icon + retry button
5. Loaded:         Content appears
```

---

## 🎨 TEMAS PREDEFINIDOS

Aunque es Dark Mode por defecto, soportar Light Mode:

### Light Mode Paleta:
```
Background:      #FFFFFF  (White)
Surface:         #F5F5F5  (Light gray)
Text:            #212121  (Dark gray)
Dividers:        #E0E0E0  (Light gray)
Primary:         #6200EE  (Same)
Error:           #B00020  (Darker red)
```

---

## 📐 MEDIDAS ESTÁNDAR

### Espaciado Vertical:
```
Párrafos:        16-24dp
Secciones:       32dp
Cards:           16dp padding
```

### Espaciado Horizontal:
```
Screen edges:    16dp
Card padding:    16dp
Element gap:     8dp
```

### Altura de Componentes:
```
Button:          56dp
Input field:     56dp
Icon button:     48dp
Chip:            32dp
List item:       64dp
Card:            Variable
```

---

**Próximo:** Implementar en Jetpack Compose

