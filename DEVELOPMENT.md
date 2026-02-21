# Guía de Desarrollo - IPS.Co

Documento que describe cómo trabajaremos en el proyecto, la estructura, convenciones y configuración del entorno.

## 📋 Índice

1. [Flujo de Trabajo](#-flujo-de-trabajo)
2. [Estructura del Proyecto](#-estructura-del-proyecto)
3. [Frontend](#-frontend)
4. [Backend](#-backend)
5. [Componentes](#-componentes)
6. [Variables de Entorno](#-variables-de-entorno)
7. [Git Workflow](#-git-workflow)
8. [Buenas Prácticas](#-buenas-prácticas)

---

## 🔄 Flujo de Trabajo

### Proceso Diario

1. **Sincronización** - Comienza el día haciendo pull de los últimos cambios
   ```bash
   git pull origin main
   npm install  # Si hay cambios en package.json
   ```

2. **Branch para tu tarea** - Crea una rama de trabajo
   ```bash
   git checkout -b feature/nombre-de-la-tarea
   ```

3. **Desarrollo** - Realiza los cambios en tu rama local
   ```bash
   npm run dev  # Servidor de desarrollo
   ```

4. **Testing** - Verifica tus cambios localmente
   ```bash
   npm run build  # Prueba la compilación
   ```

5. **Commit y Push**
   ```bash
   git add .
   git commit -m "feat: descripción clara del cambio"
   git push origin feature/nombre-de-la-tarea
   ```

6. **Pull Request** - Abre un PR para que se revisen los cambios
   - Describe qué cambios realizaste
   - Adjunta screenshots si hay cambios visuales
   - Solicita revisión a compañeros

7. **Merge** - Una vez aprobado, mezcla en `main`

---

## 📁 Estructura del Proyecto

```
ips.co/
├── src/
│   ├── components/          # Componentes reutilizables
│   │   ├── common/          # Componentes comunes (Nav, Footer, etc.)
│   │   ├── ui/              # Componentes de UI (botones, inputs, etc.)
│   │   ├── sections/        # Componentes de secciones grandes
│   │   └── layout/          # Componentes de layout
│   ├── pages/               # Páginas del sitio (rutas)
│   │   ├── api/             # Endpoints del backend (si aplica)
│   │   └── [rutas]/         # Rutas dinámicas
│   ├── content/             # Contenido estático (Markdown)
│   │   └── work/            # Trabajos/proyectos
│   ├── layouts/             # Plantillas de página
│   ├── styles/              # Estilos CSS globales
│   ├── utils/               # Funciones utilitarias
│   │   ├── api.ts           # Funciones para llamadas HTTP
│   │   ├── helpers.ts       # Funciones helper
│   │   └── constants.ts     # Constantes de la aplicación
│   ├── types/               # Tipos TypeScript compartidos
│   └── env.d.ts             # Tipos de variables de entorno
├── public/                  # Archivos estáticos
│   └── assets/              # Imágenes, íconos, etc.
├── .env                     # Variables de entorno (NO VERSIONAR)
├── .env.example             # Plantilla de .env
├── astro.config.mjs         # Configuración de Astro
├── tsconfig.json            # Configuración de TypeScript
└── package.json             # Dependencias del proyecto
```

---

## 🎨 Frontend

### Ubicación
```
src/
├── components/              # Componentes Astro/React
├── pages/                   # Rutas del sitio
└── styles/                  # Estilos CSS
```

### Convenciones

**Nombres de Componentes:**
- Usar PascalCase: `Hero.astro`, `NavigationBar.astro`
- Ser descriptivos: `PortfolioCard.astro` en lugar de `Card.astro`

**Estructura de Carpetas:**
```
src/components/
├── common/
│   ├── Nav.astro
│   ├── Footer.astro
│   └── Header.astro
├── ui/
│   ├── Button.astro
│   ├── Card.astro
│   └── Modal.astro
└── sections/
    ├── HeroSection.astro
    ├── FeaturesSection.astro
    └── CtaSection.astro
```

**Props en Componentes:**
```typescript
interface Props {
  title: string;
  description?: string;
  variant?: 'primary' | 'secondary';
}

const { title, description, variant = 'primary' } = Astro.props;
```

### Estilos

- Usar CSS Modules o CSS global en `src/styles/`
- Nombrar clases con kebab-case: `.hero-section`, `.cta-button`
- Variables CSS para colores y tamaños

---

## 🔧 Backend

### Ubicación
```
src/pages/api/
├── health.ts                # Verificación de salud
├── contact.ts               # Endpoint de contacto
└── portfolio/
    └── works.json.ts        # Endpoint para obras
```

### Convenciones

**Endpoints:**
- Usar nombres descriptivos: `/api/contact`, `/api/portfolio/works`
- Métodos RESTful: GET, POST, PUT, DELETE
- Respuestas consistentes

**Ejemplo de Endpoint:**
```typescript
// src/pages/api/contact.ts
export async function post(context: APIContext) {
  const body = await context.request.json();
  
  // Validar datos
  if (!body.email || !body.message) {
    return new Response(
      JSON.stringify({ error: 'Email y mensaje son requeridos' }),
      { status: 400 }
    );
  }
  
  // Procesar solicitud
  try {
    // Lógica aquí
    return new Response(
      JSON.stringify({ success: true, message: 'Enviado correctamente' }),
      { status: 200 }
    );
  } catch (error) {
    return new Response(
      JSON.stringify({ error: 'Error al procesar la solicitud' }),
      { status: 500 }
    );
  }
}
```

---

## 🧩 Componentes

### Estructura de un Componente

```astro
---
import Icon from './Icon.astro';

interface Props {
  title: string;
  description: string;
  icon?: string;
}

const { title, description, icon } = Astro.props;
---

<div class="card">
  {icon && <Icon name={icon} />}
  <h3>{title}</h3>
  <p>{description}</p>
</div>

<style>
  .card {
    padding: 1rem;
    border-radius: 0.5rem;
    background: var(--color-surface);
  }
  
  h3 {
    margin: 0.5rem 0;
    font-size: 1.25rem;
  }
</style>
```

### Reutilización de Componentes

- Crear componentes pequeños y específicos
- Pasar props para hacer componentes flexibles
- Evitar lógica compleja dentro de componentes
- Usar slots para contenido dinámico

```astro
<!-- Componente: Card.astro -->
<div class="card">
  <slot name="header" />
  <slot />
  <slot name="footer" />
</div>

<!-- Uso -->
<Card>
  <Fragment slot="header">
    <h3>Título</h3>
  </Fragment>
  <p>Contenido de la tarjeta</p>
  <Fragment slot="footer">
    <button>Ver más</button>
  </Fragment>
</Card>
```

---

## 🔐 Variables de Entorno

### Archivo `.env` (NO VERSIONAR)

Crear un archivo `.env` en la raíz del proyecto:

```bash
# Base de datos (si aplica)
DATABASE_URL=http://localhost:5432

# APIs Externas
API_KEY_EXTERNAL_SERVICE=tu_clave_aqui
API_URL_CONTACT=http://localhost:3001

# Frontend
PUBLIC_API_BASE_URL=http://localhost:3000
PUBLIC_SITE_URL=http://localhost:4321
PUBLIC_GTM_ID=GTM-XXXXX

# Backend (si es necesario)
PORT=3001
NODE_ENV=development

# Email (si usas un servicio)
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=tu_email@gmail.com
SMTP_PASSWORD=tu_password

# Secrets
SECRET_KEY=tu_clave_secreta_aqui
```

### Archivo `.env.example` (VERSIONAR)

Crear plantilla sin valores sensibles:

```bash
# Base de datos (si aplica)
DATABASE_URL=

# APIs Externas
API_KEY_EXTERNAL_SERVICE=
API_URL_CONTACT=

# Frontend
PUBLIC_API_BASE_URL=http://localhost:3000
PUBLIC_SITE_URL=http://localhost:4321
PUBLIC_GTM_ID=

# Backend (si es necesario)
PORT=3001
NODE_ENV=development

# Email (si usas un servicio)
SMTP_HOST=
SMTP_PORT=
SMTP_USER=
SMTP_PASSWORD=

# Secrets
SECRET_KEY=
```

### Acceso a Variables en Astro

```typescript
// Variables públicas (cliente)
const apiUrl = import.meta.env.PUBLIC_API_BASE_URL;

// Variables privadas (servidor)
const dbUrl = import.meta.env.DATABASE_URL;
```

### Validación de Variables

```typescript
// src/env.d.ts
interface ImportMetaEnv {
  readonly PUBLIC_API_BASE_URL: string;
  readonly PUBLIC_SITE_URL: string;
  readonly DATABASE_URL: string;
}

interface ImportMeta {
  readonly env: ImportMetaEnv;
}
```

---

## 🌿 Git Workflow

### Ramas

- `main` - Producción, código estable
- `develop` - Desarrollo, siguiente versión
- `feature/*` - Nuevas características
- `bugfix/*` - Correcciones de bugs
- `hotfix/*` - Arreglos urgentes en producción

### Naming Convention de Ramas

```bash
feature/agregar-seccion-portafolio
feature/integrar-api-contacto
bugfix/corregir-responsive-mobile
hotfix/arreglar-fallo-produccion
```

### Commits

Usar Conventional Commits:

```bash
feat: agregar nueva sección de portafolio
fix: corregir responsive en móvil
docs: actualizar guía de desarrollo
style: formatear código
refactor: optimizar componente Card
test: agregar tests para validación
chore: actualizar dependencias
```

---

## ✅ Buenas Prácticas

### Código

- ✅ Usar TypeScript para mayor seguridad
- ✅ Documentar componentes complejos
- ✅ Mantener componentes pequeños y enfocados
- ✅ Evitar props drilling profundo
- ✅ Usar variables CSS para temas
- ✅ Hacer commits pequeños y descriptivos

### Rendimiento

- ✅ Optimizar imágenes
- ✅ Lazy load de componentes
- ✅ Minificar CSS y JavaScript
- ✅ Usar Astro Islands para interactividad selectiva

### Seguridad

- ✅ Nunca commitear `.env`
- ✅ Validar datos en backend
- ✅ Usar variables de entorno para secrets
- ✅ Sanitizar inputs de usuarios

### Colaboración

- ✅ Revisar pull requests con cuidado
- ✅ Probar cambios de otros localmente
- ✅ Comunicar cambios importantes
- ✅ Mantener la rama `main` siempre funcional

---

## 📞 Contacto y Dudas

Si tienes dudas sobre el flujo de trabajo o la estructura:

- Revisa la [Documentación de Astro](https://docs.astro.build)
- Consulta con el equipo en las daily meetings
- Crea un ticket si encuentras un problema

---

**Última actualización:** 21 de febrero de 2026
