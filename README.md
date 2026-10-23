# Práctica: Preprocesamiento de Estilos con Sass en Ionic & Angular

**Alumno:** Daniel Daroca  
**Asignatura:** Desarrollo de Interfaces (DI) - 2º DAM  
**Curso:** 2026 - 2027  
**Repositorio GitHub:** [https://github.com/danidaroca/ionic-sass-actividad](https://github.com/danidaroca/ionic-sass-actividad)

---

## 📋 Resumen del Proyecto

Este proyecto implementa una aplicación móvil y web construida sobre **Ionic Framework (v8+)** y **Angular (Standalone Components)**, aplicando una arquitectura de estilos modular avanzada mediante el preprocesador **Sass (sintaxis SCSS)** con compilador oficial Dart Sass (`@use`).

Se da cumplimiento exacto a todos los requerimientos estéticos y funcionales solicitados en el enunciado:
1. **Título principal:** Color rojo (`#d32f2f`), transformado a mayúsculas (`text-transform: uppercase`) y alineado al centro (`text-align: center`).
2. **Botón de acción:** Ubicado en el extremo derecho mediante maquetación Flexbox (`justify-content: flex-end`) y estilizado en color azul (`#1976d2`). Implementado tanto con etiqueta HTML estándar (`<button>`) como con componente Ionic (`<ion-button>`).
3. **Imagen destacada:** Centrada en el viewport (`margin: 0 auto; display: block;` y contenedor flexbox) con un marco de borde verde de **3px** de grosor exacto (`border: 3px solid #2e7d32`).
4. **Arquitectura Sass modular:** Basada en el patrón 7-1 (subdirectorio `abstracts/` con `_variables.scss` y `_mixins.scss`), modularidad con la directiva `@use`, anidamiento (`nesting`) y reutilización mediante `@include`.

---

## ❓ Respuestas a las Preguntas Técnicas Planteadas

### 1. ¿El componente usa Sass por defecto?

**Respuesta fundamentada:**
**SÍ.** En Ionic con Angular, el uso de Sass (SCSS) viene preconfigurado y habilitado por defecto a nivel de proyecto y de esquemáticas de generación:

1. **Configuración en `angular.json`:**
   En la raíz del proyecto, el archivo `angular.json` define:
   ```json
   "schematics": {
     "@ionic/angular-toolkit:page": {
       "styleext": "scss",
       "standalone": true
     }
   },
   "inlineStyleLanguage": "scss",
   "styles": ["src/global.scss", "src/theme/variables.scss"]
   ```
   Esto indica al CLI que cualquier página o componente generado (`ionic g page ...` o `ng g c ...`) cree sus hojas de estilo automáticamente con extensión `.scss`.
2. **Componente `HomePage`:**
   Al crear el proyecto con la plantilla `blank`, el componente inicial `src/app/home/home.page.ts` ya viene vinculado de fábrica a `home.page.scss`:
   ```typescript
   @Component({
     selector: 'app-home',
     templateUrl: 'home.page.html',
     styleUrls: ['home.page.scss'],
     imports: [...]
   })
   ```
3. **Estilos globales del framework:**
   Los estilos globales en `src/global.scss` y las variables de diseño en `src/theme/variables.scss` emplean sintaxis Sass de forma nativa.

---

### 2. ¿Aplico Sass pero se compila solo? ¿Por qué no hacemos `npm run watch:sass`?

**Respuesta fundamentada:**
**SÍ, Sass se compila de manera 100% automática y transparente en memoria.**

* **En proyectos planos (Vanilla CSS/Sass como `m9-3-sass-fundamentals`):**  
  No existe un empaquetador ni un servidor de desarrollo integrado. Por esa razón, es necesario instalar el paquete `sass` manualmente en `devDependencies` y definir en `package.json` un script como `"watch:sass": "sass --watch scss:dist/css"` para que Node vigile los archivos y genere físicamente el CSS en disco.

* **En proyectos Ionic con Angular (`ionic serve`):**  
  El comando `ionic serve` delega la ejecución en el servidor de desarrollo de Angular CLI (`ng serve`). El constructor configurado en `angular.json` (`@angular/build:application` con Vite/esbuild) posee un pipeline de construcción integrado con soporte nativo para Dart Sass.  
  Cada vez que guardamos un archivo `.scss`:
  1. Angular detecta el cambio en caliente (Hot Module Replacement - HMR).
  2. Compila el SCSS en memoria a CSS optimizado.
  3. Inyecta los estilos modificados en el navegador en milisegundos sin recargar la página.  
  Por ello, **NO es necesario ni correcto ejecutar `npm run watch:sass`**, ya que el propio ciclo de vida de `ionic serve` se encarga de la compilación continua.

---

## 🏗️ Estructura del Proyecto

```text
Sass/
├── src/
│   ├── app/
│   │   ├── home/
│   │   │   ├── home.page.html       # Plantilla con componentes Ionic y HTML nativo
│   │   │   ├── home.page.scss       # Estilos SCSS con @use, variables, mixins y nesting
│   │   │   ├── home.page.ts         # Componente Standalone de Angular
│   │   │   └── home.page.spec.ts    # Pruebas unitarias
│   │   ├── app.component.ts
│   │   └── app.routes.ts
│   ├── assets/
│   │   └── sass-banner.svg          # Gráfico vectorial SVG para la demostración
│   ├── theme/
│   │   ├── abstracts/
│   │   │   ├── _variables.scss      # Tokens: $title-color, $button-bg, $img-border-...
│   │   │   └── _mixins.scss         # Mixins: @mixin flex-center, flex-align-right, framed-border...
│   │   └── variables.scss           # Variables globales de Ionic
│   ├── global.scss                  # Estilos globales y utilidades de Ionic
│   └── index.html
├── angular.json                     # Configuración de build:application, schematics SCSS
├── ionic.config.json                # Configuración de Ionic CLI (angular-standalone)
├── package.json                     # Dependencias del proyecto
└── README.md                        # Documentación técnica
```

---

## 🚀 Puesta en Marcha

### Prerrequisitos
- Node.js (v18+)
- Ionic CLI (`npm install -g @ionic/cli`)

### Comandos de Ejecución

```bash
# 1. Instalar dependencias
npm install

# 2. Iniciar el servidor de desarrollo (compila Sass automáticamente)
ionic serve

# 3. Compilar el paquete de producción (verifica tipos y compilación Sass)
npm run build
```

---

## 💻 Fragmentos Clave de Código Sass

### `src/theme/abstracts/_variables.scss`
```scss
// Requisitos de la Actividad
$title-color: #d32f2f;              // Rojo para el título principal
$title-transform: uppercase;        // Mayúsculas
$title-align: center;               // Centrado

$button-bg: #1976d2;                // Azul para el botón a la derecha
$button-bg-hover: #1565c0;
$button-text-color: #ffffff;
$button-radius: 8px;

$img-border-width: 3px;             // Grosor exacto de 3px
$img-border-style: solid;
$img-border-color: #2e7d32;         // Verde para el marco de la imagen
$img-border-radius: 12px;
```

### `src/theme/abstracts/_mixins.scss`
```scss
@use 'variables' as v;

@mixin flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}

@mixin flex-align-right {
  display: flex;
  justify-content: flex-end;
  align-items: center;
}

@mixin framed-border($width: v.$img-border-width, $style: v.$img-border-style, $color: v.$img-border-color, $radius: v.$img-border-radius) {
  border: $width $style $color;
  border-radius: $radius;
  box-shadow: 0 6px 18px rgba(46, 125, 50, 0.2);
}
```

### `src/app/home/home.page.scss`
```scss
@use '../../theme/abstracts/variables' as v;
@use '../../theme/abstracts/mixins' as m;

.page-container {
  .assignment-header {
    .assignment-title {
      color: v.$title-color;
      text-transform: v.$title-transform;
      text-align: v.$title-align;
    }
  }

  .button-section {
    @include m.flex-align-right;

    .btn-sass-html {
      @include m.styled-button(v.$button-bg, v.$button-bg-hover, v.$button-text-color);
    }

    .btn-sass-ionic {
      --background: #{v.$button-bg};
      --color: #{v.$button-text-color};
    }
  }

  .image-section {
    .image-center-container {
      @include m.flex-center;

      .framed-banner-img {
        @include m.framed-border;
      }
    }
  }
}
```
