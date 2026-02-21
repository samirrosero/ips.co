# IPS.Co - Portafolio

Un sitio web de portafolio construido con [Astro](https://astro.build), optimizado para rendimiento y fácil mantenimiento.

## Requisitos Previos

Antes de comenzar, asegúrate de tener instalados:

- **Node.js** (versión 16 o superior) - [Descargar](https://nodejs.org/)
- **npm** (generalmente incluído con Node.js)

Para verificar que tienes las versiones correctas:

```sh
node --version
npm --version
```

## 🚀 Instalación Rápida

### 1. Clonar el repositorio

```sh
git clone <URL_DEL_REPOSITORIO>
cd ips.co
```

### 2. Instalar dependencias

```sh
npm install
```

### 3. Ejecutar el servidor de desarrollo

```sh
npm run dev
```

El sitio estará disponible en `http://localhost:4321`

## 📝 Comandos Disponibles

Todos los comandos se ejecutan desde la raíz del proyecto:

| Comando                   | Descripción                                      |
| :------------------------ | :----------------------------------------------- |
| `npm install`             | Instala todas las dependencias                   |
| `npm run dev`             | Inicia servidor de desarrollo en `localhost:4321` |
| `npm run build`           | Compila el sitio para producción en `./dist/`   |
| `npm run preview`         | Previsualiza la versión de producción           |
| `npm run astro ...`       | Ejecuta comandos de Astro CLI                    |
| `npm run astro -- --help` | Obtiene ayuda del Astro CLI                      |

## � Estructura del Proyecto

```
src/
├── components/       # Componentes Astro reutilizables
├── content/          # Contenido en Markdown (portafolio)
├── layouts/          # Plantillas de diseño
├── pages/            # Páginas del sitio (rutas)
└── styles/           # Estilos CSS globales
public/               # Archivos estáticos (imágenes, assets)
```

## 🛠️ Stack Tecnológico

- **Astro 5.17.1** - Framework estático moderno
- **CSS** - Estilos personalizados
- **Markdown** - Contenido del portafolio

## 📚 Recursos Útiles

- [Documentación de Astro](https://docs.astro.build)
- [Discord Community](https://astro.build/chat)

## 💡 Despliegue

Para desplegar el sitio:

1. Ejecuta `npm run build` para generar los archivos estáticos
2. Sube el contenido de la carpeta `dist/` a tu servidor

---

¿Preguntas o problemas? Revisa la documentación de Astro o contacta al equipo de desarrollo.
