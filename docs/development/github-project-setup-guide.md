# 🚀 Guía Completa de Configuración de GitHub Project
## ProjectSynergy - Setup Completo

Esta guía contiene todos los comandos y pasos necesarios para configurar completamente el GitHub Project de ProjectSynergy, incluyendo la creación del proyecto, campos personalizados, milestones, labels e issues.

---

## 📋 **Índice**
1. [Prerequisitos](#prerequisitos)
2. [Crear GitHub Project](#crear-github-project)
3. [Configurar Campos Personalizados](#configurar-campos-personalizados)
4. [Crear Labels del Repositorio](#crear-labels-del-repositorio)
5. [Crear Milestones](#crear-milestones)
6. [Crear Issues Iniciales](#crear-issues-iniciales)
7. [Añadir Issues al Proyecto](#añadir-issues-al-proyecto)
8. [Configuración Manual desde Web](#configuración-manual-desde-web)

---

## 🔧 **Prerequisitos**

### Verificar dependencias:
```bash
# Verificar GitHub CLI
gh --version

# Verificar autenticación
gh auth status

# Verificar permisos del proyecto (si es necesario)
gh auth refresh -s project,read:project
```

### Variables del proyecto:
```bash
REPO_OWNER="Nekstoreo"
REPO_NAME="ProjectSynergy"
PROJECT_TITLE="ProjectSynergy Development"
PROJECT_ID="PVT_kwHOBdcx6c4A8ImB"  # Se obtiene después de crear el proyecto
```

---

## 🏗️ **Crear GitHub Project**

### Opción 1: Comando directo
```bash
gh project create --owner Nekstoreo --title "ProjectSynergy Development"
```

### Opción 2: Con GraphQL API
```bash
gh api graphql -f query='
mutation($ownerId: ID!, $title: String!) {
  createProjectV2(input: {ownerId: $ownerId, title: $title}) {
    projectV2 {
      id
      url
      number
    }
  }
}' -f ownerId="$(gh api user --jq .node_id)" -f title="ProjectSynergy Development"
```

### Obtener información del proyecto:
```bash
gh project list --owner Nekstoreo
```

---

## 🎯 **Configurar Campos Personalizados**

> **Nota**: El campo "Status" se crea automáticamente al crear el proyecto, pero necesita ser editado manualmente desde la interfaz web.

### 1. Campo Priority
```bash
gh api graphql -f query='
mutation($projectId: ID!) {
  createProjectV2Field(input: {
    projectId: $projectId,
    name: "Priority",
    dataType: SINGLE_SELECT,
    singleSelectOptions: [
      {name: "Critical", description: "Requires immediate attention", color: RED},
      {name: "High", description: "High priority issue", color: ORANGE},
      {name: "Medium", description: "Moderate importance", color: YELLOW},
      {name: "Low", description: "Low priority or nice-to-have", color: GREEN}
    ]
  }) {
    projectV2Field {
      ... on ProjectV2SingleSelectField {
        id
      }
    }
  }
}' -f projectId="PVT_kwHOBdcx6c4A8ImB"
```

### 2. Campo Epic
```bash
gh api graphql -f query='
mutation($projectId: ID!) {
  createProjectV2Field(input: {
    projectId: $projectId,
    name: "Epic",
    dataType: SINGLE_SELECT,
    singleSelectOptions: [
      {name: "Authentication", description: "Login, register, etc.", color: BLUE},
      {name: "Ticket Management", description: "Managing helpdesk tickets", color: PURPLE},
      {name: "Knowledge Base", description: "Documentation and FAQs", color: GREEN},
      {name: "Dashboard", description: "Admin or user dashboards", color: ORANGE},
      {name: "Infrastructure", description: "Underlying systems & services", color: GRAY}
    ]
  }) {
    projectV2Field {
      ... on ProjectV2SingleSelectField {
        id
      }
    }
  }
}' -f projectId="PVT_kwHOBdcx6c4A8ImB"
```

### 3. Campo Component
```bash
gh api graphql -f query='
mutation($projectId: ID!) {
  createProjectV2Field(input: {
    projectId: $projectId,
    name: "Component",
    dataType: SINGLE_SELECT,
    singleSelectOptions: [
      {name: "Backend", description: "Server-side logic", color: PURPLE},
      {name: "Frontend", description: "UI and client logic", color: BLUE},
      {name: "Database", description: "Data storage", color: GREEN},
      {name: "Infrastructure", description: "Servers, CI/CD, etc.", color: GRAY},
      {name: "Documentation", description: "Guides, wikis, READMEs", color: YELLOW}
    ]
  }) {
    projectV2Field {
      ... on ProjectV2SingleSelectField {
        id
      }
    }
  }
}' -f projectId="PVT_kwHOBdcx6c4A8ImB"
```

### 4. Campo Sprint
```bash
gh api graphql -f query='
mutation($projectId: ID!) {
  createProjectV2Field(input: {
    projectId: $projectId,
    name: "Sprint",
    dataType: SINGLE_SELECT,
    singleSelectOptions: [
      {name: "Sprint 1", description: "Sprint 1 of the project", color: BLUE},
      {name: "Sprint 2", description: "Sprint 2 of the project", color: BLUE},
      {name: "Sprint 3", description: "Sprint 3 of the project", color: BLUE},
      {name: "Sprint 4", description: "Sprint 4 of the project", color: BLUE},
      {name: "Sprint 5", description: "Sprint 5 of the project", color: BLUE},
      {name: "Sprint 6", description: "Sprint 6 of the project", color: BLUE},
      {name: "Sprint 7", description: "Sprint 7 of the project", color: BLUE},
      {name: "Sprint 8", description: "Sprint 8 of the project", color: BLUE}
    ]
  }) {
    projectV2Field {
      ... on ProjectV2SingleSelectField {
        id
      }
    }
  }
}' -f projectId="PVT_kwHOBdcx6c4A8ImB"
```

### 5. Campo Story Points (Número)
```bash
gh api graphql -f query='
mutation($projectId: ID!) {
  createProjectV2Field(input: {
    projectId: $projectId,
    name: "Story Points",
    dataType: NUMBER
  }) {
    projectV2Field {
      ... on ProjectV2NumberField {
        id
      }
    }
  }
}' -f projectId="PVT_kwHOBdcx6c4A8ImB"
```

---

## 🏷️ **Crear Labels del Repositorio**

```bash
# Labels por tipo
gh label create "feature" --color "0052CC" --description "Nueva funcionalidad"
gh label create "bug" --color "D73A4A" --description "Error o problema"
gh label create "technical-task" --color "FEF2C0" --description "Tarea técnica"
gh label create "documentation" --color "0075CA" --description "Documentación"
gh label create "testing" --color "BFD4F2" --description "Pruebas"

# Labels por prioridad
gh label create "priority-critical" --color "B60205" --description "Prioridad crítica"
gh label create "priority-high" --color "D93F0B" --description "Prioridad alta"
gh label create "priority-medium" --color "FBCA04" --description "Prioridad media"
gh label create "priority-low" --color "0E8A16" --description "Prioridad baja"

# Labels por componente
gh label create "backend" --color "1D76DB" --description "Componente backend"
gh label create "frontend" --color "F9D0C4" --description "Componente frontend"
gh label create "database" --color "006B75" --description "Base de datos"
gh label create "infrastructure" --color "5319E7" --description "Infraestructura"

# Labels por épica
gh label create "epic-auth" --color "0052CC" --description "Epic: Autenticación"
gh label create "epic-tickets" --color "5319E7" --description "Epic: Gestión de tickets"
gh label create "epic-knowledge" --color "0E8A16" --description "Epic: Base de conocimiento"
gh label create "epic-dashboard" --color "D93F0B" --description "Epic: Dashboard"
gh label create "epic-infrastructure" --color "6F42C1" --description "Epic: Infraestructura"
```

---

## 📅 **Crear Milestones**

```bash
# Función para calcular fechas futuras (Linux/GNU date)
get_future_date() {
    date -d "+$1 days" +%Y-%m-%d
}

# Crear milestones con fechas calculadas
gh api repos/Nekstoreo/ProjectSynergy/milestones \
  --method POST \
  --field title="Phase 1: Strategic Planning" \
  --field description="Documentación de arquitectura, estándares y metodología" \
  --field due_on="$(get_future_date 14)T23:59:59Z" \
  --field state="open"

gh api repos/Nekstoreo/ProjectSynergy/milestones \
  --method POST \
  --field title="Phase 2: Environment Setup" \
  --field description="CI/CD, Docker, herramientas de desarrollo" \
  --field due_on="$(get_future_date 21)T23:59:59Z" \
  --field state="open"

gh api repos/Nekstoreo/ProjectSynergy/milestones \
  --method POST \
  --field title="Sprint 1: Core Domain + Auth" \
  --field description="Implementación de entidades de dominio y sistema de autenticación" \
  --field due_on="$(get_future_date 35)T23:59:59Z" \
  --field state="open"

gh api repos/Nekstoreo/ProjectSynergy/milestones \
  --method POST \
  --field title="Sprint 2: Core Domain + Auth (Cont.)" \
  --field description="Continuación de autenticación y API básica de usuarios" \
  --field due_on="$(get_future_date 49)T23:59:59Z" \
  --field state="open"

gh api repos/Nekstoreo/ProjectSynergy/milestones \
  --method POST \
  --field title="Sprint 3: Ticket Management" \
  --field description="CRUD completo de tickets y sistema de estados" \
  --field due_on="$(get_future_date 63)T23:59:59Z" \
  --field state="open"

gh api repos/Nekstoreo/ProjectSynergy/milestones \
  --method POST \
  --field title="Sprint 4: Ticket Management (Cont.)" \
  --field description="Asignación, seguimiento y validaciones de negocio" \
  --field due_on="$(get_future_date 77)T23:59:59Z" \
  --field state="open"

gh api repos/Nekstoreo/ProjectSynergy/milestones \
  --method POST \
  --field title="Sprint 5: Frontend & UX" \
  --field description="Componentes UI reutilizables y páginas principales" \
  --field due_on="$(get_future_date 91)T23:59:59Z" \
  --field state="open"

gh api repos/Nekstoreo/ProjectSynergy/milestones \
  --method POST \
  --field title="Sprint 6: Frontend & UX (Cont.)" \
  --field description="Gestión de estado y responsive design" \
  --field due_on="$(get_future_date 105)T23:59:59Z" \
  --field state="open"

gh api repos/Nekstoreo/ProjectSynergy/milestones \
  --method POST \
  --field title="Sprint 7: Advanced Features" \
  --field description="Sistema de roles, base de conocimiento y dashboard" \
  --field due_on="$(get_future_date 119)T23:59:59Z" \
  --field state="open"

gh api repos/Nekstoreo/ProjectSynergy/milestones \
  --method POST \
  --field title="Sprint 8: Advanced Features (Cont.)" \
  --field description="Sistema de notificaciones y características avanzadas" \
  --field due_on="$(get_future_date 133)T23:59:59Z" \
  --field state="open"

gh api repos/Nekstoreo/ProjectSynergy/milestones \
  --method POST \
  --field title="Phase 5: Deployment & Delivery" \
  --field description="Despliegue en producción y documentación final" \
  --field due_on="$(get_future_date 147)T23:59:59Z" \
  --field state="open"
```

---

## 📝 **Crear Issues Iniciales**

### Issue 1: Backend Setup
```bash
gh issue create \
  --title "[TECH] Configurar estructura inicial del backend .NET" \
  --body "## 🎯 Objetivo
Configurar la estructura inicial del proyecto backend siguiendo Clean Architecture.

## 📋 Tareas
- [ ] Crear solución .NET 8
- [ ] Configurar proyectos por capa (Domain, Application, Infrastructure, API)
- [ ] Configurar Entity Framework Core
- [ ] Setup inicial de logging con Serilog

## 📊 Criterios de Aceptación
- [ ] Estructura de proyectos creada
- [ ] Compilación exitosa
- [ ] Configuración básica completada" \
  --label "technical-task,backend,priority-high,epic-infrastructure" \
  --milestone "Phase 2: Environment Setup"
```

### Issue 2: Frontend Setup
```bash
gh issue create \
  --title "[TECH] Configurar estructura inicial del frontend Next.js" \
  --body "## 🎯 Objetivo
Configurar la estructura inicial del proyecto frontend con Next.js 14.

## 📋 Tareas
- [ ] Crear proyecto Next.js con TypeScript  
- [ ] Configurar TailwindCSS
- [ ] Setup de ShadCN/UI
- [ ] Configurar Zustand para gestión de estado

## 📊 Criterios de Aceptación
- [ ] Proyecto Next.js funcionando
- [ ] Configuración de herramientas completada
- [ ] Página inicial básica creada" \
  --label "technical-task,frontend,priority-high,epic-infrastructure" \
  --milestone "Phase 2: Environment Setup"
```

### Issue 3: User Registration
```bash
gh issue create \
  --title "[USER STORY] Como usuario, quiero poder registrarme en el sistema" \
  --body "## 📖 Historia de Usuario
**Como** usuario nuevo
**Quiero** poder crear una cuenta en el sistema
**Para que** pueda acceder a las funcionalidades de la plataforma

## 📋 Criterios de Aceptación
- [ ] Formulario de registro con validaciones
- [ ] Encriptación de contraseñas
- [ ] Confirmación por email
- [ ] Redirección después del registro exitoso

## 🧪 Pruebas
- [ ] Validaciones de formulario
- [ ] Casos de error (email duplicado, etc.)
- [ ] Flujo completo de registro" \
  --label "feature,frontend,backend,priority-high,epic-auth" \
  --milestone "Sprint 1: Core Domain + Auth"
```

---

## 🔗 **Añadir Issues al Proyecto**

```bash
# Obtener el número del proyecto
gh project list --owner Nekstoreo

# Añadir issues al proyecto (reemplazar NUMBER con el número real)
gh project item-add NUMBER --owner Nekstoreo --url "https://github.com/Nekstoreo/ProjectSynergy/issues/4"
gh project item-add NUMBER --owner Nekstoreo --url "https://github.com/Nekstoreo/ProjectSynergy/issues/5"
gh project item-add NUMBER --owner Nekstoreo --url "https://github.com/Nekstoreo/ProjectSynergy/issues/6"
gh project item-add NUMBER --owner Nekstoreo --url "https://github.com/Nekstoreo/ProjectSynergy/issues/7"
gh project item-add NUMBER --owner Nekstoreo --url "https://github.com/Nekstoreo/ProjectSynergy/issues/8"
gh project item-add NUMBER --owner Nekstoreo --url "https://github.com/Nekstoreo/ProjectSynergy/issues/9"
```

---

## 🌐 **Configuración Manual desde Web**

### Campo Status (Editar el existente):
1. Ve a: https://github.com/users/Nekstoreo/projects/1
2. Clic en ⚙️ (Settings) → "Fields"
3. Editar campo "Status" existente
4. Configurar opciones: `Backlog, Todo, In Progress, Testing, Review, Done`

### Crear Vistas del Proyecto:
1. **Vista Kanban**: Agrupar por "Status"
2. **Vista Sprint**: Filtrar por "Sprint" actual
3. **Vista Backlog**: Ordenar por "Priority"
4. **Vista Roadmap**: Vista temporal por milestones

### Configurar Automatizaciones:
- Auto-asignar issues a "Todo" cuando se añaden al proyecto
- Mover a "Done" cuando se cierra el issue
- Notificaciones por cambios de estado

---

## 📊 **Verificación Final**

### Lista de verificación:
- [ ] GitHub Project creado
- [ ] Campos personalizados configurados (Status, Priority, Epic, Component, Sprint, Story Points)
- [ ] Labels del repositorio creados
- [ ] 11 Milestones configurados con fechas
- [ ] Issues iniciales creados
- [ ] Issues añadidos al proyecto
- [ ] Vistas del proyecto configuradas

### Comandos de verificación:
```bash
# Listar proyectos
gh project list --owner Nekstoreo

# Listar milestones
gh api repos/Nekstoreo/ProjectSynergy/milestones

# Listar labels
gh label list

# Listar issues
gh issue list
```

---

## 🎯 **Próximos Pasos**

1. **Configurar valores de campos** para cada issue en el proyecto
2. **Organizar tablero Kanban** agrupando por Status
3. **Planificar Sprint 1** asignando issues
4. **Comenzar desarrollo** siguiendo la metodología definida

---

**✅ ¡ProjectSynergy está completamente configurado y listo para el desarrollo!** 