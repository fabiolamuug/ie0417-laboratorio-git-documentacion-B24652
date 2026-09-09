# Informe - Laboratorio 1

**Curso:** IE0417 - Diseño de Software para Ingeniería  
**Estudiante:** Fabiola Muñoz  
**Carné:** B24652  

## 1. Introducción

Este laboratorio integra tres áreas principales del curso: el uso de Git como
sistema distribuido de control de versiones, la generación automática de
documentación técnica para proyectos de software y la publicación de dicha
documentación como contenido web estático.

La primera parte se desarrolló mediante Learn Git Branching, utilizando sus
niveles locales y remotos para practicar operaciones sobre ramas, referencias,
historial y repositorios remotos. Posteriormente se seleccionaron dos proyectos
de software libre de tamaño significativo: `SFML` como proyecto C++ para
documentación con Doxygen y el subsistema `pandas/io` como proyecto Python para
documentación con Sphinx.

Además de generar documentación, el laboratorio busca analizar qué información
puede obtener cada herramienta del código fuente, qué depende de documentación
escrita por los desarrolladores y qué utilidad tiene el resultado para una
persona que se incorpora por primera vez a un proyecto.

Para evitar duplicar información, este archivo presenta el análisis integrador
del laboratorio. El detalle técnico de cada parte se conserva junto a sus
respectivos artefactos:

- Git: [`git/learn-git-branching.md`](git/learn-git-branching.md)
- Doxygen: [`doxygen/informe_doxygen.md`](doxygen/informe_doxygen.md)
- Sphinx: `sphinx/informe_sphinx.md` *(pendiente)*

---

## 2. Parte I - Git y Learn Git Branching

La primera parte del laboratorio consistió en resolver y documentar los niveles
de Learn Git Branching relacionados con operaciones locales y remotas.

Durante los ejercicios se trabajó con conceptos como:

- creación y movimiento entre ramas;
- `merge` y `rebase`;
- referencias relativas;
- `HEAD` separado;
- `reset` y `revert`;
- `cherry-pick`;
- tags;
- ramas locales y remotas;
- `fetch`, `pull` y `push`;
- seguimiento de ramas remotas;
- efectos de la reescritura del historial.

La documentación completa de los niveles, los comandos ejecutados, sus efectos
sobre el historial, las evidencias, la síntesis conceptual y las respuestas al
análisis obligatorio se encuentran en:

[`git/learn-git-branching.md`](git/learn-git-branching.md)

Esta primera parte permitió practicar los conceptos de Git en un entorno
controlado antes de aplicarlos sobre el repositorio real del laboratorio. En el
repositorio de entrega se utilizaron ramas de trabajo separadas para cambios
sustantivos y commits asociados a unidades de trabajo identificables, evitando
dividir artificialmente los cambios únicamente para aumentar el número de
commits.

---

## 3. Selección de proyectos

### 3.1 Proyecto C++: SFML

Para la documentación C++ se seleccionó **SFML (Simple and Fast Multimedia
Library)**, una biblioteca multimedia escrita principalmente en C++ que ofrece
funcionalidades de gráficos, ventanas, audio, sistema y redes.

**Repositorio:** <https://github.com/SFML/SFML>  
**Licencia:** zlib/libpng  
**Commit analizado:**  
`2124d5fe87412ac1cf8e20de282502a75039efcf`

El alcance utilizado para las métricas y la documentación fue:

```text
include/SFML
src/SFML
```

La medición mediante `cloc`, considerando extensiones C/C++ relevantes, produjo
347 archivos procesados y 38 266 líneas de código. Un conteo adicional basado
en extensiones encontró 351 archivos relevantes. En ambos casos se superan los
mínimos requeridos por el laboratorio.

SFML fue considerado apropiado para Doxygen debido a la presencia de comentarios
estructurados, una jerarquía de clases significativa, múltiples namespaces,
módulos funcionales y relaciones de herencia que pueden representarse mediante
Graphviz.

El detalle de la selección se encuentra en:

[`doxygen/seleccion.md`](doxygen/seleccion.md)

### 3.2 Proyecto Python: pandas/io

Para la documentación Python se seleccionó el subsistema **`pandas/io`** del
proyecto pandas.

**Repositorio:** <https://github.com/pandas-dev/pandas>  
**Licencia:** BSD 3-Clause  
**Commit seleccionado:**  
`c26625d6e79fe8f11dd00184a3c32b5cc68d70cc`

El subsistema contiene componentes relacionados con entrada y salida de datos,
incluyendo CSV, JSON, Excel, SQL, Parquet, HTML y XML.

La medición realizada sobre `pandas/io` produjo 56 archivos Python y 28 459
líneas de código, por lo que cumple los umbrales requeridos.

La generación y el análisis con Sphinx se desarrollarán en la siguiente etapa
del laboratorio.

---

## 4. Documentación C++ con Doxygen

La documentación de SFML se generó mediante una configuración de Doxygen creada
específicamente para este laboratorio. No se utilizó directamente el `Doxyfile`
del proyecto original.

El proceso técnico completo, las decisiones de configuración, las iteraciones,
el análisis de las advertencias y las evidencias se documentan en:

[`doxygen/informe_doxygen.md`](doxygen/informe_doxygen.md)

### 4.1 Proceso y configuración

La configuración inicial se creó mediante:

```powershell
doxygen -g .\doxygen\Doxyfile
```

A partir de esta plantilla se configuraron como entradas `include/SFML`,
`src/SFML` y una página principal creada específicamente para el laboratorio.

La salida se generó en:

```text
site/cpp/
```

También se habilitaron:

- navegación jerárquica;
- exploración del código fuente;
- referencias cruzadas;
- búsqueda;
- diagramas mediante Graphviz;
- generación de gráficos en formato SVG.

Una primera generación produjo una navegación demasiado cargada con entidades
internas y numerosas advertencias relacionadas con elementos no documentados.
En lugar de desactivar las advertencias, se ajustó la política de extracción y
presentación mediante opciones como:

```ini
EXTRACT_LOCAL_CLASSES = NO
HIDE_UNDOC_MEMBERS    = YES
HIDE_UNDOC_CLASSES    = YES
WARN_IF_UNDOCUMENTED  = YES
```

El objetivo fue reducir ruido en la documentación visible sin ocultar posibles
problemas de documentación del proyecto.

### 4.2 Organización y navegación

La documentación final permite navegar SFML mediante:

- módulos;
- clases;
- estructuras;
- namespaces;
- archivos;
- funciones;
- jerarquías de clases;
- código fuente.

La página principal identifica el proyecto, la versión analizada, el commit, el
alcance utilizado y el repositorio original.

SFML también utiliza agrupaciones funcionales que Doxygen representa como
módulos. Por ejemplo, el módulo Graphics agrupa entidades como
`sf::RenderWindow`, `sf::Sprite`, `sf::Texture` y `sf::Shader`.

Esto permite explorar la biblioteca por funcionalidad y no únicamente mediante
la estructura física de archivos.

### 4.3 Clases, estructuras, namespaces, archivos y funciones

Para las clases, Doxygen genera información que incluye:

- descripción;
- constructores;
- métodos;
- miembros;
- métodos heredados;
- relaciones con otras clases.

La página de `sf::RenderWindow`, por ejemplo, presenta su relación con
`sf::Window` y `sf::RenderTarget`.

Los namespaces también agrupan estructuras y funciones relacionadas. En
`sf::Dns`, por ejemplo, se documentan estructuras como `MxRecord` y `SrvRecord`
y funciones como `resolve`, `queryNs`, `queryMx`, `querySrv` y `queryTxt`.

La navegación por archivos permite además acceder al código fuente con
numeración de líneas y resaltado de sintaxis.

### 4.4 Parámetros, valores de retorno, miembros y relaciones

Los comentarios estructurados presentes en SFML permiten que Doxygen genere
secciones específicas para parámetros, retornos, notas y referencias.

En la documentación de `sf::Sprite`, por ejemplo, pueden observarse métodos con
información detallada de parámetros y relaciones de tipo `References` y
`Referenced by`.

Las relaciones de herencia y colaboración permiten complementar esa información
textual con la estructura del diseño orientado a objetos.

### 4.5 Diagramas y referencias cruzadas

Graphviz se utilizó para generar diagramas de:

- herencia;
- colaboración;
- inclusión de archivos;
- archivos que incluyen otros archivos.

Uno de los ejemplos representativos es `sf::RenderWindow`, cuya documentación
muestra gráficamente sus relaciones con otras clases de SFML.

Las referencias cruzadas y la navegación hacia el código permiten pasar de la
descripción de una entidad a su implementación o a otras entidades relacionadas.

### 4.6 Información proveniente de comentarios y del código

No toda la información mostrada por Doxygen tiene el mismo origen.

Los comentarios estructurados escritos por los desarrolladores aportan, entre
otros elementos:

- explicaciones descriptivas;
- parámetros;
- valores de retorno;
- notas;
- referencias;
- agrupaciones funcionales.

Por otra parte, Doxygen puede inferir directamente del código:

- firmas de funciones;
- clases y estructuras;
- namespaces;
- enumeraciones;
- relaciones de herencia;
- ubicación de archivos;
- algunas referencias entre entidades.

Esta diferencia se hizo especialmente visible durante las iteraciones de
configuración. La primera salida mostraba muchas entidades internas que Doxygen
podía identificar sintácticamente aunque no estuvieran documentadas como parte
de la API destinada a sus usuarios.

### 4.7 Utilidad para una persona desarrolladora nueva

Una persona que se incorpora por primera vez al proyecto puede utilizar la
documentación para comenzar desde una vista funcional general y avanzar hacia
detalles concretos.

Por ejemplo, alguien interesado en el subsistema gráfico puede entrar al módulo
Graphics, identificar `RenderWindow`, `Sprite` y `Texture`, revisar sus métodos
y posteriormente explorar relaciones de herencia, referencias y archivos
fuente.

La combinación de documentación textual, API, diagramas y navegador de código
facilita comprender tanto el uso de la biblioteca como parte de su organización
interna.

### 4.8 Advertencias y elementos incompletos

La generación final conserva **12 advertencias**.

Estas se agrupan principalmente en:

1. funciones de `sf::Dns` cuya definición no puede asociarse de forma exacta
   con la declaración;
2. implementaciones de `JoystickImpl::update()` específicas de FreeBSD, NetBSD
   y Unix que producen asociaciones ambiguas;
3. el tipo `sf::WindowHandle`, cuyo significado depende de condiciones de
   plataforma;
4. parámetros o miembros con documentación incompleta.

Durante una iteración anterior también apareció una advertencia por el comando
personalizado `\sfplatform`. Al revisar la configuración original de SFML se
determinó que correspondía a un alias propio del proyecto. Se incorporó
únicamente esa definición a la configuración creada para el laboratorio, lo que
redujo el resultado de 13 a 12 advertencias.

Las advertencias restantes no fueron ocultadas y permanecen registradas en:

`doxygen/build.log`

El análisis detallado de cada categoría se encuentra en
[`doxygen/informe_doxygen.md`](doxygen/informe_doxygen.md).

### 4.9 Evidencias

Las evidencias representativas se encuentran en:

`doxygen/evidencias/`

Entre ellas se incluyen:

- página principal;
- diagramas de `sf::RenderWindow`;
- navegador de código de `RenderWindow.hpp`;
- módulo Graphics;
- API de `sf::Sprite`;
- documentación detallada de `sf::Sprite`;
- búsqueda interna;
- lista de clases utilizada durante el proceso de ajuste de la política de
  extracción.

---

## 5. Documentación Python con Sphinx

Pendiente de desarrollo.

La documentación técnica detallada de esta parte se registrará en:

`sphinx/informe_sphinx.md`

El análisis integrado se incorporará a esta sección una vez finalizada la
generación de la documentación.

---

## 6. Comparación Doxygen vs. Sphinx

Pendiente hasta completar la documentación con Sphinx.

La comparación incluirá al menos las siguientes dimensiones:

| Dimensión | Doxygen en C++ | Sphinx en Python |
|---|---|---|
| Fuente principal de la información | Pendiente | Pendiente |
| Configuración y proceso de generación | Pendiente | Pendiente |
| Organización y navegación | Pendiente | Pendiente |
| Documentación de API | Pendiente | Pendiente |
| Diagramas y referencias cruzadas | Pendiente | Pendiente |
| Contenido narrativo | Pendiente | Pendiente |
| Facilidad para documentar un proyecto existente | Pendiente | Pendiente |
| Utilidad para una persona nueva en el proyecto | Pendiente | Pendiente |

---

## 7. Publicación y verificación

Pendiente.

La estructura prevista para la publicación es:

```text
site/
├── index.html
├── cpp/
└── python/
```

La verificación final deberá comprobar que:

- la portada sea accesible públicamente;
- `/cpp/` cargue la documentación Doxygen;
- `/python/` cargue la documentación Sphinx;
- funcionen hojas de estilo y JavaScript;
- funcionen imágenes y diagramas;
- funcione la búsqueda;
- funcionen los enlaces internos;
- el contenido sea accesible sin una sesión iniciada.

Las URL y la fecha de verificación se registrarán al finalizar el despliegue.

---

## 8. Conclusiones

Pendiente de completar una vez finalizadas las etapas de Sphinx, comparación y
publicación.
