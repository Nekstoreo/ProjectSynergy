# 🛠️ Estándares Técnicos y Convenciones

## 📋 Resumen

Este documento define los estándares técnicos, las convenciones de codificación y las mejores prácticas para el proyecto "Plataforma de Soporte Empresarial 360". Estos estándares aseguran la consistencia, mantenibilidad y calidad profesional del código en todos los componentes.

---

## 🏗️ Estándares de Arquitectura

### **Implementación de Arquitectura Limpia**
```
├── Domain/              # Lógica de negocio central y entidades
├── Application/         # Casos de uso y servicios de aplicación
├── Infrastructure/      # Preocupaciones externas (BD, Correo Electrónico, etc.)
└── Presentation/        # Controladores, DTOs y contratos de API
```

### **Principios de Diseño**
- **Principios SOLID**: Cada clase y método debe seguir los principios SOLID
- **DRY (Don't Repeat Yourself)**: Eliminar la duplicación de código a través de la abstracción
- **YAGNI (You Aren't Gonna Need It)**: Implementar características solo cuando sean necesarias
- **Separación de Preocupaciones**: Cada componente debe tener una única responsabilidad
- **Inversión de Dependencias**: Depender de abstracciones, no de concreciones

### **Diseño Orientado al Dominio (DDD)**
- **Lenguaje Ubicuo**: Usar terminología de negocio de manera consistente
- **Contextos Delimitados**: Límites claros entre diferentes dominios
- **Agregados**: Mantener los límites de consistencia dentro de los agregados
- **Objetos de Valor**: Objetos inmutables para aspectos descriptivos
- **Eventos de Dominio**: Comunicar cambios dentro del dominio

---

## 💻 Estándares de Backend (.NET)

### **Estructura del Proyecto**
```
PlatformSupport.Backend/
├── src/
│   ├── PlatformSupport.Domain/         # Entidades e interfaces de dominio
│   ├── PlatformSupport.Application/    # Casos de uso y DTOs
│   ├── PlatformSupport.Infrastructure/ # Acceso a datos y servicios externos
│   └── PlatformSupport.API/           # Controladores y configuración de API
└── tests/
    ├── PlatformSupport.Domain.Tests/
    ├── PlatformSupport.Application.Tests/
    └── PlatformSupport.API.IntegrationTests/
```

### **Convenciones de Nomenclatura**
- **Clases**: PascalCase (ej., `TicketService`, `UserRepository`)
- **Métodos**: PascalCase (ej., `CreateTicket`, `GetUserById`)
- **Propiedades**: PascalCase (ej., `FirstName`, `CreatedAt`)
- **Campos**: camelCase con prefijo de guion bajo (ej., `_userRepository`)
- **Constantes**: UPPER_SNAKE_CASE (ej., `MAX_TICKET_TITLE_LENGTH`)
- **Interfaces**: PascalCase con prefijo 'I' (ej., `IUserRepository`)

### **Estilo de Código**
```csharp
// ✅ Bueno
public class TicketService : ITicketService
{
    private readonly ITicketRepository _ticketRepository;
    private readonly ILogger<TicketService> _logger;

    public TicketService(
        ITicketRepository ticketRepository,
        ILogger<TicketService> logger)
    {
        _ticketRepository = ticketRepository ?? throw new ArgumentNullException(nameof(ticketRepository));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));
    }

    public async Task<Result<Ticket>> CreateTicketAsync(CreateTicketRequest request)
    {
        try
        {
            // Implementación con manejo de errores adecuado
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Error al crear ticket para el usuario {UserId}", request.UserId);
            return Result<Ticket>.Failure("Error al crear ticket");
        }
    }
}
```

### **Manejo de Errores**
- Usar el patrón `Result<T>` para operaciones que pueden fallar
- Registrar todas las excepciones con el contexto apropiado
- Nunca exponer detalles internos de excepciones a los consumidores de la API
- Usar excepciones personalizadas para errores específicos del dominio

### **Directrices de Async/Await**
- Todas las operaciones de E/S deben ser asíncronas
- Usar `ConfigureAwait(false)` en el código de la biblioteca
- Evitar `async void` excepto para controladores de eventos
- Usar `CancellationToken` para operaciones de larga duración

---

## 🌐 Estándares de Frontend (Next.js/TypeScript)

### **Estructura del Proyecto**
```
frontend/
├── src/
│   ├── app/                    # Next.js App Router
│   ├── components/             # Componentes reutilizables
│   │   ├── ui/                # Componentes básicos de UI (ShadCN)
│   │   └── features/          # Componentes específicos de características
│   ├── lib/                   # Utilidades y configuraciones
│   ├── hooks/                 # Hooks personalizados de React
│   ├── stores/                # Stores de Zustand
│   ├── types/                 # Definiciones de tipos de TypeScript
│   └── utils/                 # Funciones de ayuda
└── public/                    # Activos estáticos
```

### **Convenciones de Nomenclatura**
- **Componentes**: PascalCase (ej., `TicketForm`, `UserProfile`)
- **Archivos**: kebab-case (ej., `ticket-form.tsx`, `user-profile.tsx`)
- **Directorios**: kebab-case (ej., `user-management`, `ticket-system`)
- **Hooks**: camelCase con prefijo 'use' (ej., `useTickets`, `useAuth`)
- **Constantes**: UPPER_SNAKE_CASE (ej., `API_BASE_URL`)

### **Estándares de Componentes**
```typescript
// ✅ Buena Estructura de Componente
interface TicketFormProps {
  onSubmit: (data: CreateTicketData) => Promise<void>;
  initialData?: Partial<CreateTicketData>;
  isLoading?: boolean;
}

export function TicketForm({
  onSubmit,
  initialData,
  isLoading = false
}: TicketFormProps) {
  // Implementación con TypeScript y hooks adecuados
}

// Exportar tipos junto con los componentes
export type { TicketFormProps };
```

### **Gestión de Estado**
- Usar Zustand para el estado global
- Mantener el estado del componente local cuando sea posible
- Usar tipos de TypeScript adecuados para todo el estado

### **Estándares de Estilo**
- Usar TailwindCSS para todo el estilo
- Crear variantes de componentes reutilizables con `cva` (class-variance-authority)
- Seguir el diseño responsivo mobile-first
- Mantener un espaciado consistente usando la escala de Tailwind

---

## 🗄️ Estándares de Base de Datos

### **Convenciones de PostgreSQL**
- **Tablas**: snake_case, plural (ej., `users`, `support_tickets`)
- **Columnas**: snake_case (ej., `first_name`, `created_at`)
- **Claves Primarias**: `id` (UUID preferido)
- **Claves Foráneas**: `{nombre_tabla}_id` (ej., `user_id`)
- **Índices**: `idx_{tabla}_{columna(s)}` (ej., `idx_tickets_status`)

### **Estándares de Migración**
```sql
-- ✅ Buena Estructura de Migración
-- Migración: 001_create_users_table.sql
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) NOT NULL UNIQUE,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE INDEX idx_users_email ON users(email);
```

---

## 📝 Estándares de Documentación

### **Documentación del Código**
- Todas las APIs públicas deben tener documentación XML (C#) o JSDoc (TypeScript)
- La lógica de negocio compleja requiere comentarios en línea
- Las decisiones de arquitectura deben documentarse en ADRs (Architecture Decision Records)

### **Formato de Mensaje de Commit**
Seguir [Conventional Commits](https://www.conventionalcommits.org/):
```
type(scope): descripción

[cuerpo opcional]

[pie(s) de página opcional(es)]
```

**Ejemplos:**
```
feat(api): agregar endpoint de creación de tickets
fix(auth): resolver problema de expiración de token JWT
docs(readme): actualizar instrucciones de instalación
refactor(domain): extraer lógica de validación de tickets
```

**Tipos:**
- `feat`: Nuevas características
- `fix`: Correcciones de errores
- `docs`: Cambios en la documentación
- `refactor`: Refactorización de código
- `test`: Adiciones o modificaciones de pruebas
- `chore`: Tareas de mantenimiento

### **Estándares de Pull Request**
- **Título**: Resumen claro y descriptivo
- **Descripción**: Problema resuelto, enfoque de la solución, pruebas realizadas
- **Etiquetas**: Etiquetas apropiadas para tipo y prioridad
- **Revisores**: Al menos un revisor (o auto-revisión con notas detalladas)
- **Issues Vinculados**: Referenciar issues relacionados

---

## 🧪 Estándares de Prueba

### **Categorías de Prueba**
1. **Pruebas Unitarias**: Probar componentes individuales de forma aislada
2. **Pruebas de Integración**: Probar interacciones entre componentes
3. **Pruebas de Extremo a Extremo (E2E)**: Probar flujos de trabajo completos del usuario
4. **Pruebas de Rendimiento**: Validar los requisitos de rendimiento

### **Nomenclatura de Pruebas**
```csharp
// ✅ Buenos Nombres de Prueba
[Test]
public async Task CreateTicket_WithValidData_ShouldReturnSuccessResult()

[Test]
public async Task CreateTicket_WithEmptyTitle_ShouldReturnValidationError()

[Test]
public async Task CreateTicket_WhenRepositoryThrows_ShouldReturnFailureResult()
```

### **Requisitos de Cobertura**
- **Cobertura Mínima**: 80% en general
- **Cobertura de Rutas Críticas**: 95%
- **Cobertura de Lógica de Dominio**: 90%
- **Cobertura de Endpoints de API**: 85%

---

## 🔒 Estándares de Seguridad

### **Autenticación y Autorización**
- Usar tokens JWT con expiración apropiada
- Implementar control de acceso basado en roles (RBAC)
- Validar todas las entradas en los límites de la API
- Usar HTTPS en todas partes

### **Protección de Datos**
- Hashear contraseñas usando bcrypt o similar
- Sanitizar todas las entradas de usuario
- Usar consultas parametrizadas (prevenir inyección SQL)
- Implementar limitación de tasa en las APIs

### **Gestión de Secretos**
- Nunca commitear secretos al repositorio
- Usar variables de entorno para la configuración
- Usar almacenamiento seguro de secretos en producción

---

## 📊 Estándares de Rendimiento

### **Rendimiento de API**
- **Tiempo de Respuesta**: <200ms para el percentil 95
- **Consultas de Base de Datos**: Optimizar problemas de consulta N+1
- **Caché**: Implementar estrategias de caché apropiadas
- **Paginación**: Requerido para endpoints de lista

### **Rendimiento de Frontend**
- **Core Web Vitals**: Cumplir con las recomendaciones de Google
- **Tamaño del Bundle**: Optimizar los tamaños de los bundles de JavaScript
- **Optimización de Imágenes**: Usar el componente `Image` de Next.js
- **Carga Perezosa (Lazy Loading)**: Implementar para contenido no crítico

---

## 🔍 Herramientas de Calidad de Código

### **Análisis Automatizado**
- **SonarCloud**: Análisis de calidad y seguridad del código
- **ESLint**: Linting de JavaScript/TypeScript
- **Prettier**: Formateo de código
- **Husky**: Hooks de Git para comprobaciones de calidad

### **Puertas de Calidad**
- Sin problemas críticos o bloqueadores en SonarCloud
- Todas las pruebas pasan en CI/CD
- La cobertura de código cumple con los requisitos mínimos
- Sin errores de TypeScript
- Todas las reglas de linting pasan

---

## 📚 Recursos de Aprendizaje

### **Arquitectura**
- [Arquitectura Limpia de Robert C. Martin](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)
- [Referencia de Diseño Orientado al Dominio](https://www.domainlanguage.com/ddd/reference/)

### **.NET**
- [Directrices de Microsoft .NET](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/)
- [Mejores Prácticas de ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/best-practices)

### **Frontend**
- [Documentación de Next.js](https://nextjs.org/docs)
- [Mejores Prácticas de React](https://react.dev/learn)
- [Manual de TypeScript](https://www.typescriptlang.org/docs/)

---

## ✅ Lista de Verificación de Cumplimiento

Antes de cualquier fusión de código, asegurarse de que:
- [ ] El código sigue las convenciones de nomenclatura
- [ ] Todas las pruebas pasan
- [ ] La cobertura de código cumple con los requisitos
- [ ] La documentación está actualizada
- [ ] Se siguen las directrices de seguridad
- [ ] Se cumplen los estándares de rendimiento
- [ ] Los mensajes de commit siguen la convención
- [ ] No hay secretos en el código
- [ ] El manejo de errores está implementado
- [ ] El registro se añade donde sea apropiado

---

*Estos estándares son documentos vivos y se actualizarán a medida que el proyecto evolucione y se identifiquen nuevas mejores prácticas.*