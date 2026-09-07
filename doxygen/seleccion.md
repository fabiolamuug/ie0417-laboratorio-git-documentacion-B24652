# Selección del proyecto C++ para Doxygen

## Proyecto seleccionado

**Nombre:** SFML (Simple and Fast Multimedia Library)

**Repositorio original:**  
https://github.com/SFML/SFML

**Commit analizado:**  
`2124d5fe87412ac1cf8e20de282502a75039efcf`

**Licencia:**  
zlib/libpng

**Lenguaje principal:**  
C++

## Descripción

SFML es una biblioteca multimedia escrita principalmente en C++ que proporciona
una interfaz de alto nivel para desarrollar aplicaciones gráficas y multimedia.
La biblioteca está organizada en módulos relacionados con gráficos, ventanas,
audio, red y funcionalidades del sistema.

Para este laboratorio se analizará el código fuente principal de SFML contenido
en:

- `include/SFML`
- `src/SFML`

No se incluyen pruebas, ejemplos, dependencias de terceros ni archivos
generados.

## Tamaño del proyecto

El tamaño fue medido utilizando `cloc` 2.10 con el siguiente comando:

```powershell
cloc .\include\SFML .\src\SFML `
  --include-ext=h,hpp,hh,cc,cpp,cxx
```

El resultado obtenido fue:

| Lenguaje | Archivos | Líneas en blanco | Comentarios | Líneas de código |
|---|---:|---:|---:|---:|
| C++ | 142 | 8411 | 8787 | 30883 |
| C/C++ Header | 205 | 4053 | 28281 | 7383 |
| **Total** | **347** | **12464** | **37068** | **38266** |

Además, se realizó un conteo independiente de archivos fuente mediante
PowerShell:

```powershell
$sfmlFiles = Get-ChildItem .\include\SFML, .\src\SFML -Recurse -File |
    Where-Object {
        $_.Extension -in '.h', '.hpp', '.hh', '.cc', '.cpp', '.cxx'
    }

$sfmlFiles.Count
```

Este conteo identificó **351 archivos** con extensiones de código fuente
relevantes.

La diferencia entre los 347 archivos procesados por `cloc` y los 351
identificados mediante PowerShell se debe a que `cloc` reportó algunos archivos
como ignorados durante el análisis.

En ambos casos, el proyecto supera ampliamente los requisitos mínimos del
laboratorio de 10 000 líneas de código fuente y 30 archivos relevantes.

## Razón de selección

SFML resulta apropiado para analizar con Doxygen porque es un proyecto C++
real, de tamaño considerable y con una arquitectura modular claramente
definida.

La biblioteca incluye múltiples clases, estructuras, enumeraciones, espacios
de nombres, funciones y relaciones entre componentes. Esto permite que Doxygen
genere distintos tipos de información técnica, como documentación de clases,
archivos, funciones, miembros, jerarquías de herencia y referencias cruzadas.

Además, el propósito de SFML facilita interpretar la documentación generada,
ya que sus módulos representan áreas concretas de una aplicación multimedia,
como gráficos, ventanas, audio, redes y funcionalidades del sistema.

Esto hace posible evaluar no solamente si Doxygen logra extraer información
del código, sino también qué tan útil resulta esa documentación para una
persona desarrolladora que desea comprender y utilizar la biblioteca.

## Calidad inicial de los comentarios

Antes de generar la documentación se observa que SFML contiene una cantidad
importante de comentarios estructurados orientados a documentación.

En los archivos de cabecera aparecen comentarios asociados a clases, métodos,
parámetros, valores de retorno y otros elementos de la API pública. También se
utilizan comandos reconocibles por Doxygen para organizar y describir la
información.

La salida de `cloc` refuerza esta observación. Dentro del alcance seleccionado
se identificaron **37 068 líneas de comentarios**, una cantidad considerable
en relación con las 38 266 líneas de código.

Esto sugiere que Doxygen debería ser capaz de generar documentación detallada
para buena parte de la API pública de SFML. Sin embargo, durante el análisis
posterior será necesario comprobar qué elementos provienen directamente de
comentarios estructurados y qué información puede inferir Doxygen únicamente
a partir de la estructura del código.

## Dependencias y dificultades previstas

Para la generación de documentación se utilizarán Doxygen y Graphviz.

No se pretende compilar ni ejecutar SFML como parte de esta etapa, ya que el
objetivo es analizar estáticamente sus archivos fuente y generar documentación
HTML a partir del código y de los comentarios disponibles.

Entre las posibles dificultades se encuentran:

- La cantidad de archivos y elementos documentados puede producir un sitio
  relativamente grande.
- Algunas partes de la implementación interna podrían generar información poco
  relevante para una persona interesada principalmente en la API pública.
- Será necesario configurar correctamente las rutas de entrada y exclusión para
  limitar el análisis a `include/SFML` y `src/SFML`.
- La configuración de diagramas mediante Graphviz puede aumentar el tiempo de
  generación.
- Algunas relaciones entre clases, plantillas o componentes internos pueden
  requerir ajustes adicionales en el `Doxyfile` para visualizarse de manera
  útil.

Por estas razones se utilizará una configuración propia de Doxygen, diseñada
específicamente para el alcance definido en este laboratorio, en lugar de
reutilizar directamente la configuración oficial incluida en el repositorio de
SFML.
