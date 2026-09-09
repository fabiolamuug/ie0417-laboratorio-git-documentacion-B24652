# Informe técnico - Documentación de SFML con Doxygen

**Curso:** IE0417 - Diseño de Software para Ingeniería  
**Laboratorio:** Laboratorio 1  
**Estudiante:** Fabiola Muñoz  
**Carné:** B24652  

## 1. Objetivo

El objetivo de esta etapa fue generar documentación técnica HTML para un
proyecto C++ de software libre utilizando una configuración propia de Doxygen.

El proyecto seleccionado fue SFML. La intención no fue únicamente ejecutar
Doxygen sobre código previamente documentado, sino estudiar el proyecto,
definir un alcance apropiado, diseñar una configuración adecuada para ese
alcance, evaluar iterativamente la calidad de la salida y ajustar la
configuración según los resultados observados.

El detalle de la selección, licencia, métricas y commit analizado se encuentra
en:

[`seleccion.md`](seleccion.md)

---

## 2. Proyecto y alcance analizado

El proyecto documentado fue **SFML (Simple and Fast Multimedia Library)**.

**Repositorio:** <https://github.com/SFML/SFML>  
**Commit analizado:**  
`2124d5fe87412ac1cf8e20de282502a75039efcf`

El proyecto declara una versión de desarrollo 3.2.0, por lo que para la
documentación generada se utilizó:

```text
SFML 3.2.0-dev
```

El alcance seleccionado fue:

```text
include/SFML
src/SFML
```

Esto permite analizar tanto las declaraciones de la API pública como parte de
las implementaciones que hacen posible explorar relaciones internas y navegar
hacia el código fuente.

Se excluyeron del alcance elementos como:

- pruebas;
- ejemplos;
- dependencias externas;
- archivos generados.

---

## 3. Proceso de generación

La documentación no se generó utilizando directamente el `Doxyfile` incluido en
el repositorio de SFML.

Se creó una configuración independiente mediante:

```powershell
doxygen -g .\doxygen\Doxyfile
```

A partir de esa plantilla se realizaron varias iteraciones.

### 3.1 Definición inicial del proyecto

Primero se configuraron:

- nombre;
- versión;
- descripción breve;
- directorios de entrada;
- tipos de archivos;
- salida HTML;
- página principal.

También se decidió generar el sitio dentro de:

```text
site/cpp/
```

para que pudiera integrarse posteriormente con la estructura de publicación del
laboratorio.

### 3.2 Página principal

Se creó:

```text
doxygen/mainpage.md
```

como página principal propia.

Esta página contiene:

- identificación de SFML;
- propósito de la documentación;
- commit analizado;
- alcance utilizado;
- atribución al proyecto original;
- enlace al repositorio.

Se configuró mediante:

```ini
USE_MDFILE_AS_MAINPAGE = doxygen/mainpage.md
```

### 3.3 Primera generación

La documentación se generó inicialmente mediante:

```powershell
doxygen .\doxygen\Doxyfile
```

Posteriormente se utilizó redirección mediante `cmd` para conservar un log sin
las estructuras adicionales que PowerShell agrega al manejar la salida estándar
de error de programas nativos:

```powershell
cmd /c "doxygen .\doxygen\Doxyfile > .\doxygen\build.log 2>&1"
```

La primera salida generó correctamente el HTML, pero presentó dos problemas
principales:

1. una cantidad muy alta de advertencias relacionadas con entidades internas;
2. una lista de clases demasiado extensa y cargada de detalles de
   implementación.

La evidencia `evidencias/08-classlist.png` fue utilizada durante esta etapa para
evaluar el efecto de la configuración.

---

## 4. Decisiones de configuración del `Doxyfile`

## 4.1 Identificación del proyecto

Se configuró:

```ini
PROJECT_NAME   = "SFML"
PROJECT_NUMBER = "3.2.0-dev"
PROJECT_BRIEF  = "Simple and Fast Multimedia Library - documentation generated for IE0417 Lab 1"
```

La versión se determinó a partir del archivo `CMakeLists.txt` del proyecto y del
estado del repositorio analizado.

La utilización de `3.2.0-dev` permite distinguir la documentación generada de
una versión estable publicada.

---

## 4.2 Directorios de entrada

Se configuró:

```ini
INPUT = ../lab1-projects/SFML/include/SFML \
        ../lab1-projects/SFML/src/SFML \
        doxygen/mainpage.md

RECURSIVE = YES
```

`include/SFML` contiene gran parte de la interfaz pública de la biblioteca.

`src/SFML` contiene implementaciones que permiten explorar relaciones entre las
declaraciones públicas y la implementación.

Se mantuvieron ambos directorios porque limitar Doxygen únicamente a los headers
habría producido una documentación más limpia, pero habría reducido la
posibilidad de analizar referencias cruzadas y navegar por la implementación.

`RECURSIVE = YES` fue necesario debido a la estructura jerárquica de SFML, que
contiene subsistemas como:

- Audio;
- Graphics;
- Network;
- System;
- Window.

---

## 4.3 Patrones de archivos

Se utilizaron patrones relacionados con C++:

```ini
FILE_PATTERNS = *.h \
                *.hpp \
                *.hh \
                *.cc \
                *.cpp \
                *.cxx \
                *.md
```

El objetivo fue limitar el análisis a archivos relevantes para la documentación
técnica seleccionada.

El patrón `*.md` también permite incorporar la página principal escrita
manualmente.

---

## 4.4 Exclusiones

Se utilizaron exclusiones orientadas a evitar contenido fuera del alcance del
laboratorio:

```ini
EXCLUDE_PATTERNS = */third_party/* \
                   */tests/* \
                   */examples/*
```

Las dependencias de terceros no forman parte del código que se pretende analizar
y documentar.

Las pruebas y ejemplos pueden ser útiles en la documentación oficial del
proyecto, pero no eran necesarios para cumplir el alcance definido en este
laboratorio.

---

## 4.5 Política de extracción

Una de las decisiones más relevantes ocurrió después de evaluar la primera
generación.

La salida inicial incluía muchas entidades internas que Doxygen detectaba a
partir del código aunque su presencia aportaba poco a una documentación centrada
en comprender la API del proyecto.

La lista observada durante esta etapa se conserva como:

```text
evidencias/08-classlist.png
```

Para mejorar la navegación se configuró:

```ini
EXTRACT_ALL           = NO
EXTRACT_PRIVATE       = NO
EXTRACT_STATIC        = NO
EXTRACT_LOCAL_CLASSES = NO

HIDE_UNDOC_MEMBERS    = YES
HIDE_UNDOC_CLASSES    = YES
```

Estas opciones no modifican SFML ni eliminan código del análisis.

Su efecto se limita a la política utilizada para construir la documentación
visible.

### `EXTRACT_ALL = NO`

Se decidió no forzar la extracción de absolutamente todas las entidades
detectables.

Esto permite que las entidades explícitamente documentadas por SFML tengan mayor
peso en la documentación resultante.

También hace posible analizar la diferencia entre:

- información escrita intencionalmente como documentación;
- información que Doxygen puede inferir únicamente del código.

### `EXTRACT_PRIVATE = NO`

Los miembros privados forman parte de la implementación interna y normalmente no
representan la interfaz que una persona usuaria de la biblioteca necesita
consultar.

Se decidió no mostrarlos de manera general para reducir ruido.

### `EXTRACT_STATIC = NO`

Los elementos estáticos internos tampoco fueron considerados prioritarios para
el objetivo de la documentación.

### `EXTRACT_LOCAL_CLASSES = NO`

Esta opción evita incluir clases cuyo alcance se limita a implementaciones
locales.

Durante la primera generación este tipo de entidades aumentaba
considerablemente la cantidad de elementos visibles.

### `HIDE_UNDOC_MEMBERS = YES`

Los miembros que Doxygen puede reconocer sintácticamente pero que no poseen
documentación útil dejaron de dominar las páginas de clase.

### `HIDE_UNDOC_CLASSES = YES`

La navegación de clases se redujo para favorecer entidades con documentación
significativa.

Es importante distinguir esta decisión de ocultar problemas.

Las advertencias se mantuvieron habilitadas:

```ini
WARNINGS             = YES
WARN_IF_UNDOCUMENTED = YES
WARN_IF_DOC_ERROR    = YES
```

Por tanto, una entidad puede no aparecer como elemento destacado en la
navegación final y aun así provocar una advertencia registrada en
`build.log`.

---

## 4.6 Navegación por código fuente

Se configuró:

```ini
SOURCE_BROWSER = YES
INLINE_SOURCES = NO
```

`SOURCE_BROWSER = YES` genera páginas que permiten explorar los archivos con:

- numeración de líneas;
- resaltado de sintaxis;
- enlaces entre entidades.

La evidencia:

```text
evidencias/03-renderwindow-source.png
```

muestra el navegador de código para `RenderWindow.hpp`.

`INLINE_SOURCES = NO` evita insertar grandes bloques de implementación dentro de
cada página de API. El código permanece disponible mediante enlaces específicos
al navegador de fuentes.

---

## 4.7 Referencias cruzadas

Se habilitaron:

```ini
REFERENCED_BY_RELATION = YES
REFERENCES_RELATION    = YES
```

Estas opciones agregan información sobre relaciones de uso.

Por ejemplo, una función puede mostrar:

- qué entidades referencia;
- desde cuáles otras entidades es referenciada.

Esta información complementa las firmas y descripciones tradicionales al
mostrar conexiones entre distintas partes del sistema.

---

## 4.8 Navegación jerárquica

Se mantuvo:

```ini
GENERATE_TREEVIEW = YES
DISABLE_INDEX     = NO
```

Esto genera una navegación lateral que facilita recorrer:

- módulos;
- namespaces;
- clases;
- archivos;
- páginas.

La navegación jerárquica fue considerada particularmente útil en SFML debido al
tamaño del proyecto y a su división funcional.

---

## 4.9 Generación HTML

Se configuró:

```ini
OUTPUT_DIRECTORY = site

GENERATE_HTML = YES
HTML_OUTPUT   = cpp

GENERATE_LATEX = NO
```

El resultado se genera en:

```text
site/cpp/
```

Esto fue elegido para coincidir con la estructura prevista para el sitio final:

```text
site/
├── index.html
├── cpp/
└── python/
```

La documentación C++ podrá publicarse entonces directamente bajo `/cpp/`.

---

## 4.10 Integración con Graphviz

Se configuró:

```ini
HAVE_DOT           = YES
CLASS_GRAPH         = YES
COLLABORATION_GRAPH = YES
INCLUDE_GRAPH       = YES
INCLUDED_BY_GRAPH   = YES

DOT_IMAGE_FORMAT = svg
```

Graphviz permite representar relaciones que no son tan fáciles de interpretar
mediante una lista textual.

Entre los diagramas generados se encuentran:

- herencia;
- colaboración;
- inclusión;
- archivos incluidos por otros.

La evidencia:

```text
evidencias/02-renderwindow-diagrams.png
```

muestra diagramas asociados a `sf::RenderWindow`.

Se utilizó SVG porque:

- mantiene calidad al ampliar;
- resulta apropiado para navegadores;
- funciona correctamente en un sitio estático.

Se dejaron deshabilitados:

```ini
CALL_GRAPH   = NO
CALLER_GRAPH = NO
```

La generación de grafos de llamadas para un proyecto del tamaño de SFML habría
incrementado considerablemente la cantidad de diagramas y el tiempo de
generación.

Para el objetivo del laboratorio se consideró suficiente analizar relaciones de
herencia, colaboración e inclusión.

---

## 4.11 Página principal propia

La portada no se tomó de la documentación oficial de SFML.

Se creó:

```text
doxygen/mainpage.md
```

y se configuró:

```ini
USE_MDFILE_AS_MAINPAGE = doxygen/mainpage.md
```

La portada identifica:

- proyecto;
- versión;
- propósito académico;
- commit;
- directorios incluidos;
- repositorio original;
- licencia.

Esto hace que el sitio generado conserve contexto aun cuando se accede
directamente a la documentación publicada.

---

## 4.12 Alias `sfplatform`

Durante una de las generaciones apareció:

```text
Found unknown command '\sfplatform'
```

Al investigar la causa se encontró que SFML utiliza un comando Doxygen
personalizado para documentar restricciones específicas de plataforma.

La configuración original de SFML define ese comando mediante `ALIASES`.

En lugar de copiar el `Doxyfile` oficial, se tomó únicamente la definición
necesaria y se incorporó a la configuración creada para el laboratorio:

```ini
ALIASES = "sfplatform{1}=..." \
          "sfplatform{2}=..."
```

Esta adaptación permite que Doxygen interprete correctamente los comentarios
propios de SFML sin sustituir la configuración independiente desarrollada para
este laboratorio.

La advertencia desapareció después de esta modificación.

---

## 5. Iteraciones de configuración

El proceso no produjo directamente la configuración final.

Se realizaron varias iteraciones.

### 5.1 Primera generación

La primera ejecución mostró una gran cantidad de coincidencias de advertencias y
una navegación demasiado cargada de entidades internas.

Además, al redirigir inicialmente la salida desde PowerShell, algunas
advertencias se repetían dentro de estructuras `ErrorRecord`, haciendo que un
conteo simple produjera aproximadamente 518 coincidencias.

No todas esas coincidencias representaban 518 problemas independientes.

### 5.2 Ajuste de extracción y presentación

Después de revisar la lista de clases y distintas páginas del sitio se ajustaron:

```ini
EXTRACT_LOCAL_CLASSES = NO
HIDE_UNDOC_MEMBERS    = YES
HIDE_UNDOC_CLASSES    = YES
```

También se mantuvo:

```ini
EXTRACT_ALL = NO
```

La intención fue enfocar la navegación en elementos documentados y relevantes.

La evidencia:

```text
evidencias/08-classlist.png
```

permite documentar la etapa utilizada para evaluar esta decisión.

### 5.3 Corrección de captura del log

La generación posterior se realizó mediante:

```powershell
cmd /c "doxygen .\doxygen\Doxyfile > .\doxygen\build.log 2>&1"
```

Esto permitió conservar la salida de Doxygen directamente en el archivo de log
sin las estructuras adicionales producidas anteriormente por PowerShell.

Con esta forma de conteo se identificaron **13 advertencias reales**.

### 5.4 Resolución de `sfplatform`

Una de las 13 advertencias correspondía al alias:

```text
\sfplatform
```

Después de agregar la definición utilizada por SFML, la generación final
produjo:

```text
12 warnings
```

No se desactivó ninguna bandera de advertencias para obtener este resultado.

---

## 6. Análisis de las 12 advertencias finales

La documentación final se generó correctamente, pero Doxygen conserva 12
advertencias.

Estas no se ocultaron porque representan información relevante sobre las
limitaciones encontradas al documentar un proyecto real.

## 6.1 Funciones de `sf::Dns`

Cinco advertencias corresponden a:

- `resolve`;
- `queryNs`;
- `queryMx`;
- `querySrv`;
- `queryTxt`.

Doxygen encuentra las declaraciones correspondientes y reconoce candidatos
compatibles, pero no logra asociar de manera exacta algunas definiciones de
`Dns.cpp` con los miembros documentados.

Las diferencias pueden involucrar elementos como:

- calificadores de namespace;
- tipos expresados de manera distinta;
- detalles presentes únicamente en la declaración.

Estas advertencias no impiden que las funciones aparezcan en el sitio
generado.

La página del namespace `sf::Dns` muestra correctamente las funciones y las
estructuras asociadas.

Por esta razón se decidió conservar la advertencia en lugar de modificar código
de terceros para intentar satisfacer a Doxygen.

---

## 6.2 Implementaciones de `JoystickImpl::update()`

Tres advertencias corresponden a:

```text
sf::priv::JoystickImpl::update()
```

en implementaciones específicas para:

- FreeBSD;
- NetBSD;
- Unix.

SFML utiliza distintas implementaciones de clases internas dependiendo de la
plataforma.

Doxygen encuentra varios miembros potencialmente compatibles y no puede
determinar de forma única cuál corresponde a determinadas definiciones.

Estas advertencias reflejan una característica de la arquitectura
multiplataforma del proyecto.

No se consideró adecuado excluir arbitrariamente estas plataformas o alterar el
código únicamente para obtener un log vacío.

---

## 6.3 `sf::WindowHandle`

Otra advertencia corresponde a:

```text
sf::WindowHandle
```

Doxygen encuentra documentación asociada al símbolo, pero no determina una
declaración o definición inequívoca.

`WindowHandle` representa un tipo cuyo valor concreto depende de la plataforma.

Este tipo de construcción puede involucrar procesamiento condicional, lo que
hace más difícil que una ejecución estática de Doxygen represente
simultáneamente todas las configuraciones posibles.

La advertencia se mantuvo como una limitación conocida de la generación.

---

## 6.4 Parámetros sin documentación

Dos advertencias corresponden a parámetros identificados por Doxygen pero sin
descripción asociada.

Los casos observados fueron:

- `isHighDpi` en código de `SFOpenGLView` para macOS;
- `factor` en código de `SFView` para iOS.

Doxygen reconoce las funciones y sus parámetros, pero detecta que los comentarios
estructurados no incluyen documentación para esos argumentos.

Estas advertencias son útiles porque muestran que disponer de comentarios
Doxygen no significa necesariamente que cada elemento de la API esté
documentado de forma completa.

---

## 6.5 Enumeración sin documentación

La última advertencia corresponde a:

```text
sf::Sftp::SessionInfo::HostKey::Type
```

Doxygen identifica el elemento, pero la enumeración no posee documentación
suficiente.

Al igual que en los parámetros anteriores, esto representa una ausencia real de
documentación y no un fallo de generación.

---

## 6.6 Advertencia resuelta: `sfplatform`

Durante una generación anterior también apareció:

```text
Found unknown command '\sfplatform'
```

A diferencia de las 12 advertencias anteriores, esta sí podía corregirse desde
la configuración del laboratorio.

Se comprobó que `sfplatform` era un alias definido por SFML y se incorporó
únicamente esa definición al `Doxyfile` propio.

Esto redujo el resultado de:

```text
13 warnings
```

a:

```text
12 warnings
```

sin modificar el código fuente ni desactivar advertencias.

---

## 7. Análisis de la documentación generada

## 7.1 Página principal y navegación

La página principal identifica:

- SFML;
- versión 3.2.0-dev;
- propósito del sitio;
- commit analizado;
- alcance;
- repositorio original.

La navegación permite acceder a:

- Main Page;
- Related Pages;
- Topics;
- Namespaces;
- Classes;
- Files.

La evidencia:

```text
evidencias/01-main-page.png
```

muestra esta portada.

La estructura hace posible comenzar desde una vista general y avanzar
progresivamente hacia entidades específicas.

---

## 7.2 Módulos

SFML utiliza grupos de documentación que Doxygen representa como módulos
funcionales.

Por ejemplo:

```text
Graphics module
```

agrupa clases relacionadas con gráficos como:

- `sf::RenderWindow`;
- `sf::Sprite`;
- `sf::Texture`;
- `sf::Shader`.

La evidencia:

```text
evidencias/04-graphics-module.png
```

muestra este tipo de navegación.

Esta organización permite comprender la biblioteca según responsabilidades y no
únicamente mediante directorios y archivos.

---

## 7.3 Clases

Las páginas de clase contienen:

- descripción;
- constructores;
- métodos;
- miembros;
- métodos heredados;
- relaciones.

La evidencia:

```text
evidencias/02-renderwindow-diagrams.png
```

muestra la página de:

```text
sf::RenderWindow
```

En ella pueden observarse relaciones con otras clases de SFML.

---

## 7.4 Estructuras y namespaces

Los namespaces agrupan entidades relacionadas.

Un ejemplo observado fue:

```text
sf::Dns
```

La página contiene estructuras como:

- `MxRecord`;
- `SrvRecord`.

También incluye funciones como:

- `resolve`;
- `queryNs`;
- `queryMx`;
- `querySrv`;
- `queryTxt`.

Esto permite comprender la responsabilidad de un componente sin recorrer
manualmente todos sus archivos.

---

## 7.5 Métodos, parámetros y retornos

Los comentarios estructurados de SFML permiten generar información detallada de
métodos.

La evidencia:

```text
evidencias/06-sprite-detailed-documentation.png
```

muestra documentación asociada a métodos de:

```text
sf::Sprite
```

Doxygen representa información como:

- parámetros;
- descripciones;
- referencias;
- elementos relacionados.

La evidencia:

```text
evidencias/05-sprite-api.png
```

muestra además el resumen de la API de la clase.

---

## 7.6 Código fuente

Con:

```ini
SOURCE_BROWSER = YES
```

Doxygen genera navegación hacia archivos fuente.

La evidencia:

```text
evidencias/03-renderwindow-source.png
```

muestra:

```text
RenderWindow.hpp
```

con:

- líneas numeradas;
- resaltado de sintaxis;
- navegación entre entidades.

Esta funcionalidad conecta la documentación de API con la implementación real.

---

## 7.7 Diagramas

Graphviz permitió generar diagramas estructurales.

En `sf::RenderWindow` se observan relaciones de herencia y colaboración.

Estos diagramas permiten identificar rápidamente relaciones que requerirían
revisar múltiples declaraciones si solo se utilizara el código fuente.

También se generaron gráficos relacionados con inclusión de archivos.

---

## 7.8 Referencias cruzadas

Las opciones:

```ini
REFERENCES_RELATION    = YES
REFERENCED_BY_RELATION = YES
```

permiten que algunas entidades muestren referencias hacia otros componentes y
desde otros componentes.

Esto facilita explorar dependencias y relaciones de uso.

---

## 7.9 Información proveniente de comentarios

Los comentarios estructurados del proyecto proporcionan elementos como:

- descripciones;
- parámetros;
- retornos;
- notas;
- referencias;
- agrupaciones funcionales.

Por ejemplo, el agrupamiento en módulos como Graphics depende de información
incorporada intencionalmente por los desarrolladores.

---

## 7.10 Información inferida del código

Doxygen también extrae información estructural directamente del código C++.

Entre ella se encuentra:

- nombres de clases;
- estructuras;
- funciones;
- firmas;
- namespaces;
- enumeraciones;
- relaciones de herencia;
- ubicación de archivos.

La diferencia entre información documentada e información inferida se hizo
visible durante la primera generación, cuando aparecieron numerosas entidades
internas aunque no todas poseían documentación orientada a usuarios.

La evidencia:

```text
evidencias/08-classlist.png
```

se utiliza para documentar este comportamiento y justificar los cambios
posteriores en la política de extracción.

---

## 8. Utilidad para una persona nueva en el proyecto

La documentación generada proporciona varias formas de aproximarse a SFML.

Una persona nueva puede comenzar desde los módulos funcionales para entender las
principales responsabilidades del proyecto.

Por ejemplo, para estudiar gráficos puede seguir la ruta:

```text
Graphics
    ↓
RenderWindow / Sprite / Texture
    ↓
API
    ↓
Herencia y colaboración
    ↓
Referencias
    ↓
Código fuente
```

De esta manera puede pasar progresivamente de una vista conceptual a los
detalles de implementación.

Esta combinación resulta más útil para aprender un proyecto que navegar
directamente por cientos de archivos C++ sin contexto.

---

## 9. Elementos incompletos o poco claros

La documentación final no representa de manera perfecta todos los aspectos del
proyecto.

Entre las limitaciones encontradas se incluyen:

- algunas asociaciones ambiguas entre declaraciones y definiciones;
- complejidad provocada por implementaciones específicas de plataforma;
- parámetros sin documentación completa;
- elementos internos que pueden aparecer debido al análisis de `src/SFML`;
- imposibilidad de representar simultáneamente todas las condiciones de
  compilación de todas las plataformas.

Además, aunque la navegación final se redujo significativamente, todavía pueden
aparecer algunas entidades internas documentadas bajo namespaces como
`sf::priv`.

Esto no se consideró un error. El alcance incluye deliberadamente
`src/SFML`, por lo que una parte de la implementación sigue siendo visible.

---

## 10. Evidencias

Las evidencias conservadas son:

| Archivo | Contenido |
|---|---|
| `evidencias/01-main-page.png` | Página principal de la documentación |
| `evidencias/02-renderwindow-diagrams.png` | Diagramas y relaciones de `sf::RenderWindow` |
| `evidencias/03-renderwindow-source.png` | Navegación por `RenderWindow.hpp` |
| `evidencias/04-graphics-module.png` | Organización del módulo Graphics |
| `evidencias/05-sprite-api.png` | Resumen de API de `sf::Sprite` |
| `evidencias/06-sprite-detailed-documentation.png` | Documentación detallada de métodos |
| `evidencias/07-search.png` | Funcionamiento del buscador interno |
| `evidencias/08-classlist.png` | Lista utilizada durante el ajuste de la política de extracción |

Las URL públicas correspondientes se añadirán una vez que el sitio sea
desplegado.

---

## 11. Archivos relacionados

La configuración utilizada se encuentra en:

```text
doxygen/Doxyfile
```

La página principal se encuentra en:

```text
doxygen/mainpage.md
```

El registro de la generación final se encuentra en:

```text
doxygen/build.log
```

La información de selección y métricas se encuentra en:

```text
doxygen/seleccion.md
```

La documentación HTML se genera localmente en:

```text
site/cpp/
```

---

## 12. Conclusiones

La generación de documentación con Doxygen requirió más que ejecutar la
herramienta sobre un repositorio que ya poseía comentarios estructurados.

Fue necesario decidir:

- qué parte del proyecto analizar;
- qué tipos de entidades mostrar;
- cuánto detalle interno incluir;
- cómo organizar la navegación;
- qué relaciones representar;
- qué diagramas generar;
- cómo tratar comandos personalizados;
- cómo interpretar y conservar advertencias.

La primera generación permitió identificar una cantidad excesiva de entidades
internas y problemas de presentación. La configuración se ajustó
iterativamente para producir una documentación más enfocada sin ocultar las
advertencias del proyecto.

La salida final permite navegar clases, namespaces, archivos, módulos, código
fuente, relaciones de herencia y referencias cruzadas. Al mismo tiempo, las 12
advertencias restantes muestran limitaciones reales asociadas con un proyecto
multiplataforma y con documentación incompleta.

Por tanto, la principal contribución del trabajo realizado en esta etapa no fue
simplemente volver a generar la documentación existente de SFML, sino diseñar
una configuración de Doxygen adecuada para el alcance y los objetivos
específicos del laboratorio.
