# 🔄 Metodología de Desarrollo: Scrum + Kanban Híbrido

## 🎯 Visión General

ProjectSynergy utiliza una **metodología híbrida** que combina lo mejor de Scrum y Kanban, adaptada para un proyecto de desarrollo individual con enfoque en demostración de habilidades profesionales.

---

## 🏗️ Estructura de la Metodología

### **📅 Sprints de Scrum (Estructura Temporal)**
- **Duración**: 2 semanas por sprint
- **Objetivo**: Entregar incrementos funcionales de valor
- **Rituales adaptados**: Planning, Review, Retrospectiva

### **🔄 Flujo de Kanban (Gestión Continua)**
- **Flujo visual**: Tablero con estados claros
- **Límites WIP**: Control de trabajo en progreso
- **Mejora continua**: Optimización del flujo

---

## 📊 Tablero Kanban Híbrido

### **Columnas del Tablero:**

| Columna | Descripción | Límite WIP |
|---------|-------------|------------|
| **📋 Backlog** | Historias de usuario y tareas priorizadas | ∞ |
| **🎯 Sprint Backlog** | Tareas seleccionadas para el sprint actual | 8-12 |
| **🔄 En Progreso** | Trabajo activo en desarrollo | 3 |
| **🧪 En Testing** | Desarrollo completo, en fase de pruebas | 2 |
| **👀 Code Review** | Esperando revisión de código | 2 |
| **✅ Hecho** | Completado y desplegado | ∞ |

### **🏷️ Etiquetas y Clasificación:**

**Por Tipo:**
- `feature` - Nueva funcionalidad
- `bug` - Corrección de errores
- `tech-debt` - Mejoras técnicas
- `documentation` - Documentación
- `testing` - Pruebas

**Por Prioridad:**
- `critical` - Crítico para el MVP
- `high` - Alta prioridad
- `medium` - Prioridad media
- `low` - Baja prioridad

**Por Componente:**
- `backend` - Backend .NET
- `frontend` - Frontend Next.js
- `infrastructure` - DevOps/Infraestructura
- `database` - Base de datos

---

## 🎯 Ceremonias Adaptadas

### **📋 Sprint Planning (Cada 2 semanas)**
**Duración**: 2 horas
**Participantes**: Desarrollador principal (auto-facilitado)

**Agenda:**
1. **Revisión del Sprint anterior** (30 min)
   - Velocidad alcanzada
   - Lecciones aprendidas
   - Ajustes necesarios

2. **Selección de Historias** (60 min)
   - Priorización del Product Backlog
   - Estimación con Planning Poker (adaptado)
   - Selección basada en capacidad

3. **Definición de Objetivos** (30 min)
   - Objetivo del Sprint
   - Criterios de éxito
   - Riesgos identificados

**Entregable**: `SPRINT_X_PLANNING.md`

### **📈 Daily Standups (Diario - Auto-documentado)**
**Duración**: 15 minutos
**Formato**: Reflexión personal documentada

**Preguntas clave:**
- ¿Qué completé ayer?
- ¿Qué haré hoy?
- ¿Qué impedimentos tengo?

**Entregable**: Actualización en `DAILY_LOG.md`

### **🎪 Sprint Review (Final de Sprint)**
**Duración**: 1 hora
**Participantes**: Auto-revisión con documentación

**Agenda:**
1. **Demo del Incremento** (30 min)
   - Funcionalidades completadas
   - Captura de pantallas/videos
   - Despliegue en ambiente de staging

2. **Métricas del Sprint** (30 min)
   - Velocidad alcanzada
   - Calidad del código
   - Cobertura de pruebas

**Entregable**: `SPRINT_X_REVIEW.md`

### **🔄 Sprint Retrospectiva (Final de Sprint)**
**Duración**: 1 hora
**Formato**: Reflexión estructurada

**Metodología - Start/Stop/Continue:**
- **Start**: ¿Qué debería empezar a hacer?
- **Stop**: ¿Qué debería dejar de hacer?
- **Continue**: ¿Qué debería seguir haciendo?

**Entregable**: `SPRINT_X_RETROSPECTIVE.md`

---

## 📏 Métricas y KPIs

### **📊 Métricas de Velocidad**
- **Story Points completados por sprint**
- **Tiempo promedio por historia**
- **Burndown chart del sprint**

### **🎯 Métricas de Calidad**
- **Cobertura de código por sprint**
- **Número de bugs encontrados**
- **Tiempo de resolución de issues**

### **🔄 Métricas de Flujo (Kanban)**
- **Lead Time**: Tiempo desde creación hasta completado
- **Cycle Time**: Tiempo en desarrollo activo
- **Throughput**: Elementos completados por semana

---

## 🛠️ Herramientas y Configuración

### **GitHub Projects v2**
- **Tablero principal**: Vista Kanban con todas las columnas
- **Vista de Sprint**: Filtrada por sprint actual
- **Vista de Roadmap**: Timeline de épicas y milestones

### **Automatizaciones**
- **Auto-move**: Issues en PR → "Code Review"
- **Auto-close**: PR merged → "Hecho"
- **Auto-label**: Por tipo de archivo modificado

### **Templates y Workflows**
- **Issue templates**: Para cada tipo de trabajo
- **PR template**: Con checklist de calidad
- **GitHub Actions**: CI/CD integrado con el flujo

---

## 📋 Definición de Hecho (DoD)

### **Para Historias de Usuario:**
- [ ] Código implementado siguiendo estándares
- [ ] Pruebas unitarias con >80% cobertura
- [ ] Pruebas de integración cuando aplique
- [ ] Documentación técnica actualizada
- [ ] Code review completado
- [ ] Funcionalidad desplegada en staging
- [ ] Criterios de aceptación cumplidos

### **Para Tareas Técnicas:**
- [ ] Implementación completada
- [ ] Pruebas de regresión pasando
- [ ] Documentación técnica actualizada
- [ ] No introduce deuda técnica
- [ ] Métricas de calidad mantenidas

---

## 🎯 Adaptaciones para Proyecto Individual

### **🔄 Flexibilidad de Scrum**
- **Sprints variables**: Pueden extenderse si es necesario
- **Ceremonias asíncronas**: Documentadas en lugar de meetings
- **Roles unificados**: Product Owner + Scrum Master + Developer

### **📈 Beneficios de Kanban**
- **Visibilidad continua**: Estado del proyecto siempre visible
- **Límites WIP**: Evita sobrecarga de trabajo
- **Mejora continua**: Optimización constante del proceso

### **📊 Reporting Profesional**
- **Dashboards automatizados**: Métricas en tiempo real
- **Reportes de sprint**: Documentación profesional
- **Demos grabadas**: Para mostrar progreso

---

## 🚀 Implementación Práctica

### **Semana 1 de cada Sprint:**
- **Lunes**: Sprint Planning
- **Martes-Viernes**: Desarrollo con daily logs
- **Viernes**: Mid-sprint review

### **Semana 2 de cada Sprint:**
- **Lunes-Jueves**: Desarrollo y testing
- **Viernes**: Sprint Review + Retrospectiva + Planning siguiente

### **Herramientas de Seguimiento:**
- **GitHub Projects**: Tablero principal
- **GitHub Milestones**: Sprints y releases
- **GitHub Discussions**: Reflexiones y decisiones
- **README.md**: Dashboard de proyecto actualizado

---

## 📈 Evolución de la Metodología

La metodología se adaptará basada en:
- **Lecciones aprendidas** en retrospectivas
- **Feedback** de revisores del código
- **Métricas** de productividad y calidad
- **Necesidades** del proyecto en evolución

Esta metodología híbrida demuestra **madurez profesional** y capacidad de adaptar frameworks ágiles a contextos específicos, una habilidad muy valorada en entornos empresariales. 