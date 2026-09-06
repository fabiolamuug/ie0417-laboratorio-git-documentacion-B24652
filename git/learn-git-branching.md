# Learn Git Branching

## Índice

- [Learn Git Branching](#learn-git-branching)
  - [Índice](#índice)
  - [Principal](#principal)
    - [Secuencia introductoria](#secuencia-introductoria)
      - [Nivel 1 - Introducción a los commits de Git](#nivel-1---introducción-a-los-commits-de-git)
      - [Nivel 2 - Creando ramas en Git](#nivel-2---creando-ramas-en-git)
      - [Nivel 3 - Haciendo merge en Git](#nivel-3---haciendo-merge-en-git)
      - [Nivel 4 - Introducción a rebase](#nivel-4---introducción-a-rebase)
    - [Acelerando](#acelerando)
      - [Nivel 5 - Desacopla tu HEAD](#nivel-5---desacopla-tu-head)
      - [Nivel 6 - Referencias relativas (^)](#nivel-6---referencias-relativas-)
      - [Nivel 7 - Referencias relativas #2 (~)](#nivel-7---referencias-relativas-2-)
      - [Nivel 8 - Revirtiendo cambios en Git](#nivel-8---revirtiendo-cambios-en-git)
    - [Moviendo el trabajo por ahí](#moviendo-el-trabajo-por-ahí)
      - [Nivel 9 - Introducción a cherry-pick](#nivel-9---introducción-a-cherry-pick)
      - [Nivel 10 - Introducción al rebase interactivo](#nivel-10---introducción-al-rebase-interactivo)
      - [Nivel 11 - Área de Staging (preparando)](#nivel-11---área-de-staging-preparando)
      - [Nivel 12 - Undoing with git restore](#nivel-12---undoing-with-git-restore)
    - [Un poco de todo](#un-poco-de-todo)
      - [Nivel 13 - Tomando un único commit](#nivel-13---tomando-un-único-commit)
      - [Nivel 14 - Haciendo malabares con los commits](#nivel-14---haciendo-malabares-con-los-commits)
      - [Nivel 15 - Haciendo malabares con los commits #2](#nivel-15---haciendo-malabares-con-los-commits-2)
      - [Nivel 16 - Tags en Git](#nivel-16---tags-en-git)
      - [Nivel 17 - Git Describe](#nivel-17---git-describe)
    - [Temas avanzados](#temas-avanzados)
      - [Nivel 18 - Rebaseando más de 9000 veces](#nivel-18---rebaseando-más-de-9000-veces)
      - [Nivel 19 - Múltiples padres](#nivel-19---múltiples-padres)
      - [Nivel 20 - Ensalada de ramas](#nivel-20---ensalada-de-ramas)
    - [Progreso completo de Principal](#progreso-completo-de-principal)

## Principal

<a id="secuencia-introductoria"></a>

### Secuencia introductoria

<a id="nivel-01"></a>

#### Nivel 1 - Introducción a los commits de Git

**Objetivo:**  
Crear dos nuevos commits para observar cómo se construye un historial lineal en
Git y cómo una rama avanza automáticamente cuando se agregan commits.

**Estado inicial:**  
El repositorio inicia con dos commits, `C0` y `C1`, conectados de forma
secuencial. La rama `main` se encuentra apuntando a `C1`, que es el commit
actual.

| Paso | Comando | Efecto sobre el repositorio |
| ---: | --- | --- |
| 1 | `git commit` | Crea el nuevo commit `C2` como hijo de `C1`. La rama `main` avanza y pasa a apuntar a `C2`. |
| 2 | `git commit` | Crea el nuevo commit `C3` como hijo de `C2`. La rama `main` avanza nuevamente y pasa a apuntar a `C3`. |

**Estado final:**  
El historial queda compuesto por cuatro commits consecutivos:

`C0 → C1 → C2 → C3`

La rama `main` se encuentra en `C3`, el commit más reciente.

![Resultado del Nivel 1](evidencias/principal/secciones/secuencia-introductoria/nivel-01.png)

**Aprendizaje:**  
Cada vez que se ejecuta `git commit`, Git crea un nuevo commit a partir del
commit actual. Cuando `HEAD` está asociado a una rama, como `main`, la rama
avanza automáticamente para apuntar al nuevo commit. Esto permite construir un
historial lineal de cambios.

---

<a id="nivel-02"></a>

#### Nivel 2 - Creando ramas en Git

**Objetivo:**  
Crear una nueva rama llamada `bugFix` y cambiar el contexto de trabajo hacia
esa rama.

**Estado inicial:**  
El repositorio contiene los commits `C0` y `C1`. La rama `main` se encuentra
apuntando a `C1` y es la rama activa.

| Paso | Comando | Efecto sobre el repositorio |
| ---: | --- | --- |
| 1 | `git branch bugFix` | Crea la rama `bugFix` apuntando al mismo commit que `main`, sin cambiar la rama activa. |
| 2 | `git checkout bugFix` | Cambia la rama activa de `main` a `bugFix`. |

**Estado final:**  
Las ramas `main` y `bugFix` apuntan ambas al commit `C1`, pero `bugFix` es la
rama activa.

![Resultado del Nivel 2](evidencias/principal/secciones/secuencia-introductoria/nivel-02.png)

**Aprendizaje:**  
Crear una rama no implica cambiarse automáticamente a ella. El comando
`git branch` crea la referencia, mientras que `git checkout` permite cambiar
la rama activa. Una rama nueva comienza apuntando al mismo commit desde el
cual fue creada.

---

<a id="nivel-03"></a>

#### Nivel 3 - Haciendo merge en Git

**Objetivo:**  

Crear una rama `bugFix`, realizar un commit en ella, regresar a `main`,
crear otro commit y finalmente integrar los cambios de `bugFix` en `main`
mediante un merge.

**Estado inicial:**  

El repositorio contiene los commits `C0` y `C1`. La rama `main` se encuentra
apuntando a `C1` y es la rama activa.

| Paso | Comando | Efecto sobre el repositorio |
| ---: | --- | --- |
| 1 | `git checkout -b bugFix` | Crea la nueva rama `bugFix` a partir de `main` y cambia inmediatamente a ella. Ambas ramas parten inicialmente de `C1`. |
| 2 | `git commit` | Crea el commit `C2` como hijo de `C1`. La rama `bugFix` avanza hasta `C2`. |
| 3 | `git checkout main` | Cambia nuevamente a la rama `main`, que continúa apuntando a `C1`. |
| 4 | `git commit` | Crea el commit `C3` como hijo de `C1`. La rama `main` avanza hasta `C3`, generando una línea de desarrollo distinta a la de `bugFix`. |
| 5 | `git merge bugFix` | Integra en `main` los cambios de `bugFix`. Como ambas ramas contienen commits distintos posteriores a `C1`, Git crea el merge commit `C4`, cuyos padres son `C3` y `C2`. |

**Estado final:**  

La rama `bugFix` permanece apuntando a `C2`, mientras que `main` queda activa
y apunta al merge commit `C4`.

La estructura final puede representarse como:

`bugFix → C2`  
`main → C4`

El commit `C4` combina las dos líneas de desarrollo que parten de `C1`.

![Resultado del Nivel 3](evidencias/principal/secciones/secuencia-introductoria/nivel-03.png)

**Aprendizaje:**  

`git merge` permite integrar historiales que han avanzado de forma
independiente. Cuando las dos ramas contienen cambios distintos desde un
ancestro común, Git crea un commit de merge con dos padres, preservando ambas
líneas de desarrollo en el historial.

También se aprendió que `git checkout -b bugFix` permite crear una rama y
cambiarse a ella en un solo comando, siendo equivalente a ejecutar primero
`git branch bugFix` y después `git checkout bugFix`. Esto permitió completar
el nivel con menos comandos sin cambiar el resultado final.

---

<a id="nivel-04"></a>

#### Nivel 4 - Introducción a rebase

**Objetivo:**  
Crear una rama `bugFix`, generar cambios independientes en `bugFix` y `main`,
y posteriormente reorganizar el historial de `bugFix` mediante `git rebase`
para colocar sus cambios encima del último commit de `main`.

**Estado inicial:**  
El repositorio contiene los commits `C0` y `C1`. La rama `main` se encuentra
apuntando a `C1` y es la rama activa.

| Paso | Comando | Efecto sobre el repositorio |
| ---: | --- | --- |
| 1 | `git checkout -b bugFix` | Crea la rama `bugFix` a partir de `main` y cambia inmediatamente a ella. |
| 2 | `git commit` | Crea el commit `C2` como hijo de `C1`. La rama `bugFix` avanza a `C2`. |
| 3 | `git checkout main` | Cambia nuevamente a la rama `main`, que continúa apuntando a `C1`. |
| 4 | `git commit` | Crea el commit `C3` como hijo de `C1`. La rama `main` avanza a `C3`. |
| 5 | `git checkout bugFix` | Cambia nuevamente a la rama `bugFix`, ubicada inicialmente en `C2`. |
| 6 | `git rebase main` | Reaplica el cambio de `C2` encima de `C3`, generando el nuevo commit `C2'`. |

**Estado final:**  
La rama `main` apunta a `C3`, mientras que `bugFix` apunta a `C2'`. El historial
activo queda lineal:

`C0 → C1 → C3 → C2'`

El commit original `C2` deja de formar parte del historial actual de
`bugFix`, ya que fue reemplazado por `C2'` durante el rebase.

![Resultado del Nivel 4](evidencias/principal/secciones/secuencia-introductoria/nivel-04.png)

**Aprendizaje:**  
`git rebase` permite reorganizar el historial colocando los commits de una
rama encima de otra. A diferencia de `git merge`, no crea un commit adicional
de unión. En cambio, vuelve a aplicar los cambios y genera nuevos commits con
un padre diferente, por lo que sus identificadores también cambian.

---

<a id="acelerando"></a>

### Acelerando

<a id="nivel-05"></a>

#### Nivel 5 - Desacopla tu HEAD

**Objetivo:**  

Desacoplar `HEAD` de la rama `bugFix` y hacer que apunte directamente al
commit `C4`, especificando dicho commit mediante su hash.

**Estado inicial:**  

El repositorio contiene los commits `C0`, `C1`, `C2`, `C3` y `C4`. La rama
`main` se encuentra apuntando a `C2`, mientras que la rama `bugFix` apunta a
`C4` y es la rama activa.

| Paso | Comando | Efecto sobre el repositorio |
|---:|---|---|
| 1 | `git checkout C4` | Desacopla `HEAD` de la rama `bugFix` y hace que apunte directamente al commit `C4`. La rama `bugFix` permanece apuntando a `C4`. |

**Estado final:**  

`HEAD` queda apuntando directamente al commit `C4`, mientras que la rama
`bugFix` también permanece apuntando a `C4`. La rama `main` continúa
apuntando a `C2`.

El cambio principal puede representarse de la siguiente manera:

`bugFix → C4 ← HEAD`

![Resultado del Nivel 5](evidencias/principal/secciones/acelerando/nivel-05.png)

**Aprendizaje:**  

Normalmente `HEAD` se encuentra asociado a una rama, y esa rama apunta a un
commit. Al ejecutar `git checkout` utilizando directamente el hash de un
commit, `HEAD` deja de estar asociado a una rama y pasa a apuntar directamente
al commit seleccionado. Este estado se conoce como *detached HEAD* y permite
examinar o trabajar temporalmente desde un punto específico del historial sin
mover las ramas existentes.

---

<a id="nivel-06"></a>

#### Nivel 6 - Referencias relativas (^)

**Objetivo:**  

Mover `HEAD` al commit padre del commit actual utilizando una referencia
relativa, en lugar de especificar directamente el hash del commit.

**Estado inicial:**  

El repositorio contiene los commits `C0`, `C1`, `C2`, `C3` y `C4`. La rama
`main` se encuentra apuntando a `C2`, mientras que la rama `bugFix` apunta a
`C4` y es la rama activa. Por lo tanto, `HEAD` se encuentra asociado a
`bugFix` en el commit `C4`.

| Paso | Comando | Efecto sobre el repositorio |
|---:|---|---|
| 1 | `git checkout HEAD^` | Utiliza la referencia relativa `^` para seleccionar el padre del commit actual. Como `HEAD` se encontraba en `C4`, se mueve a su padre `C3`. Al hacer checkout directamente a ese commit, `HEAD` queda desacoplado de la rama `bugFix`. |

**Estado final:**  

`HEAD` queda apuntando directamente al commit `C3` en estado *detached HEAD*.
La rama `bugFix` permanece apuntando a `C4`, mientras que `main` continúa
apuntando a `C2`.

La referencia utilizada puede interpretarse como:

`HEAD^ = padre de C4 = C3`

![Resultado del Nivel 6](evidencias/principal/secciones/acelerando/nivel-06.png)

**Aprendizaje:**  

Las referencias relativas permiten navegar por el historial sin necesidad de
conocer el hash exacto de cada commit. El símbolo `^` indica el commit padre
de la referencia especificada. En este caso, `HEAD^` permitió desplazarse una
generación hacia atrás desde `C4` hasta `C3`. Al realizar checkout directamente
sobre ese commit, `HEAD` quedó desacoplado sin modificar la posición de las
ramas existentes.

---

<a id="nivel-07"></a>

#### Nivel 7 - Referencias relativas #2 (~)

**Objetivo:**  

Mover `HEAD`, `main` y `bugFix` a las posiciones indicadas utilizando
referencias relativas y el movimiento forzado de ramas. El objetivo final es
que `bugFix` apunte a `C0`, `HEAD` quede desacoplado en `C1` y `main` apunte
a `C6`.

**Estado inicial:**  

El repositorio contiene los commits `C0`, `C1`, `C2`, `C3`, `C4`, `C5` y
`C6`. `HEAD` se encuentra desacoplado y apunta directamente a `C2`. La rama
`main` apunta a `C4`, mientras que la rama `bugFix` apunta a `C5`.

| Paso | Comando | Efecto sobre el repositorio |
| ---: | --- | --- |
| 1 | `git branch -f bugFix HEAD~2` | Mueve forzadamente la rama `bugFix` al commit ubicado dos generaciones antes de `HEAD`. Como `HEAD` apunta a `C2`, `HEAD~2` corresponde a `C0`, por lo que `bugFix` pasa de `C5` a `C0`. |
| 2 | `git checkout HEAD~1` | Mueve `HEAD` una generación hacia atrás, desde `C2` hasta su padre `C1`. Como se realiza checkout directamente sobre el commit, `HEAD` permanece desacoplado. |
| 3 | `git branch -f main C6` | Mueve forzadamente la rama `main` desde `C4` hasta el commit `C6`, sin cambiar la posición actual de `HEAD`. |

**Estado final:**  

La rama `bugFix` queda apuntando al commit `C0`, `HEAD` queda desacoplado y
apuntando directamente a `C1`, y la rama `main` queda apuntando al commit
`C6`.

Las posiciones finales pueden resumirse de la siguiente manera:

`bugFix → C0`  
`HEAD → C1`  
`main → C6`

![Resultado del Nivel 7](evidencias/principal/secciones/acelerando/nivel-07.png)

**Aprendizaje:**  

El operador `~` permite desplazarse varias generaciones hacia atrás en el
historial de commits. Por ejemplo, `HEAD~2` representa el segundo ancestro del
commit al que apunta `HEAD`. Además, `git branch -f` permite cambiar
directamente el commit al que apunta una rama sin necesidad de realizar
checkout sobre ella. Este ejercicio demuestra que las ramas funcionan como
referencias móviles y que es posible modificar su posición de manera
independiente a la posición de `HEAD`.

---

<a id="nivel-08"></a>

#### Nivel 8 - Revirtiendo cambios en Git

**Objetivo:**  

Revertir el commit más reciente tanto en una rama local como en una rama que
representa cambios ya publicados, utilizando el método apropiado en cada caso.

**Estado inicial:**  

El repositorio contiene los commits `C0`, `C1`, `C2` y `C3`. La rama `main`
apunta a `C1`. La rama `pushed` apunta a `C2`, mientras que la rama `local`
apunta a `C3` y es la rama activa.

| Paso | Comando | Efecto sobre el repositorio |
| ---: | --- | --- |
| 1 | `git reset HEAD^` | Mueve la rama `local` desde `C3` hasta su commit padre `C1`. Como el cambio de `C3` es local y no se considera compartido, se puede reescribir el historial de la rama eliminando ese commit de su línea activa. |
| 2 | `git checkout pushed` | Cambia la rama activa de `local` a `pushed`, que se encuentra apuntando al commit `C2`. |
| 3 | `git revert HEAD` | Crea un nuevo commit `C2'` que revierte los cambios introducidos por `C2`. La rama `pushed` avanza a `C2'` sin eliminar `C2` del historial. |

**Estado final:**  

Las ramas `main` y `local` quedan apuntando al commit `C1`. La rama `pushed`
queda activa y apunta al nuevo commit `C2'`, creado para revertir los cambios
del commit `C2`.

Las posiciones finales pueden resumirse de la siguiente manera:

`main → C1`  
`local → C1`  
`pushed → C2'`

![Resultado del Nivel 8](evidencias/principal/secciones/acelerando/nivel-08.png)

**Aprendizaje:**  

`git reset` y `git revert` permiten deshacer cambios, pero modifican el
historial de forma diferente. `git reset` mueve la referencia de una rama a
un commit anterior, por lo que es apropiado para cambios locales que todavía
no han sido compartidos. En cambio, `git revert` conserva el historial
existente y crea un nuevo commit que deshace los cambios de otro commit, por
lo que resulta más seguro cuando el historial ya ha sido publicado o
compartido con otras personas.

---

<a id="moviendo-el-trabajo-por-ahi"></a>

### Moviendo el trabajo por ahí

<a id="nivel-09"></a>

#### Nivel 9 - Introducción a cherry-pick

**Objetivo:**  

Copiar hacia la rama `main` tres commits específicos provenientes de otras
ramas, sin integrar por completo los historiales de dichas ramas. Los commits
seleccionados son `C3`, `C4` y `C7`.

**Estado inicial:**  

El repositorio contiene varias ramas que parten del commit `C1`. La rama
`bugFix` contiene los commits `C2` y `C3`, la rama `side` contiene los commits
`C4` y `C5`, y la rama `another` contiene los commits `C6` y `C7`. La rama
`main` se encuentra apuntando a `C1` y es la rama activa.

| Paso | Comando | Efecto sobre el repositorio |
|---:|---|---|
| 1 | `git cherry-pick C3 C4 C7` | Toma los cambios introducidos por los commits `C3`, `C4` y `C7`, en ese orden, y los reaplica sobre la rama activa `main`. Como los commits se aplican sobre una ubicación diferente del historial, Git genera nuevas copias identificadas en el simulador como `C3'`, `C4'` y `C7'`. Las ramas originales no cambian de posición. |

**Estado final:**  

La rama `main` queda apuntando a `C7'` después de incorporar de forma
secuencial copias de los cambios correspondientes a `C3`, `C4` y `C7`.

El nuevo historial de `main` puede representarse como:

`C0 → C1 → C3' → C4' → C7'`

Las ramas `bugFix`, `side` y `another` permanecen en sus posiciones
originales, ya que `cherry-pick` copia commits específicos en lugar de
fusionar las ramas completas.

![Resultado del Nivel 9](evidencias/principal/secciones/moviendo-el-trabajo-por-ahi/nivel-09.png)

**Aprendizaje:**  

`git cherry-pick` permite seleccionar uno o varios commits concretos de otra
parte del historial y aplicar sus cambios sobre la rama actual. A diferencia
de `merge`, no incorpora necesariamente toda una rama ni crea una unión entre
dos historiales. Los commits seleccionados se vuelven a crear sobre la rama
destino, por lo que conservan sus cambios pero adquieren nuevos
identificadores. Además, cuando se especifican varios commits en un mismo
comando, Git los aplica en el orden indicado.

---

<a id="nivel-10"></a>

#### Nivel 10 - Introducción al rebase interactivo

**Objetivo:**  

Utilizar un rebase interactivo para reorganizar los commits recientes de la
rama `main`, eliminando un commit y modificando el orden de los restantes hasta
alcanzar el historial indicado en la visualización objetivo.

**Estado inicial:**  

La rama `main` contiene cuatro commits consecutivos posteriores a `C1`:
`C2`, `C3`, `C4` y `C5`. `main` es la rama activa y `HEAD` se encuentra
asociado a ella en el commit `C5`. La referencia `overHere` permanece
apuntando a `C1`.

| Paso | Comando | Efecto sobre el repositorio |
|---:|---|---|
| 1 | `git rebase -i HEAD~4` | Inicia un rebase interactivo tomando como base el commit ubicado cuatro generaciones antes de `HEAD`, que corresponde a `C1`. Esto permite modificar los cuatro commits posteriores (`C2`, `C3`, `C4` y `C5`), cambiando su orden o eliminándolos. Durante el proceso se elimina `C2` y se reorganizan los demás para obtener el orden `C3`, `C5`, `C4`. Git vuelve a crear estos commits como `C3'`, `C5'` y `C4'`. |

**Estado final:**  

La rama `main` queda apuntando al nuevo commit `C4'`, luego de reorganizar el
historial. La nueva secuencia activa es:

`C0 → C1 → C3' → C5' → C4'`

Los commits originales `C2`, `C3`, `C4` y `C5` dejan de formar parte del
historial actual de `main`, ya que el rebase interactivo reescribió esa parte
del historial.

![Resultado del Nivel 10](evidencias/principal/secciones/moviendo-el-trabajo-por-ahi/nivel-10.png)

**Aprendizaje:**  

`git rebase -i` permite modificar de forma interactiva una parte reciente del
historial, incluyendo el orden de los commits y cuáles de ellos deben
conservarse. La expresión `HEAD~4` no se refiere al commit `C4`, sino al commit
ubicado cuatro generaciones antes de la posición actual de `HEAD`. Ese commit
funciona como base del rebase, y los commits posteriores son los que pueden
reorganizarse. Como el rebase vuelve a crear los commits seleccionados, sus
identificadores cambian.

---

<a id="nivel-11"></a>

#### Nivel 11 - Área de Staging (preparando)

**Objetivo:**  

Preparar y confirmar los cambios de dos archivos de manera independiente,
utilizando el área de staging para mantener cada commit enfocado en un único
cambio. Primero se debe agregar y confirmar `app.js`, y posteriormente hacer
lo mismo con `styles.css`.

**Estado inicial:**  

La rama `main` se encuentra activa y apunta al commit `C1`. El directorio de
trabajo contiene cambios pendientes en los archivos `app.js` y `styles.css`,
los cuales todavía no han sido incluidos en nuevos commits.

| Paso | Comando | Efecto sobre el repositorio |
| ---: | --- | --- |
| 1 | `git add app.js` | Agrega únicamente los cambios de `app.js` al área de staging, dejándolos preparados para formar parte del próximo commit. Los cambios de `styles.css` permanecen fuera del staging. |
| 2 | `git commit` | Crea el commit `C2` utilizando únicamente el contenido previamente preparado en el staging area, por lo que este commit contiene los cambios de `app.js`. La rama `main` avanza a `C2`. |
| 3 | `git add styles.css` | Agrega los cambios pendientes de `styles.css` al área de staging para incluirlos en el siguiente commit. |
| 4 | `git commit` | Crea el commit `C3` con los cambios preparados de `styles.css`. La rama `main` avanza desde `C2` hasta `C3`. |

**Estado final:**  

La rama `main` queda apuntando al commit `C3`. Los cambios de los dos archivos
se encuentran registrados en commits independientes:

`C0 → C1 → C2 (app.js) → C3 (styles.css)`

De esta manera, cada commit representa un cambio específico y mantiene el
historial organizado.

![Resultado del Nivel 11](evidencias/principal/secciones/moviendo-el-trabajo-por-ahi/nivel-11.png)

**Aprendizaje:**  

El área de staging funciona como una zona intermedia entre el directorio de
trabajo y el historial de commits. `git add` permite seleccionar exactamente
qué cambios formarán parte del próximo commit, mientras que `git commit`
registra únicamente lo que se encuentra preparado en dicha área. Esto permite
separar cambios relacionados con distintos archivos o tareas en commits
pequeños, coherentes y fáciles de revisar, en lugar de agrupar todos los
cambios pendientes en un solo commit.

---

<a id="nivel-12"></a>

#### Nivel 12 - Undoing with git restore

> **Nota de la plataforma:** Este nivel apareció sin traducir al español en la interfaz utilizada.

**Objetivo:**  

Preparar el repositorio para crear un commit que contenga únicamente los
cambios de `app.js`. Para ello, se debe retirar `secret.env` del área de
staging sin perder sus modificaciones, descartar los cambios no preparados
de `experiment.js` y finalmente crear el commit con el archivo que permanece
en staging.

**Estado inicial:**  

La rama `main` se encuentra activa. El área de staging contiene cambios de
`app.js` y `secret.env`, mientras que `experiment.js` presenta modificaciones
en el directorio de trabajo que todavía no han sido preparadas.

El objetivo es conservar los cambios de `secret.env` sin incluirlos en el
próximo commit, descartar los cambios de `experiment.js` y registrar únicamente
`app.js`.

| Paso | Comando | Efecto sobre el repositorio |
| ---: | --- | --- |
| 1 | `git restore --staged secret.env` | Retira `secret.env` del área de staging, pero conserva sus modificaciones en el directorio de trabajo. De esta manera, el archivo deja de formar parte del próximo commit sin perder sus cambios. |
| 2 | `git restore experiment.js` | Descarta las modificaciones no preparadas de `experiment.js` y restaura el archivo a la versión almacenada en `HEAD`. |
| 3 | `git commit` | Crea un nuevo commit utilizando únicamente los cambios que continúan en el área de staging. Como `app.js` es el único archivo preparado en ese momento, el nuevo commit contiene exclusivamente sus cambios. |

**Estado final:**  

La rama `main` avanza al nuevo commit `C2`, que contiene únicamente los cambios
de `app.js`.

El área de staging queda limpia. `experiment.js` vuelve a su versión almacenada
en `HEAD`, mientras que los cambios de `secret.env` permanecen sin preparar en
el directorio de trabajo.

![Resultado del Nivel 12](evidencias/principal/secciones/moviendo-el-trabajo-por-ahi/nivel-12.png)

**Aprendizaje:**  

`git restore` puede utilizarse de distintas maneras según dónde se encuentren
los cambios. Con la opción `--staged`, permite retirar un archivo del área de
staging sin eliminar sus modificaciones del directorio de trabajo. En cambio,
cuando se utiliza sin `--staged` sobre un archivo modificado que no está
preparado, permite descartar esos cambios y restaurar la versión almacenada en
`HEAD`.

Este ejercicio también demuestra que `git commit` registra únicamente los
cambios presentes en el área de staging. Por esta razón, no fue necesario
ejecutar `git add`: `app.js` ya se encontraba preparado desde el estado inicial
y era el único archivo que se deseaba incluir en el commit.

---

<a id="un-poco-de-todo"></a>

### Un poco de todo

<a id="nivel-13"></a>

#### Nivel 13 - Tomando un único commit

**Objetivo:**  

Hacer que la rama `main` reciba únicamente el commit al que apunta
`bugFix`, sin incorporar necesariamente todos los commits intermedios de
la rama.

**Estado inicial:**  

El historial parte de `C1` y contiene una serie de commits en la rama
`bugFix`: `C2`, `C3` y `C4`. Las referencias `debug` y `printf` apuntan a
`C2` y `C3`, respectivamente, mientras que `bugFix` apunta a `C4`.

La rama `main` apunta a `C1`. Inicialmente, `bugFix` es la rama activa y
`HEAD` se encuentra asociado a ella en `C4`.

| Paso | Comando | Efecto sobre el repositorio |
| ---: | --- | --- |
| 1 | `git checkout main` | Cambia la rama activa desde `bugFix` hacia `main`. `HEAD` pasa a estar asociado a `main`, mientras que las demás ramas permanecen en sus posiciones originales. |
| 2 | `git cherry-pick C4` | Toma únicamente los cambios introducidos por el commit `C4` y los reaplica sobre la rama `main`. Como el commit se crea sobre una base diferente, Git genera una nueva copia del commit, representada como `C4'`. |

**Estado final:**  

La rama `main` queda activa y apunta al nuevo commit `C4'`, que contiene los
cambios provenientes del commit `C4`.

Las ramas originales permanecen sin modificaciones:

`debug → C2`  
`printf → C3`  
`bugFix → C4`  
`main → C4'`

El nuevo historial de `main` puede representarse como:

`C0 → C1 → C4'`

![Resultado del Nivel 13](evidencias/principal/secciones/un-poco-de-todo/nivel-13.png)

**Aprendizaje:**  

`git cherry-pick` permite trasladar un commit específico a otra rama sin
tener que incorporar toda la secuencia de commits que lo precede en la rama
original. En este caso, aunque `C4` se encontraba después de `C2` y `C3` en
`bugFix`, fue posible aplicar únicamente los cambios correspondientes a `C4`
sobre `main`.

El commit resultante aparece como `C4'` porque Git vuelve a crear el cambio
sobre una base distinta. Por lo tanto, aunque contiene los cambios de `C4`,
es un commit nuevo con una identidad diferente.

---

<a id="nivel-14"></a>

#### Nivel 14 - Haciendo malabares con los commits

**Objetivo:**  

Modificar uno de los commits recientes sin alterar permanentemente el orden
original del historial. Para ello, se debe utilizar un rebase interactivo para
mover temporalmente el commit que se desea corregir hasta la posición de
`HEAD`, modificarlo mediante `git commit --amend`, volver a colocar los commits
en su orden original y finalmente mover `main` al historial actualizado.

**Estado inicial:**  

El historial contiene dos commits recientes, `C2` y `C3`, posteriores a `C1`.
La referencia `newImage` apunta al commit `C2`, mientras que la rama `caption`
se encuentra en la parte más reciente del historial y es la rama activa.

El objetivo consiste en modificar el commit `C2` y conservar posteriormente
el orden original de los commits.

| Paso | Comando | Efecto sobre el repositorio |
| ---: | --- | --- |
| 1 | `git rebase -i HEAD~2` | Inicia un rebase interactivo sobre los dos commits más recientes. Se modifica temporalmente su orden para colocar el commit que se desea corregir en la posición de `HEAD`. Al reescribir el historial, Git genera nuevas versiones de los commits. |
| 2 | `git commit --amend` | Modifica el commit ubicado actualmente en `HEAD`. Como un commit es inmutable, Git no cambia el commit original, sino que crea una nueva versión con un identificador diferente. |
| 3 | `git rebase -i HEAD~2` | Ejecuta un segundo rebase interactivo para restaurar el orden original de los dos commits. Al volver a reescribirlos, se generan nuevas versiones de ambos commits. |
| 4 | `git branch -f main C3''` | Mueve forzadamente la rama `main` al commit más reciente del historial corregido, representado por el simulador como `C3''`. |

**Estado final:**  

El historial conserva el orden deseado, pero contiene nuevas versiones de los
commits como resultado de los rebases y del `amend`.

La parte actualizada del historial queda representada como:

`C0 → C1 → C2''' → C3''`

La rama `main` queda apuntando a `C3''`, junto con la rama activa `caption`.
La referencia `newImage` permanece apuntando al commit original `C2`.

El commit corregido, `C2`, presenta un apóstrofe adicional porque además de ser
reescrito durante los rebases también fue modificado mediante
`git commit --amend`.

![Resultado del Nivel 14](evidencias/principal/secciones/un-poco-de-todo/nivel-14.png)

**Aprendizaje:**  

Un commit de Git no se modifica directamente. Operaciones como
`git commit --amend` y `git rebase` generan nuevos commits con nuevos
identificadores. En Learn Git Branching, los apóstrofes permiten visualizar
pedagógicamente estas nuevas versiones.

El rebase interactivo también puede utilizarse para mover temporalmente un
commit, modificarlo y después reorganizar nuevamente el historial. En este
ejercicio, `C2` fue reescrito durante el primer rebase, modificado mediante
`--amend` y reescrito nuevamente durante el segundo rebase, por lo que terminó
representado como `C2'''`. El commit `C3`, en cambio, únicamente fue reescrito
durante los dos rebases y terminó como `C3''`.

---

<a id="nivel-15"></a>

#### Nivel 15 - Haciendo malabares con los commits #2

**Objetivo:**  

Modificar el commit asociado a `newImage` y construir en `main` un historial
equivalente al objetivo, pero sin utilizar `git rebase -i`. Para lograrlo,
se deben copiar commits específicos mediante `cherry-pick`, modificar el commit
correspondiente con `git commit --amend` y conservar el orden final deseado.

**Estado inicial:**  

El historial parte del commit `C1`. La rama `newImage` apunta al commit `C2`,
mientras que la rama `caption` apunta al commit `C3` y es la rama activa.
La rama `main` permanece apuntando a `C1`.

El objetivo es que `main` termine conteniendo una versión modificada de `C2`
seguida por una copia de `C3`.

| Paso | Comando | Efecto sobre el repositorio |
| ---: | --- | --- |
| 1 | `git checkout main` | Cambia la rama activa desde `caption` hacia `main`. `HEAD` pasa a estar asociado a `main`, que se encuentra apuntando a `C1`. |
| 2 | `git cherry-pick C2` | Copia los cambios introducidos por `C2` sobre la rama `main`. Como se crea un nuevo commit sobre una base distinta, el simulador lo representa como `C2'`. |
| 3 | `git commit --amend` | Modifica el commit recién creado en `HEAD`. Git genera una nueva versión del commit, representada como `C2''`. |
| 4 | `git cherry-pick C3` | Copia los cambios introducidos por `C3` encima del commit modificado `C2''`. El nuevo commit se representa como `C3'`, y la rama `main` avanza hasta él. |

**Estado final:**  

La rama `main` queda activa y apunta al commit `C3'`. Su historial actualizado
queda representado como:

`C0 → C1 → C2'' → C3'`

Las referencias originales permanecen en sus posiciones:

`newImage → C2`  
`caption → C3`  
`main → C3'`

El commit `C2''` contiene la versión modificada del cambio original de `C2`,
mientras que `C3'` corresponde a una nueva aplicación de los cambios de `C3`.

![Resultado del Nivel 15](evidencias/principal/secciones/un-poco-de-todo/nivel-15.png)

**Aprendizaje:**  

`git cherry-pick` puede utilizarse para construir de manera selectiva una
nueva secuencia de commits sin necesidad de reorganizar el historial existente
mediante un rebase interactivo. En este ejercicio se copió primero `C2`, se
modificó su nueva versión mediante `git commit --amend` y posteriormente se
copió `C3` encima de ella.

El resultado demuestra que diferentes combinaciones de comandos pueden alcanzar
una estructura de historial equivalente. También refuerza que tanto
`cherry-pick` como `--amend` generan nuevos commits con identificadores
distintos, lo que Learn Git Branching representa mediante apóstrofes.

---

<a id="nivel-16"></a>

#### Nivel 16 - Tags en Git

**Objetivo:**  

Crear dos etiquetas (`tags`) llamadas `v0` y `v1` sobre commits específicos
del historial y posteriormente realizar checkout sobre `v1` para observar el
comportamiento de `HEAD` al posicionarse directamente sobre un tag.

**Estado inicial:**  

El repositorio contiene varias ramas y commits. La rama `main` es la rama
activa y se encuentra apuntando al commit `C5`. La rama `side` apunta a `C3`.
Los commits `C1` y `C2` todavía no tienen los tags solicitados.

El objetivo es crear el tag `v0` sobre `C1`, el tag `v1` sobre `C2` y
finalmente posicionar `HEAD` sobre `v1`.

| Paso | Comando | Efecto sobre el repositorio |
| ---: | --- | --- |
| 1 | `git tag v0 C1` | Crea el tag `v0` y lo asocia al commit `C1`. La posición de las ramas y de `HEAD` no cambia. |
| 2 | `git tag v1 C2` | Crea el tag `v1` y lo asocia al commit `C2`. Al igual que el comando anterior, solamente se crea una referencia al commit y no se modifica el historial. |
| 3 | `git checkout v1` | Hace checkout al commit identificado por el tag `v1`, es decir, `C2`. Como un tag no es una rama móvil, `HEAD` queda apuntando directamente a `C2` en estado *detached HEAD*. |

**Estado final:**  

El tag `v0` queda asociado al commit `C1`, mientras que el tag `v1` queda
asociado al commit `C2`.

Después de ejecutar `git checkout v1`, `HEAD` apunta directamente a `C2` y
queda desacoplado. Las ramas existentes permanecen en sus posiciones
originales.

Las referencias relevantes pueden resumirse como:

`v0 → C1`  
`v1 → C2 ← HEAD`  
`side → C3`  
`main → C5`

![Resultado del Nivel 16](evidencias/principal/secciones/un-poco-de-todo/nivel-16.png)

**Aprendizaje:**  

Un tag permite asignar un nombre permanente y fácilmente identificable a un
commit específico del historial. A diferencia de una rama, un tag no avanza
automáticamente cuando se crean nuevos commits, por lo que resulta útil para
marcar versiones o puntos importantes del proyecto.

Al hacer checkout directamente sobre un tag, `HEAD` queda desacoplado porque
el tag no funciona como una rama sobre la cual se pueda continuar avanzando
automáticamente. Para desarrollar nuevos cambios desde ese punto sería
necesario crear o cambiar a una rama.

---

<a id="nivel-17"></a>

#### Nivel 17 - Git Describe

**Objetivo:**  

Explorar el funcionamiento de `git describe` sobre distintas ramas del
repositorio para interpretar su posición con respecto a los tags existentes.
Después de revisar las descripciones solicitadas, crear un nuevo commit para
completar el nivel.

**Estado inicial:**  

El repositorio contiene los tags `v0` y `v1`. El tag `v0` apunta al commit
`C0`, mientras que `v1` apunta a `C3`.

La rama `main` apunta a `C2`, la rama `side` apunta a `C4` y la rama
`bugFix` apunta a `C6` y es la rama activa.

| Paso | Comando | Efecto sobre el repositorio |
| ---: | --- | --- |
| 1 | `git describe main` | Describe la posición de `main` con respecto al tag alcanzable más cercano. El resultado `v0-2-gC2` indica que `main` se encuentra dos commits después de `v0`, en el commit `C2`. El repositorio no se modifica. |
| 2 | `git describe side` | Produce `v1-1-gC4`, indicando que la rama `side` se encuentra un commit después del tag `v1`, en `C4`. No modifica el historial. |
| 3 | `git describe bugFix` | Produce `v1-2-gC6`, indicando que `bugFix` se encuentra dos commits después de `v1`, en `C6`. No modifica el historial. |
| 4 | `git describe` | Describe la posición actual de `HEAD`. Como la rama activa es `bugFix` y apunta a `C6`, devuelve igualmente `v1-2-gC6`. |
| 5 | `git commit` | Crea un nuevo commit `C7` como hijo de `C6`. La rama activa `bugFix` avanza hasta este nuevo commit. |

**Estado final:**  

La rama `bugFix` permanece activa y avanza desde `C6` hasta el nuevo commit
`C7`. Las demás ramas y los tags no cambian de posición.

Las referencias principales quedan de la siguiente manera:

`v0 → C0`  
`main → C2`  
`v1 → C3`  
`side → C4`  
`bugFix → C7`

![Resultado del Nivel 17](evidencias/principal/secciones/un-poco-de-todo/nivel-17.png)

**Aprendizaje:**  

`git describe` permite identificar un commit utilizando como referencia el tag
alcanzable más cercano, la cantidad de commits que lo separan de dicho tag y
una representación del identificador del commit.

Por ejemplo, la salida:

`v1-2-gC6`

puede interpretarse como que el commit consultado se encuentra dos commits
después del tag `v1` y que el identificador mostrado por el simulador es `C6`.

El comando `git describe` es únicamente informativo y no modifica ramas,
commits ni tags. En este nivel, el único comando que alteró el repositorio fue
`git commit`, que creó `C7` y movió la rama `bugFix` hacia adelante.

---

<a id="temas-avanzados"></a>

### Temas avanzados

<a id="nivel-18"></a>

#### Nivel 18 - Rebaseando más de 9000 veces

**Objetivo:**  

Construir un historial lineal en la rama `main` utilizando únicamente
operaciones de `rebase`, integrando de forma ordenada los cambios provenientes
de las ramas `bugFix`, `side` y `another`.

El resultado esperado es que los commits de las distintas ramas queden
reaplicados secuencialmente sobre `main`, sin utilizar `cherry-pick`.

**Estado inicial:**  

El repositorio contiene varias líneas de desarrollo independientes. La rama
`main` es la rama activa y apunta al commit `C2`. La rama `bugFix` apunta a
`C3`, mientras que `side` y `another` parten de otra línea del historial y
apuntan a `C6` y `C7`, respectivamente.

La estructura relevante puede resumirse como:

`main → C2`  
`bugFix → C3`  
`side → C6`  
`another → C7`

El objetivo es reorganizar estos cambios para crear una única línea de
historial sobre `main`.

| Paso | Comando | Efecto sobre el repositorio |
| ---: | --- | --- |
| 1 | `git rebase main bugFix` | Reaplica sobre `main` los commits pertenecientes a `bugFix` que todavía no están contenidos en `main`. El commit `C3` se vuelve a crear encima de `C2`, generando `C3'`, y la rama `bugFix` avanza hasta esta nueva versión. |
| 2 | `git rebase bugFix side` | Reaplica la línea de desarrollo de `side` encima de la nueva posición de `bugFix`. Los commits `C4`, `C5` y `C6` se vuelven a crear como `C4'`, `C5'` y `C6'`, haciendo que `side` termine apuntando a `C6'`. |
| 3 | `git rebase --onto side C5 another` | Reaplica sobre `side` únicamente los commits de `another` que se encuentran después de `C5`. Como el único commit exclusivo posterior a `C5` es `C7`, se crea una nueva versión `C7'` encima de `C6'`, evitando volver a copiar `C4` y `C5`. |
| 4 | `git rebase another main` | Actualiza `main` utilizando `another` como nueva base. Como el historial de `another` ya contiene toda la secuencia reorganizada, `main` avanza hasta `C7'` y queda apuntando al mismo commit que `another`. |

**Estado final:**  

El historial queda completamente lineal y la rama `main` termina apuntando al
commit `C7'`, junto con la rama `another`.

La nueva secuencia es:

`C0 → C1 → C2 → C3' → C4' → C5' → C6' → C7'`

Las referencias finales quedan aproximadamente de la siguiente manera:

`bugFix → C3'`  
`side → C6'`  
`another → C7'`  
`main → C7'`

La rama `main` permanece activa.

![Resultado del Nivel 18](evidencias/principal/secciones/temas-avanzados/nivel-18.png)

**Aprendizaje:**  

`git rebase` permite trasladar secuencias completas de commits y construir un
historial lineal a partir de ramas que originalmente se desarrollaron de forma
independiente. Al reaplicar los commits sobre una nueva base, Git crea nuevas
versiones de estos commits, lo que Learn Git Branching representa mediante
apóstrofes.

Este ejercicio también introduce el uso avanzado de `git rebase --onto`. La
forma `git rebase --onto A B C` puede interpretarse como: tomar de la rama `C`
los commits posteriores a `B` y volver a aplicarlos encima de `A`. En este
caso, `git rebase --onto side C5 another` permitió seleccionar únicamente
`C7` de la rama `another` y colocarlo después de `C6'`, sin volver a copiar
los commits `C4` y `C5`.

Finalmente, el ejercicio demuestra que es posible reorganizar varias ramas y
construir una única línea de historial únicamente mediante operaciones de
rebase, sin necesidad de utilizar `cherry-pick`.

---

<a id="nivel-19"></a>

#### Nivel 19 - Múltiples padres

**Objetivo:**  

Crear una nueva rama llamada `bugWork` apuntando al commit `C2`, utilizando
referencias relativas para navegar por un historial que contiene un commit con
múltiples padres. La rama `main` debe permanecer activa y en su posición
original.

**Estado inicial:**  

La rama `main` es la rama activa y apunta al commit `C7`. El historial contiene
un commit de merge `C6`, el cual posee dos padres: `C4` como primer padre y
`C5` como segundo padre.

El commit `C5`, a su vez, tiene como padre a `C2`. El objetivo es crear la rama
`bugWork` directamente sobre `C2` sin mover `HEAD` de la rama `main`.

| Paso | Comando | Efecto sobre el repositorio |
|---:|---|---|
| 1 | `git branch bugWork HEAD~^2~` | Crea la rama `bugWork` en el commit obtenido al navegar desde `HEAD`: `HEAD~` lleva de `C7` a `C6`; `^2` selecciona el segundo padre de `C6`, que es `C5`; y el último `~` lleva al primer padre de `C5`, que es `C2`. La nueva rama queda apuntando a `C2` sin cambiar la posición de `HEAD` ni de `main`. |

**Estado final:**  

La nueva rama `bugWork` queda apuntando al commit `C2`, mientras que `main`
permanece activa y continúa apuntando a `C7`.

Las referencias finales pueden resumirse como:

`bugWork → C2`  
`main → C7`

![Resultado del Nivel 19](evidencias/principal/secciones/temas-avanzados/nivel-19.png)

**Aprendizaje:**  

Las referencias relativas pueden combinar los operadores `~` y `^` para
navegar por historiales que contienen commits con múltiples padres. El operador
`~` sigue el primer padre, mientras que `^2` permite seleccionar explícitamente
el segundo padre de un commit de merge.

En este ejercicio, `HEAD~^2~` permitió llegar desde `C7` hasta `C2` siguiendo
una ruta específica por el historial. Además, `git branch` puede recibir una
referencia como segundo argumento, lo que permite crear una rama directamente
en un commit determinado sin necesidad de hacer checkout previamente.

Inicialmente, la solución se realizó en tres pasos haciendo checkout al commit,
creando la rama y regresando posteriormente a `main`. Sin embargo, utilizar
`git branch bugWork HEAD~^2~` permite obtener exactamente el mismo resultado
en un solo comando y sin modificar temporalmente la posición de `HEAD`.

---

<a id="nivel-20"></a>

#### Nivel 20 - Ensalada de ramas

**Objetivo:**  

Actualizar las ramas `one`, `two` y `three` utilizando versiones reorganizadas
de los commits recientes de `main`. Cada rama requiere una estructura diferente:
`one` debe excluir `C5` y reordenar los commits restantes, `two` debe conservar
todos los commits pero en un orden diferente, y `three` únicamente debe apuntar
al commit `C2`.

**Estado inicial:**  

La rama `main` es la rama activa y contiene una secuencia lineal de commits:

`C0 → C1 → C2 → C3 → C4 → C5`

La rama `main` apunta a `C5`, mientras que las ramas `one`, `two` y `three`
apuntan inicialmente al commit `C1`.

Las posiciones iniciales pueden resumirse como:

`one → C1`  
`two → C1`  
`three → C1`  
`main → C5`

| Paso | Comando | Efecto sobre el repositorio |
| ---: | --- | --- |
| 1 | `git checkout one` | Cambia la rama activa desde `main` hacia `one`, que se encuentra apuntando a `C1`. |
| 2 | `git cherry-pick C4 C3 C2` | Reaplica sobre `one` los cambios de `C4`, `C3` y `C2`, en ese orden. Esto genera las nuevas versiones `C4'`, `C3'` y `C2'`. El commit `C5` no se incluye, tal como requiere el objetivo. |
| 3 | `git checkout two` | Cambia la rama activa desde `one` hacia `two`, que todavía se encuentra apuntando a `C1`. |
| 4 | `git cherry-pick C5 C4 C3 C2` | Reaplica sobre `two` los commits `C5`, `C4`, `C3` y `C2` en el orden indicado. Se generan `C5'`, `C4''`, `C3''` y `C2''`. La rama `two` avanza hasta `C2''`. |
| 5 | `git branch -f three C2` | Mueve forzadamente la rama `three` desde `C1` hasta el commit original `C2`, sin cambiar la rama activa ni crear un nuevo commit. |

**Estado final:**  

La rama `main` permanece sin modificaciones y continúa apuntando a `C5`.

La rama `one` contiene los cambios reorganizados de `C4`, `C3` y `C2`,
excluyendo `C5`:

`C1 → C4' → C3' → C2'`

La rama `two` contiene los cuatro commits reorganizados:

`C1 → C5' → C4'' → C3'' → C2''`

La rama `three` apunta directamente al commit original `C2`.

Las posiciones finales pueden resumirse como:

`main → C5`  
`three → C2`  
`one → C2'`  
`two → C2''`

La rama `two` queda activa al finalizar el nivel.

![Resultado del Nivel 20](evidencias/principal/secciones/temas-avanzados/nivel-20.png)

**Aprendizaje:**  

`git cherry-pick` permite construir historiales personalizados seleccionando
commits específicos y definiendo explícitamente el orden en que sus cambios
deben reaplicarse. Esto permite tanto excluir commits como reorganizarlos sin
modificar la rama original.

En la rama `one` se excluyó `C5` y se aplicaron únicamente `C4`, `C3` y `C2`.
En la rama `two` se conservaron los cuatro commits, pero se aplicaron en el
orden `C5`, `C4`, `C3`, `C2`. Como `C4`, `C3` y `C2` ya habían sido copiados
previamente para construir `one`, Learn Git Branching representa las nuevas
copias creadas para `two` con un segundo apóstrofe.

El ejercicio también demuestra que no siempre es necesario copiar commits.
Cuando únicamente se desea cambiar la posición de una rama, como ocurrió con
`three`, `git branch -f` permite mover directamente la referencia al commit
deseado sin modificar el historial.

---

<a id="progreso-completo"></a>

### Progreso completo de Principal

La siguiente captura muestra todos los niveles disponibles de la sección
`Principal` completados en la versión utilizada de Learn Git Branching.

![Mapa completo de progreso de Principal](evidencias/principal/progreso-completo.png)
