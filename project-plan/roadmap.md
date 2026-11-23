# 🗺️ Project Roadmap - Workout Companion

**Versión:** 1.0  
**Fecha:** 2026-11-23  
**Duración Total:** 12 Sprints (12-15 semanas)

---

## 📊 Visión General

```
Phase 1: Foundation        Phase 2: Core Features    Phase 3: Polish
(Sprints 1-3)              (Sprints 4-9)             (Sprints 10-12)

Setup Android              Rutinas Personalizadas   UI Refinement
DB Schema                  Postura Detection        Testing
Base Architecture          Analytics                Performance
                          Multiple Routines        Release Prep
```

---

## 🎯 Hitos Principales (Milestones)

| Milestone | Target | Sprints | Key Features |
|-----------|--------|---------|--------------|
| **M1: Proyecto Setup** | Semana 2 | 1 | Android project, DB schema, CI/CD |
| **M2: MVP Rutinas** | Semana 4 | 2 | Personalización básica, generador |
| **M3: Cámara Integrada** | Semana 6 | 3 | MediaPipe, skeleton display |
| **M4: Postura Funcional** | Semana 8 | 4 | Validación por ejercicio |
| **M5: Analytics Beta** | Semana 10 | 5-6 | Progreso, charts, insights |
| **M6: Alpha Release** | Semana 12 | 7-8 | Múltiples rutinas, configuración avanzada |
| **M7: Beta Release** | Semana 14 | 9-10 | Full testing, bug fixes, optimizations |
| **M8: Production Release** | Semana 15+ | 11-12 | App store release ready |

---

## 📅 CRONOGRAMA DETALLADO

### **SPRINT 1: Proyecto & Arquitectura (Semana 1-2)**

**Duración:** 2 semanas  
**Effort:** 25 puntos

**Objetivos:**
- [ ] Configurar proyecto Android con Kotlin
- [ ] Configurar MVVM + Clean Architecture
- [ ] Crear esquema de base de datos
- [ ] Configurar CI/CD con GitHub Actions
- [ ] Setup testing framework

**User Stories:**
- [ ] Setup Android Project (US-001)
- [ ] Configure Database Schema (US-002)
- [ ] Setup CI/CD Pipeline (US-003)
- [ ] Testing Infrastructure (US-004)

**Deliverables:**
- ✅ Proyecto Android compilable
- ✅ DB migrations en lugar
- ✅ GitHub Actions workflow
- ✅ Documentation

**Definition of Done:**
- [ ] Code compiles sin errores
- [ ] All unit tests pass
- [ ] Code review completado

---

### **SPRINT 2: Generador de Rutinas (Semana 3-4)**

**Duración:** 2 semanas  
**Effort:** 31 puntos

**Objetivos:**
- Implementar cuestionario de perfil
- Crear algoritmo de generación
- UI para visualizar rutinas
- Editar rutinas

**User Stories:**
- [x] US-101: Cuestionario perfil
- [x] US-102: Generar rutina
- [x] US-103: Detalles ejercicio
- [x] US-104: Editar rutina
- [x] US-105: Alternativas
- [x] US-106: Guardar rutina

**Deliverables:**
- ✅ Questionnaire UI (Jetpack Compose)
- ✅ Routine generation algorithm
- ✅ Routine detail screen
- ✅ Exercise database (100+ exercises)
- ✅ Edit/customize functionality

**Definition of Done:**
- [ ] Generación < 5 segundos
- [ ] All acceptance criteria met
- [ ] Integration tests pass
- [ ] Code review

---

### **SPRINT 3: Integración Cámara (Semana 5-6)**

**Duración:** 2 semanas  
**Effort:** 25 puntos

**Objetivos:**
- Integrar MediaPipe Pose
- Visualizar skeleton en video
- Configurar permisos de cámara
- Manejar iluminación pobre

**User Stories:**
- [x] US-201: Habilitar cámara
- [x] US-202: Skeleton visual
- [x] US-205: Detectar baja iluminación

**Deliverables:**
- ✅ CameraX + MediaPipe integration
- ✅ Real-time pose detection
- ✅ Skeleton rendering (OpenGL)
- ✅ Permission handling
- ✅ Flashlight support

**Definition of Done:**
- [ ] 30+ FPS on mid-range devices
- [ ] Confidence score displayed
- [ ] Lighting detection working
- [ ] Tested on 5+ devices

---

### **SPRINT 4: Validación de Postura (Semana 7-8)**

**Duración:** 2 semanas  
**Effort:** 39 puntos

**Objetivos:**
- Implementar validaciones por ejercicio
- Rep counting con feedback
- Form scoring
- Exercise-specific logic

**User Stories:**
- [x] US-203: Validar postura Squat
- [x] US-204: Contar reps
- [x] US-206: Validar Bench Press
- Plus: Deadlift, Push-up, Pull-up

**Deliverables:**
- ✅ Squat validation algorithm
- ✅ Bench Press validation
- ✅ Deadlift validation
- ✅ Rep counter logic
- ✅ Form scoring (0-100)
- ✅ Visual feedback (green/red)
- ✅ Audio/vibration feedback

**Definition of Done:**
- [ ] ≥85% precision validated with experts
- [ ] All 5 exercises working
- [ ] Edge cases handled
- [ ] Performance optimized

---

### **SPRINT 5: Rastreo de Progreso (Semana 9-10)**

**Duración:** 2 semanas  
**Effort:** 42 puntos

**Objetivos:**
- Registrar datos de sesiones
- Visualizar progreso
- Personal Records
- Gráficos avanzados

**User Stories:**
- [x] US-301: Registrar sesión
- [x] US-302: Historial entrenamientos
- [x] US-303: Personal Records
- [x] US-304: Gráficos progreso
- [x] US-305: Insights inteligentes

**Deliverables:**
- ✅ Session recording UI
- ✅ Workout history screen
- ✅ PR tracking + notifications
- ✅ 5 analytics charts
- ✅ Insights engine
- ✅ Data export (PDF/CSV)

**Definition of Done:**
- [ ] Charts render < 1 second
- [ ] Queries optimized (< 500ms)
- [ ] Tested with 1000+ records

---

### **SPRINT 6: Múltiples Rutinas & Config Avanzada (Semana 11-12)**

**Duración:** 2 semanas  
**Effort:** 31 puntos

**Objetivos:**
- Gestión de múltiples rutinas
- Periodización de entrenamientos
- Configuración avanzada
- Switching entre rutinas

**User Stories:**
- [x] INT-4: Manage Multiple Routines
- [x] INT-5: Advanced Configuration

**Deliverables:**
- ✅ Multiple routines management
- ✅ Routine switching
- ✅ Archive functionality
- ✅ Periodization planner
- ✅ Advanced parameters
- ✅ Deload week automation

**Definition of Done:**
- [ ] Seamless switching
- [ ] No data loss
- [ ] User testing passed

---

### **SPRINT 7-8: Refinamiento de UI/UX & Testing (Semana 13-16)**

**Duración:** 4 semanas  
**Effort:** 50 puntos

**Objetivos:**
- Diseño Material Design 3
- Temas (Light/Dark)
- Animaciones
- Accesibilidad
- Comprehensive testing

**Tasks:**
- [ ] Design system implementation
- [ ] All screens Material 3
- [ ] Dark mode support
- [ ] Animations & transitions
- [ ] Accessibility (a11y) audit
- [ ] Comprehensive test suite
- [ ] Integration tests
- [ ] E2E tests

**Deliverables:**
- ✅ Polished UI
- ✅ Smooth animations
- ✅ Dark/Light themes
- ✅ 90%+ test coverage
- ✅ Accessibility compliant

---

### **SPRINT 9-10: Performance & Optimization (Semana 17-20)**

**Duración:** 4 semanas  
**Effort:** 40 puntos

**Objetivos:**
- Optimizar rendimiento
- Reducir consumo de batería
- Offline support
- Crashlytics & monitoring

**Tasks:**
- [ ] Performance profiling
- [ ] Database optimization
- [ ] Camera efficiency
- [ ] Battery optimization
- [ ] Offline capability
- [ ] Error handling & reporting
- [ ] Crash analytics

**Deliverables:**
- ✅ App <120MB
- ✅ Cold start < 3 segundos
- ✅ Offline mode working
- ✅ Error tracking setup

---

### **SPRINT 11-12: Beta & Release (Semana 21-24)**

**Duración:** 4 semanas  
**Effort:** 35 puntos

**Objetivos:**
- Beta testing con usuarios reales
- Feedback incorporation
- Production readiness
- Play Store release

**Tasks:**
- [ ] Internal testing
- [ ] Closed beta (50-100 users)
- [ ] Bug fixes from beta
- [ ] Final documentation
- [ ] Play Store listings
- [ ] Privacy policy & T&C
- [ ] Release notes
- [ ] Production deployment

**Deliverables:**
- ✅ Beta app published
- ✅ User feedback collected
- ✅ Final version
- ✅ Play Store ready

---

## 📊 CAPACIDAD DE SPRINTS

```
Sprint 1:  25 pts  ████░░░░░░░░  
Sprint 2:  31 pts  ██████░░░░░░
Sprint 3:  25 pts  ████░░░░░░░░
Sprint 4:  39 pts  ████████░░░░
Sprint 5:  42 pts  ████████░░░░
Sprint 6:  31 pts  ██████░░░░░░
Sprint 7-8: 50 pts (25 pts/semana)
Sprint 9-10: 40 pts (20 pts/semana)
Sprint 11-12: 35 pts (17.5 pts/semana)

Total: ~308 puntos de trabajo
```

---

## 🎯 KEY PERFORMANCE INDICATORS (KPIs)

| KPI | Target | Measurement |
|-----|--------|-------------|
| **Performance** | 30+ FPS camera | Profiling tool |
| **Accuracy** | ≥85% form detection | Expert validation |
| **Battery** | <5% drain/hour | Battery monitor |
| **Coverage** | ≥90% unit test | Code coverage tool |
| **Uptime** | 99.9% | Crashlytics |
| **User Rating** | ≥4.0/5.0 | Play Store |
| **Retention** | ≥60% (30-day) | Analytics |

---

## 🚀 PHASE BREAKDOWN

### **PHASE 1: FOUNDATION (Sprints 1-3)**
- Project setup
- Database design
- Architecture implementation
- Basic routine generation
- Camera integration

**Output:** Working app with camera + basic routines

---

### **PHASE 2: CORE FEATURES (Sprints 4-6)**
- Posture validation
- Form feedback
- Progress tracking
- Advanced configuration
- Multiple routines

**Output:** MVP with core features

---

### **PHASE 3: POLISH (Sprints 7-12)**
- UI refinement
- Performance optimization
- Comprehensive testing
- Beta testing
- Production release

**Output:** Production-ready app

---

## 📈 TEAM & ROLES

| Role | Responsibility | Hours/Week |
|------|----------------|-----------|
| **Lead Developer** | Architecture, reviews | 40h |
| **Android Developer** | UI implementation | 40h |
| **ML Developer** | MediaPipe integration | 20h |
| **QA Engineer** | Testing, bug tracking | 30h |
| **UI/UX Designer** | Design, wireframes | 20h |

**Total:** 150 hours/week

---

## 🔄 DEPENDENCIES

```
Sprint 1 (Setup)
    ↓
Sprint 2 (Routines) ← depends on Sprint 1
    ↓
Sprint 3 (Camera) ← depends on Sprint 1
    ↓
Sprint 4 (Posture) ← depends on Sprint 3
    ↓
Sprint 5 (Progress) ← depends on Sprint 2, 4
    ↓
Sprint 6 (Advanced) ← depends on Sprint 2, 5
    ↓
Sprint 7-12 (Polish, Test, Release)
```

---

## 🎁 FUTURE ENHANCEMENTS (Post-MVP)

- [ ] Social features (share workouts, compete)
- [ ] Wearable integration (Apple Watch, Fitbit)
- [ ] AI coach (voice feedback)
- [ ] Personalized meal plans
- [ ] Form comparison (before/after)
- [ ] VR/AR form visualization
- [ ] Group training modes
- [ ] Integration with other fitness apps
- [ ] Advanced periodization (Westside Barbell, 531, PPL, etc)
- [ ] Video storage in cloud
- [ ] Form analytics (angle tracking, tempo analysis)

---

## 📋 SUCCESS CRITERIA

**Launch Ready Checklist:**

- [ ] All 17 User Stories completed
- [ ] ≥90% test coverage
- [ ] Zero critical bugs in beta
- [ ] ≥4.0 star rating (100+ reviews)
- [ ] <5% crash rate
- [ ] Load time <3 seconds
- [ ] Form accuracy ≥85%
- [ ] User retention ≥60%
- [ ] 1000+ downloads (first month)

---

**Last Updated:** 2026-11-23  
**Next Review:** 2026-11-30

