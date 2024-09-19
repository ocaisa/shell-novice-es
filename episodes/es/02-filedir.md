---
title: Navigating Files and Directories
teaching: 30
exercises: 10
---


::::::::::::::::::::::::::::::::::::::: objectives

- Explique las semejanzas y diferencias entre un fichero y un directorio.
- Convierte una ruta absoluta en relativa y viceversa.
- Construir rutas absolutas y relativas que identifiquen archivos y directorios
  específicos.
- Utiliza opciones y argumentos para cambiar el comportamiento de un comando del shell.
- Demostrar el uso del tabulador y explicar sus ventajas.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- ¿Cómo puedo moverme por mi ordenador?
- ¿Cómo puedo ver qué archivos y directorios tengo?
- ¿Cómo puedo especificar la ubicación de un archivo o directorio en mi ordenador?

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: instructor

Introducir y navegar por el sistema de archivos en el shell (tratado en la sección
[Navegar por archivos y directorios](02-filedir.md)) puede resultar confuso. Puede tener
tanto la terminal como el explorador de archivos GUI abiertos uno al lado del otro para
que los alumnos puedan ver el contenido y la estructura de archivos mientras utilizan la
terminal para navegar por el sistema.

::::::::::::::::::::::::::::::::::::::::::::::::::

La parte del sistema operativo encargada de gestionar los archivos y directorios se
denomina **sistema de archivos**. Organiza nuestros datos en archivos, que contienen
información, y directorios (también llamados "carpetas"), que contienen archivos u otros
directorios.

Hay varios comandos que se utilizan con frecuencia para crear, inspeccionar, renombrar y
eliminar archivos y directorios. Para empezar a explorarlos, iremos a nuestra ventana de
shell abierta.

Primero, averigüemos dónde estamos ejecutando un comando llamado `pwd` (que significa
'imprimir directorio de trabajo'). Los directorios son como *lugares* - en cualquier
momento mientras usamos el shell, estamos exactamente en un lugar llamado nuestro
**directorio de trabajo actual**. La mayoría de los comandos leen y escriben archivos en
el directorio de trabajo actual, es decir, "aquí", por lo que es importante saber dónde
nos encontramos antes de ejecutar un comando. `pwd` te muestra dónde estás:

```bash
$ pwd
```

```output
/Users/nelle
```

Aquí, la respuesta del ordenador es `/Users/nelle`, que es el **directorio de inicio**
de Nelle:

::::::::::::::::::::::::::::::::::::::::: callout

## Variación del directorio de inicio

La ruta del directorio de inicio tendrá un aspecto diferente en los distintos sistemas
operativos. En Linux, puede parecerse a `/home/nelle`, y en Windows, será similar a
`C:\Documents and Settings\nelle` o `C:\Users\nelle`. (Tenga en cuenta que puede tener
un aspecto ligeramente diferente para las distintas versiones de Windows.) En futuros
ejemplos, hemos utilizado la salida de Mac por defecto - la salida de Linux y Windows
puede diferir ligeramente, pero en general debería ser similar.

También asumiremos que tu comando `pwd` devuelve el directorio home de tu usuario. Si
`pwd` devuelve algo diferente, puede que necesites navegar hasta allí usando `cd` o
algunos comandos de esta lección no funcionarán como están escritos. Ver [Explorando
otros directorios](#explorando-otros-directorios) para más detalles sobre el comando
`cd`.


::::::::::::::::::::::::::::::::::::::::::::::::::

Para entender qué es un "directorio personal", veamos cómo está organizado el sistema de
archivos en su conjunto. En este ejemplo, ilustraremos el sistema de archivos del
ordenador de nuestra científica Nelle. Después de esta ilustración, aprenderás comandos
para explorar tu propio sistema de archivos, que estará construido de forma similar,
pero no será exactamente idéntico.

En el ordenador de Nelle, el sistema de archivos tiene este aspecto:

![](fig/filesystem.svg){alt='El sistema de ficheros se compone de un directorio raíz que
contiene subdirectorios titulados bin, data, users y tmp'}

El sistema de ficheros se parece a un árbol invertido. El directorio superior es el
**directorio raíz** que contiene todo lo demás. Nos referimos a él utilizando un
carácter de barra oblicua, `/`, por sí solo; este carácter es la barra oblicua inicial
de `/Users/nelle`.

Dentro de ese directorio hay otros directorios: `bin` (que es donde se almacenan algunos
programas incorporados), `data` (para archivos de datos varios), `Users` (donde se
encuentran los directorios personales de los usuarios), `tmp` (para archivos temporales
que no necesitan almacenarse a largo plazo), etc.

Sabemos que nuestro directorio de trabajo actual `/Users/nelle` está almacenado dentro
de `/Users` porque `/Users` es la primera parte de su nombre. Del mismo modo, sabemos
que `/Users` está dentro del directorio raíz `/` porque su nombre empieza por `/`.

::::::::::::::::::::::::::::::::::::::::: callout

## Barra oblicua

Observe que el carácter `/` tiene dos significados. Cuando aparece al principio del
nombre de un fichero o directorio, se refiere al directorio raíz. Cuando aparece
*dentro* de una ruta, es sólo un separador.


::::::::::::::::::::::::::::::::::::::::::::::::::

Debajo de `/Users`, encontramos un directorio para cada usuario con una cuenta en la
máquina de Nelle, sus colegas *imhotep* y *larry*.

![](fig/home-directories.svg){alt='Al igual que otros directorios, los directorios
personales son subdirectorios que se encuentran debajo de "/Usuarios", como
"/Usuarios/imhotep", "/Usuarios/larry" o"/Usuarios/nelle"'}

Los archivos del usuario *imhotep* se almacenan en `/Users/imhotep`, los del usuario
*larry* en `/Users/larry`, y los de Nelle en `/Users/nelle`. Nelle es el usuario en
nuestros ejemplos aquí; por lo tanto, obtenemos `/Users/nelle` como nuestro directorio
personal. Normalmente, cuando abres un nuevo símbolo del sistema, estarás en tu
directorio personal para empezar.

Ahora vamos a aprender el comando que nos permitirá ver el contenido de nuestro propio
sistema de ficheros. Podemos ver lo que hay en nuestro directorio personal ejecutando
`ls`:

```bash
$ ls
```

```output
Applications Documents    Library      Music        Public
Desktop      Downloads    Movies       Pictures
```

(De nuevo, tus resultados pueden ser ligeramente diferentes dependiendo de tu sistema
operativo y de cómo hayas personalizado tu sistema de ficheros)

`ls` imprime los nombres de los ficheros y directorios del directorio actual. Podemos
hacer su salida más comprensible utilizando la **opción** `-F` que indica a `ls` que
clasifique la salida añadiendo un marcador a los nombres de ficheros y directorios para
indicar de qué se trata:

- un `/` al final indica que se trata de un directorio
- `@` indica un enlace
- `*` indica un ejecutable

Dependiendo de la configuración predeterminada del intérprete de comandos, éste también
puede utilizar colores para indicar si cada entrada es un archivo o un directorio.

```bash
$ ls -F
```

```output
Applications/ Documents/    Library/      Music/        Public/
Desktop/      Downloads/    Movies/       Pictures/
```

Aquí podemos ver que el directorio raíz contiene sólo **subdirectorios**. Cualquier
nombre en la salida que no tenga un símbolo de clasificación son **archivos** en el
directorio de trabajo actual.

::::::::::::::::::::::::::::::::::::::::: callout

## Borrar su terminal

Si la pantalla está demasiado llena, puedes borrarla con el comando `clear`. Puedes
seguir accediendo a comandos anteriores utilizando <kbd>↑</kbd> y <kbd>↓</kbd> para
desplazarte línea a línea, o desplazándote por el terminal.


::::::::::::::::::::::::::::::::::::::::::::::::::

### Obtener ayuda

`ls` tiene muchas otras **opciones**. Hay dos formas comunes de averiguar cómo usar un
comando y qué opciones acepta --- **dependiendo de tu entorno, puede que sólo una de
estas formas funcione:**

1. Podemos pasar una opción `--help` a cualquier comando (disponible en Linux y Git
   Bash), por ejemplo

  ```bash
  $ ls --help
  ```

2. Podemos leer su manual con `man` (disponible en Linux y macOS):

  ```bash
  $ man ls
  ```

A continuación describiremos ambas formas.

::::::::::::::::::::::::::::::::::::::::: callout

## Ayuda para comandos incorporados

Algunos comandos están integrados en el intérprete de comandos Bash, en lugar de existir
como programas independientes en el sistema de archivos. Un ejemplo es el comando `cd`
(cambiar directorio). Si recibes un mensaje como `No manual entry for cd`, prueba con
`help cd` en su lugar. El comando `help` es la forma de obtener información de uso de
[Bash built-ins](https://www.gnu.org/software/bash/manual/html_node/Bash-Builtins.html).


::::::::::::::::::::::::::::::::::::::::::::::::::

#### La opción `--help`

La mayoría de los comandos de bash y programas que la gente ha escrito para ser
ejecutados desde bash, soportan una opción `--help` que muestra más información sobre
cómo usar el comando o programa.

```bash
$ ls --help
```

```output
Usage: ls [OPTION]... [FILE]...
List information about the FILEs (the current directory by default).
Sort entries alphabetically if neither -cftuvSUX nor --sort is specified.

Mandatory arguments to long options are mandatory for short options, too.
  -a, --all                  do not ignore entries starting with .
  -A, --almost-all           do not list implied . and ..
      --author               with -l, print the author of each file
  -b, --escape               print C-style escapes for nongraphic characters
      --block-size=SIZE      scale sizes by SIZE before printing them; e.g.,
                               '--block-size=M' prints sizes in units of
                               1,048,576 bytes; see SIZE format below
  -B, --ignore-backups       do not list implied entries ending with ~
  -c                         with -lt: sort by, and show, ctime (time of last
                               modification of file status information);
                               with -l: show ctime and sort by name;
                               otherwise: sort by ctime, newest first
  -C                         list entries by columns
      --color[=WHEN]         colorize the output; WHEN can be 'always' (default
                               if omitted), 'auto', or 'never'; more info below
  -d, --directory            list directories themselves, not their contents
  -D, --dired                generate output designed for Emacs' dired mode
  -f                         do not sort, enable -aU, disable -ls --color
  -F, --classify             append indicator (one of */=>@|) to entries
...        ...        ...
```

::::::::::::::::::::::::::::::::::::::::: callout

### Cuándo utilizar opciones cortas o largas
Cuando existen opciones tanto cortas como largas:

- Utilice la opción abreviada cuando escriba comandos directamente en el intérprete de
  comandos para minimizar las pulsaciones de teclas y realizar su tarea más rápidamente.
- Utilice la opción larga en los guiones para aportar claridad. Se leerá muchas veces y
  se escribirá una sola vez.

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::: callout

## Opciones de línea de comandos no compatibles

Si intenta utilizar una opción que no está soportada, `ls` y otros comandos normalmente
imprimirán un mensaje de error similar a:

```bash
$ ls -j
```

```error
ls: invalid option -- 'j'
Try 'ls --help' for more information.
```

::::::::::::::::::::::::::::::::::::::::::::::::::

#### El comando `man`

La otra forma de conocer `ls` es escribir

```bash
$ man ls
```

Este comando convertirá tu terminal en una página con una descripción del comando `ls` y
sus opciones.

Para navegar por las páginas `man`, puede utilizar <kbd>↑</kbd> y <kbd>↓</kbd> para
desplazarse línea a línea, o pruebe con <kbd>b</kbd> y <kbd>Barra espaciadora</kbd> para
saltar una página completa hacia arriba y hacia abajo. Para buscar un carácter o palabra
en las páginas `man`, utilice <kbd>/</kbd> seguido del carácter o palabra que busca. A
veces, una búsqueda dará como resultado varios resultados. Si es así, puede desplazarse
entre ellas con <kbd>N</kbd> (para avanzar) y <kbd>Mayús</kbd>\+<kbd>N</kbd> (para
retroceder).

Para **salir** de las páginas `man`, pulse <kbd>q</kbd>.

::::::::::::::::::::::::::::::::::::::::: callout

## Páginas del manual en la web

Por supuesto, existe una tercera forma de acceder a la ayuda de los comandos: buscar en
Internet a través del navegador web. Cuando utilices la búsqueda en Internet, incluir la
frase `unix man page` en tu consulta de búsqueda te ayudará a encontrar resultados
relevantes.

GNU proporciona enlaces a sus [manuales](https://www.gnu.org/manual/manual.html)
incluyendo el [núcleo de utilidades
GNU](https://www.gnu.org/software/coreutils/manual/coreutils.html), que cubre muchos
comandos introducidos en esta lección.


::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Explorar más opciones `ls`

También puede utilizar dos opciones al mismo tiempo. ¿Qué hace el comando `ls` cuando se
utiliza con la opción `-l`? ¿Y si utilizas tanto la opción `-l` como la `-h`?

Parte de sus resultados se refieren a propiedades que no tratamos en esta lección (como
los permisos y la propiedad de los archivos), pero el resto debería ser útil.

::::::::::::::: solution

## Solución

La opción `-l` hace que `ls` utilice un formato de listado **l**largo, mostrando no sólo
los nombres de los ficheros/directorios sino también información adicional, como el
tamaño del fichero y la hora de su última modificación. Si utilizas tanto la opción `-h`
como la opción `-l`, esto hace que el tamaño del fichero sea '**h**man readable', es
decir, mostrando algo como `5.3K` en lugar de `5369`.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Listado en orden cronológico inverso

Por defecto, `ls` lista el contenido de un directorio en orden alfabético por nombre. El
comando `ls -t` lista los elementos por la hora del último cambio en lugar de
alfabéticamente. El comando `ls -r` lista los contenidos de un directorio en orden
inverso. ¿Qué fichero se muestra en último lugar cuando se combinan las opciones `-t` y
`-r`? Pista: Puede que necesites usar la opción `-l` para ver las fechas de los últimos
cambios.

::::::::::::::: solution

## Solución

El fichero modificado más recientemente aparece en último lugar cuando se utiliza `-rt`.
Esto puede ser muy útil para encontrar sus ediciones más recientes o para comprobar si
se ha escrito un nuevo archivo de salida.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

### Explorando otros directorios

No sólo podemos utilizar `ls` en el directorio de trabajo actual, sino que también
podemos utilizarlo para listar el contenido de un directorio diferente. Echemos un
vistazo a nuestro directorio `Desktop` ejecutando `ls -F Desktop`, es decir, el comando
`ls` con la **opción** `-F` y el [**argumento**][Argumentos] `Desktop`. El argumento
`Desktop` indica a `ls` que queremos un listado de algo distinto a nuestro directorio de
trabajo actual:

```bash
$ ls -F Desktop
```

```output
shell-lesson-data/
```

Tenga en cuenta que si un directorio llamado `Desktop` no existe en su directorio de
trabajo actual, este comando devolverá un error. Típicamente, un directorio `Desktop`
existe en tu directorio home, el cual asumimos es el directorio de trabajo actual de tu
shell bash.

Su salida debería ser una lista de todos los archivos y subdirectorios en su directorio
de Escritorio, incluyendo el directorio `shell-lesson-data` que descargó en el [setup
para esta lección](../learners/setup.md). (En la mayoría de los sistemas, el contenido
del directorio `Desktop` en el shell se mostrará como iconos en una interfaz gráfica de
usuario detrás de todas las ventanas abiertas. Compruebe si éste es su caso)

Organizar las cosas jerárquicamente nos ayuda a seguir nuestro trabajo. Aunque es
posible poner cientos de archivos en nuestro directorio personal, igual que es posible
apilar cientos de papeles impresos en nuestro escritorio, es mucho más fácil encontrar
las cosas cuando se han organizado en subdirectorios con nombres sensatos.

Ahora que sabemos que el directorio `shell-lesson-data` se encuentra en nuestro
directorio Desktop, podemos hacer dos cosas.

Primero, utilizando la misma estrategia que antes, podemos ver su contenido pasando un
nombre de directorio a `ls`:

```bash
$ ls -F Desktop/shell-lesson-data
```

```output
exercise-data/  north-pacific-gyre/
```

En segundo lugar, podemos cambiar nuestra ubicación a un directorio diferente, de modo
que ya no nos encontremos en nuestro directorio raíz.

El comando para cambiar de ubicación es `cd` seguido de un nombre de directorio para
cambiar nuestro directorio de trabajo. `cd` significa 'cambiar directorio', lo que es un
poco engañoso. El comando no cambia el directorio; cambia el directorio de trabajo
actual del shell. En otras palabras, cambia la configuración de la shell para saber en
qué directorio estamos. El comando `cd` es similar a hacer doble clic en una carpeta en
una interfaz gráfica para entrar en esa carpeta.

Supongamos que queremos entrar en el directorio `exercise-data` que hemos visto antes.
Podemos utilizar la siguiente serie de comandos para llegar allí:

```bash
$ cd Desktop
$ cd shell-lesson-data
$ cd exercise-data
```

Estos comandos nos moverán de nuestro directorio de inicio a nuestro directorio de
Escritorio, luego al directorio `shell-lesson-data`, luego al directorio
`exercise-data`. Notarás que `cd` no imprime nada. Esto es normal. Muchos comandos del
shell no muestran nada en pantalla cuando se ejecutan correctamente. Pero si ejecutamos
`pwd` después, podemos ver que ahora estamos en
`/Users/nelle/Desktop/shell-lesson-data/exercise-data`.

Si ahora ejecutamos `ls -F` sin argumentos, nos muestra el contenido de
`/Users/nelle/Desktop/shell-lesson-data/exercise-data`, porque es donde estamos ahora:

```bash
$ pwd
```

```output
/Users/nelle/Desktop/shell-lesson-data/exercise-data
```

```bash
$ ls -F
```

```output
alkanes/  animal-counts/  creatures/  numbers.txt  writing/
```

Ya sabemos cómo descender por el árbol de directorios (es decir, cómo entrar en un
subdirectorio), pero ¿cómo ascender (es decir, cómo salir de un directorio y entrar en
su directorio padre)? Podríamos intentar lo siguiente:

```bash
$ cd shell-lesson-data
```

```error
-bash: cd: shell-lesson-data: No such file or directory
```

¡Pero obtenemos un error! ¿A qué se debe?

Con nuestros métodos hasta ahora, `cd` sólo puede ver subdirectorios dentro de su
directorio actual. Hay diferentes maneras de ver los directorios por encima de su
ubicación actual; vamos a empezar con el más simple.

Existe un atajo en el shell para subir un nivel de directorio. Funciona de la siguiente
manera:

```bash
$ cd ..
```

`..` es un nombre de directorio especial que significa "el directorio que contiene a
éste", o más sucintamente, el **padre** del directorio actual. Efectivamente, si
ejecutamos `pwd` después de ejecutar `cd ..`, volvemos a estar en
`/Users/nelle/Desktop/shell-lesson-data`:

```bash
$ pwd
```

```output
/Users/nelle/Desktop/shell-lesson-data
```

El directorio especial `..` no suele aparecer cuando ejecutamos `ls`. Si queremos
mostrarlo, podemos añadir la opción `-a` a `ls -F`:

```bash
$ ls -F -a
```

```output
./  ../  exercise-data/  north-pacific-gyre/
```

`-a` significa 'mostrar todo' (incluyendo ficheros ocultos); fuerza a `ls` a mostrarnos
nombres de ficheros y directorios que empiecen por `.`, como `..` (que, si estamos en
`/Users/nelle`, se refiere al directorio `/Users`). Como puedes ver, también muestra
otro directorio especial que se llama simplemente `.`, que significa 'el directorio de
trabajo actual'. Puede parecer redundante tener un nombre para él, pero pronto le
veremos algunos usos.

Tenga en cuenta que en la mayoría de las herramientas de línea de comandos, se pueden
combinar varias opciones con una sola `-` y sin espacios entre las opciones; `ls -F -a`
equivale a `ls -Fa`.

::::::::::::::::::::::::::::::::::::::::: callout

## Otros archivos ocultos

Además de los directorios ocultos `..` y `.`, también puede ver un archivo llamado
`.bash_profile`. Este archivo suele contener ajustes de configuración del shell. También
puedes ver otros ficheros y directorios que empiezan por `.`. Normalmente son ficheros y
directorios que se utilizan para configurar diferentes programas en tu ordenador. El
prefijo `.` se utiliza para evitar que estos archivos de configuración saturen el
terminal cuando se utiliza un comando estándar `ls`.


::::::::::::::::::::::::::::::::::::::::::::::::::

Estos tres comandos son los básicos para navegar por el sistema de ficheros de tu
ordenador: `pwd`, `ls`, y `cd`. Exploremos algunas variaciones de estos comandos. ¿Qué
ocurre si escribes `cd` solo, sin dar un directorio?

```bash
$ cd
```

¿Cómo puedes comprobar lo que ha pasado? ¡`pwd` nos da la respuesta!

```bash
$ pwd
```

```output
/Users/nelle
```

Resulta que `cd` sin un argumento te devolverá a tu directorio de inicio, lo que es
genial si te has perdido en tu propio sistema de archivos.

Intentemos volver al directorio `exercise-data` de antes. La última vez, usamos tres
comandos, pero en realidad podemos encadenar la lista de directorios para movernos a
`exercise-data` en un solo paso:

```bash
$ cd Desktop/shell-lesson-data/exercise-data
```

Comprueba que nos hemos movido al lugar correcto ejecutando `pwd` y `ls -F`.

Si queremos subir un nivel desde el directorio de datos, podríamos utilizar `cd ..`.
Pero hay otra forma de moverse a cualquier directorio, independientemente de su
ubicación actual.

Hasta ahora, al especificar nombres de directorio, o incluso una ruta de directorio
(como arriba), hemos estado utilizando **rutas relativas**. Cuando se utiliza una ruta
relativa con un comando como `ls` o `cd`, se intenta encontrar esa ubicación desde donde
nos encontramos, en lugar de desde la raíz del sistema de ficheros.

Sin embargo, es posible especificar la **ruta absoluta** a un directorio incluyendo su
ruta completa desde el directorio raíz, que se indica con una barra oblicua inicial. La
barra `/` indica al ordenador que siga la ruta desde la raíz del sistema de ficheros, de
forma que siempre se refiera exactamente a un directorio, independientemente de dónde
nos encontremos cuando ejecutemos el comando.

Esto nos permite movernos a nuestro directorio `shell-lesson-data` desde cualquier parte
del sistema de ficheros (incluso desde dentro de `exercise-data`). Para encontrar la
ruta absoluta que buscamos, podemos usar `pwd` y luego extraer el trozo que necesitamos
mover a `shell-lesson-data`.

```bash
$ pwd
```

```output
/Users/nelle/Desktop/shell-lesson-data/exercise-data
```

```bash
$ cd /Users/nelle/Desktop/shell-lesson-data
```

Ejecuta `pwd` y `ls -F` para asegurarte de que estamos en el directorio que esperamos.

::::::::::::::::::::::::::::::::::::::::: callout

## Dos atajos más

El shell interpreta que una tilde (`~`) al principio de una ruta significa "el
directorio personal del usuario actual". Por ejemplo, si el directorio personal de Nelle
es `/Users/nelle`, entonces `~/data` equivale a `/Users/nelle/data`. Esto sólo funciona
si es el primer carácter de la ruta; `here/there/~/elsewhere` *no* es
`here/there/Users/nelle/elsewhere`.

Otro atajo es el carácter `-` (guión). `cd` traducirá `-` a *el directorio anterior en
el que estaba*, lo que es más rápido que tener que recordar, y luego teclear, la ruta
completa. Esta es una forma *muy* eficiente de moverse *hacia adelante y hacia atrás
entre dos directorios* -- es decir, si ejecutas `cd -` dos veces, acabarás de vuelta en
el directorio inicial.

La diferencia entre `cd ..` y `cd -` es que el primero te lleva *hacia arriba*, mientras
que el segundo te lleva *hacia atrás*.

***

¡Pruébalo! Primero navega a `~/Desktop/shell-lesson-data` (ya deberías estar allí).

```bash
$ cd ~/Desktop/shell-lesson-data
```

Luego `cd` en el directorio `exercise-data/creatures`

```bash
$ cd exercise-data/creatures
```

Ahora bien, si ejecuta

```bash
$ cd -
```

verás que has vuelto a `~/Desktop/shell-lesson-data`. Vuelve a ejecutar `cd -` y estarás
de nuevo en `~/Desktop/shell-lesson-data/exercise-data/creatures`


::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Trayectorias absolutas vs relativas

Partiendo de `/Users/nelle/data`, ¿cuál de los siguientes comandos podría utilizar Nelle
para navegar hasta su directorio personal, que es `/Users/nelle`?

1. `cd .`
2. `cd /`
3. `cd /home/nelle`
4. `cd ../..`
5. `cd ~`
6. `cd home`
7. `cd ~/data/..`
8. `cd`
9. `cd ..`

::::::::::::::: solution

## Solución

1. No: `.` representa el directorio actual.
2. No: `/` representa el directorio raíz.
3. No: el directorio personal de Nelle es `/Users/nelle`.
4. No: este comando sube dos niveles, es decir, termina en `/Users`.
5. Sí: `~` representa el directorio personal del usuario, en este caso `/Users/nelle`.
6. No: este comando navegaría a un directorio `home` en el directorio actual si existe.
7. Sí: innecesariamente complicado, pero correcto.
8. Sí: acceso directo para volver al directorio personal del usuario.
9. Sí: sube un nivel.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Resolución relativa de la trayectoria

Utilizando el siguiente diagrama del sistema de ficheros, si `pwd` muestra
`/Users/thing`, ¿qué mostrará `ls -F ../backup`?

1. `../backup: No such file or directory`
2. `2012-12-01 2013-01-08 2013-01-27`
3. `2012-12-01/ 2013-01-08/ 2013-01-27/`
4. `original/ pnas_final/ pnas_sub/`

![](fig/filesystem-challenge.svg){alt='Un árbol de directorios bajo el directorio Users
donde "/Users" contiene los directorios "backup" y "thing"; "/Users/backup" contiene
"original", "pnas\_final" y "pnas\_sub"; "/Users/thing" contiene "backup";
y"/Users/thing/backup" contiene "2012-12-01", "2013-01-08" y "2013-01-27"'}

::::::::::::::: solution

## Solución

1. No: *existe* un directorio `backup` en `/Users`.
2. No: este es el contenido de `Users/thing/backup`, pero con `..`, pedimos un nivel más
   arriba.
3. No: véase la explicación anterior.
4. Sí: `../backup/` se refiere a `/Users/backup/`.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## `ls` Comprensión lectora

Usando el diagrama del sistema de ficheros de abajo, si `pwd` muestra `/Users/backup`, y
`-r` le dice a `ls` que muestre las cosas en orden inverso, qué comando(s) dará(n) como
resultado la siguiente salida:

```output
pnas_sub/ pnas_final/ original/
```

![](fig/filesystem-challenge.svg){alt='Un árbol de directorios bajo el directorio Users
donde "/Users" contiene los directorios "backup" y "thing"; "/Users/backup" contiene
"original", "pnas\_final" y "pnas\_sub"; "/Users/thing" contiene "backup";
y"/Users/thing/backup" contiene "2012-12-01", "2013-01-08" y "2013-01-27"'}

1. `ls pwd`
2. `ls -r -F`
3. `ls -r -F /Users/backup`

::::::::::::::: solution

## Solución

1. No: `pwd` no es el nombre de un directorio.
2. Sí: `ls` sin argumento de directorio lista los archivos y directorios del directorio
   actual.
3. Sí: utiliza la ruta absoluta explícitamente.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Sintaxis general de un comando Shell

Ya nos hemos encontrado con comandos, opciones y argumentos, pero quizá sea útil
formalizar algo de terminología.

Considere el siguiente comando como un ejemplo general de comando, que diseccionaremos
en sus partes componentes:

```bash
$ ls -F /
```

![](fig/shell_command_syntax.svg){alt='Sintaxis general de un comando shell'}

`ls` es el **comando**, con una **opción** `-F` y un **argumento** `/`. Ya hemos visto
opciones que empiezan con un guión (`-`), conocidas como **opciones cortas**, o con dos
guiones (`--`), conocidas como **opciones largas**. las [Opciones] cambian el
comportamiento de un comando y los [Argumentos] le indican al comando sobre qué operar
(por ejemplo, archivos y directorios). A veces, las opciones y los argumentos se
denominan **parámetros**. Un comando puede ser llamado con más de una opción y más de un
argumento, pero un comando no siempre requiere un argumento o una opción.

Puede que a veces veas que se hace referencia a las opciones como **interruptores** o
**banderas**, especialmente para opciones que no tienen argumento. En esta lección
seguiremos utilizando el término *opción*.

Cada parte está separada por espacios. Si omite el espacio entre `ls` y `-F` el shell
buscará un comando llamado `ls-F`, que no existe. Además, las mayúsculas pueden ser
importantes. Por ejemplo, `ls -s` mostrará el tamaño de los ficheros y directorios junto
a los nombres, mientras que `ls -S` ordenará los ficheros y directorios por tamaño, como
se muestra a continuación:

```bash
$ cd ~/Desktop/shell-lesson-data
$ ls -s exercise-data
```

```output
total 28
 4 animal-counts   4 creatures  12 numbers.txt   4 alkanes   4 writing
```

Tenga en cuenta que los tamaños devueltos por `ls -s` están en *bloques*. Como éstos se
definen de forma diferente para los distintos sistemas operativos, es posible que no
obtenga las mismas cifras que en el ejemplo.

```bash
$ ls -S exercise-data
```

```output
animal-counts  creatures  alkanes  writing  numbers.txt
```

Juntando todo esto, nuestro comando `ls -F /` de arriba nos da un listado de archivos y
directorios en el directorio raíz `/`. A continuación se muestra un ejemplo de la salida
que se puede obtener con el comando anterior:

```bash
$ ls -F /
```

```output
Applications/         System/
Library/              Users/
Network/              Volumes/
```

### Nelle's Pipeline: Organización de archivos

Sabiendo todo esto sobre archivos y directorios, Nelle está lista para organizar los
archivos que creará la máquina de ensayo de proteínas.

Crea un directorio llamado `north-pacific-gyre` (para recordar de dónde proceden los
datos), que contendrá los archivos de datos de la máquina de ensayo y sus scripts de
procesamiento de datos.

Cada una de sus muestras físicas está etiquetada según la convención de su laboratorio
con un ID único de diez caracteres, como "NENE01729A". Este ID es el que utiliza en su
registro de recogida para anotar la ubicación, la hora, la profundidad y otras
características de la muestra, por lo que decide utilizarlo en el nombre de archivo de
cada fichero de datos. Como la salida de la máquina de ensayo es texto sin formato,
llamará a sus archivos `NENE01729A.txt`, `NENE01812A.txt`, etcétera. Los 1520 ficheros
irán en el mismo directorio.

Ahora, en su directorio actual `shell-lesson-data`, Nelle puede ver qué archivos tiene
utilizando el comando

```bash
$ ls north-pacific-gyre/
```

Este comando es muy largo de escribir, pero puede dejar que el shell haga la mayor parte
del trabajo a través de lo que se llama **completar pestañas**. Si escribe:

```bash
$ ls nor
```

y, a continuación, pulsa <kbd>Tab</kbd> (la tecla de tabulación de su teclado), el shell
completa automáticamente el nombre del directorio por ella:

```bash
$ ls north-pacific-gyre/
```

Pulsar <kbd>Tab</kbd> de nuevo no hace nada, ya que hay múltiples posibilidades;
pulsando <kbd>Tab</kbd> dos veces aparece una lista de todos los archivos.

Si Nelle pulsa <kbd>G</kbd> y, a continuación, vuelve a pulsar <kbd>Tab</kbd>, el
intérprete de comandos añadirá "goo", ya que todos los archivos que empiezan por "g"
comparten los tres primeros caracteres "goo".

```bash
$ ls north-pacific-gyre/goo
```

Para ver todos esos archivos, puede pulsar <kbd>Tab</kbd> dos veces más.

```bash
ls north-pacific-gyre/goo
goodiff.sh   goostats.sh
```

Esto se llama **completar pestañas**, y lo veremos en muchas otras herramientas a medida
que avancemos.



[Arguments]: https://swcarpentry.github.io/shell-novice/reference.html#argument


:::::::::::::::::::::::::::::::::::::::: keypoints

- El sistema de archivos se encarga de gestionar la información del disco.
- La información se almacena en ficheros, que a su vez se guardan en directorios
  (carpetas).
- Los directorios también pueden almacenar otros directorios, que forman un árbol de
  directorios.
- `pwd` imprime el directorio de trabajo actual del usuario.
- `ls [path]` imprime un listado de un archivo o directorio específico; `ls` por sí solo
  lista el directorio de trabajo actual.
- `cd [path]` cambia el directorio de trabajo actual.
- La mayoría de los comandos aceptan opciones que empiezan por `-`.
- Los nombres de directorio de una ruta se separan con `/` en Unix, pero con `\` en
  Windows.
- `/` por sí solo es el directorio raíz de todo el sistema de archivos.
- Una ruta absoluta especifica una ubicación desde la raíz del sistema de archivos.
- Una ruta relativa especifica una ubicación a partir de la ubicación actual.
- `.` por sí solo significa 'el directorio actual'; `..` significa 'el directorio por
  encima del actual'.

::::::::::::::::::::::::::::::::::::::::::::::::::



