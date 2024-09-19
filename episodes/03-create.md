---
title: Working With Files and Directories
teaching: 30
exercises: 20
---


::::::::::::::::::::::::::::::::::::::: objectives

- Crear una jerarquía de directorios que coincida con un diagrama dado.
- Crea archivos en esa jerarquía utilizando un editor o copiando y renombrando archivos
  existentes.
- Borrar, copiar y mover archivos y/o directorios especificados.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- ¿Cómo puedo crear, copiar y borrar archivos y directorios?
- ¿Cómo puedo editar archivos?

::::::::::::::::::::::::::::::::::::::::::::::::::


## Creación de directorios

Ya sabemos cómo explorar archivos y directorios, pero ¿cómo los creamos?

En este episodio aprenderemos a crear y mover archivos y directorios, utilizando como
ejemplo el directorio `exercise-data/writing`.

### Primer paso: ver dónde estamos y lo que ya tenemos

Deberíamos seguir en el directorio `shell-lesson-data` en el Escritorio, que podemos
comprobar utilizando:

```bash
$ pwd
```

```output
/Users/nelle/Desktop/shell-lesson-data
```

A continuación pasaremos al directorio `exercise-data/writing` y veremos qué contiene:

```bash
$ cd exercise-data/writing/
$ ls -F
```

```output
haiku.txt  LittleWomen.txt
```

### Crear un directorio

Vamos a crear un nuevo directorio llamado `thesis` utilizando el comando `mkdir thesis`
(que no tiene salida):

```bash
$ mkdir thesis
```

Como puede adivinar por su nombre, `mkdir` significa 'crear directorio'. Dado que
`thesis` es una ruta relativa (es decir, no tiene una barra inicial, como
`/what/ever/thesis`), el nuevo directorio se crea en el directorio de trabajo actual:

```bash
$ ls -F
```

```output
haiku.txt  LittleWomen.txt  thesis/
```

Como acabamos de crear el directorio `thesis`, todavía no hay nada en él:

```bash
$ ls -F thesis
```

Tenga en cuenta que `mkdir` no se limita a crear directorios individuales de uno en uno.
La opción `-p` permite a `mkdir` crear un directorio con subdirectorios anidados en una
sola operación:

```bash
$ mkdir -p ../project/data ../project/results
```

La opción `-R` del comando `ls` listará todos los subdirectorios anidados dentro de un
directorio. Utilicemos `ls -FR` para listar recursivamente la nueva jerarquía de
directorios que acabamos de crear en el directorio `project`:

```bash
$ ls -FR ../project
```

```output
../project/:
data/  results/

../project/data:

../project/results:
```

::::::::::::::::::::::::::::::::::::::::: callout

## Dos formas de hacer lo mismo

Utilizar el shell para crear un directorio no es diferente de utilizar un explorador de
archivos. Si abre el directorio actual utilizando el explorador gráfico de archivos de
su sistema operativo, el directorio `thesis` también aparecerá allí. Aunque el shell y
el explorador de archivos son dos formas diferentes de interactuar con los archivos, los
archivos y directorios en sí son los mismos.

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::: callout

## Buenos nombres para archivos y directorios

Los nombres complicados de archivos y directorios pueden hacerte la vida imposible
cuando trabajas en la línea de comandos. Aquí te damos algunos consejos útiles para los
nombres de tus archivos y directorios.

1. No utilice espacios.

Los espacios pueden hacer que un nombre tenga más sentido, pero como los espacios se
utilizan para separar argumentos en la línea de órdenes, es mejor evitarlos en los
nombres de archivos y directorios. Puede utilizar `-` o `_` en su lugar (por ejemplo,
`north-pacific-gyre/` en lugar de `north pacific gyre/`). Para comprobarlo, prueba a
escribir `mkdir north pacific gyre` y mira qué directorio (¡o directorios!) se crean
cuando lo compruebas con `ls -F`.

2. No empiece el nombre con `-` (guión).

Los comandos tratan los nombres que empiezan por `-` como opciones.

3. Utiliza letras, números, `.` (punto), `-` (guión) y `_` (guión bajo).

Muchos otros caracteres tienen significados especiales en la línea de comandos.
Aprenderemos sobre algunos de ellos durante esta lección. Hay caracteres especiales que
pueden hacer que su comando no funcione como se espera e incluso pueden provocar la
pérdida de datos.

Si necesita hacer referencia a nombres de archivos o directorios que contengan espacios
u otros caracteres especiales, debe rodear el nombre con comillas simples
[https://www.gnu.org/software/bash/manual/html_node/Quoting.html] (`''`).

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::: instructor

En ocasiones, los alumnos pueden quedar atrapados en editores de texto de línea de
comandos como Vim, Emacs o Nano. Cerrar el emulador de terminal y abrir uno nuevo puede
resultar frustrante, ya que los alumnos tendrán que volver a navegar hasta la carpeta
correcta. Nuestra recomendación para mitigar este problema es que los instructores
utilicen el mismo editor de texto que los alumnos durante los talleres (en la mayoría de
los casos, Nano).

::::::::::::::::::::::::::::::::::::::::::::::::::

### Crear un archivo de texto

Vamos a cambiar nuestro directorio de trabajo a `thesis` utilizando `cd`, a
continuación, ejecute un editor de texto llamado Nano para crear un archivo llamado
`draft.txt`:

```bash
$ cd thesis
$ nano draft.txt
```

::::::::::::::::::::::::::::::::::::::::: callout

## ¿Qué editor?

Cuando decimos "`nano` es un editor de texto" nos referimos realmente a "texto". Sólo
puede trabajar con datos de caracteres planos, no con tablas, imágenes o cualquier otro
medio amigable para el ser humano. Lo utilizamos en los ejemplos porque es uno de los
editores de texto menos complejos. Sin embargo, debido a esta característica, puede que
no sea lo suficientemente potente o flexible para el trabajo que necesites hacer después
de este taller. En sistemas Unix (como Linux y macOS), muchos programadores utilizan
[Emacs](https://www.gnu.org/software/emacs/) o [Vim](https://www.vim.org/) (ambos
requieren más tiempo de aprendizaje), o un editor gráfico como
[Gedit](https://projects.gnome.org/gedit/) o [VScode](https://code.visualstudio.com/).
En Windows, puede utilizar [Notepad++](https://notepad-plus-plus.org/). Windows también
tiene un editor integrado llamado `notepad` que se puede ejecutar desde la línea de
comandos de la misma manera que `nano` para los propósitos de esta lección.

Independientemente del editor que utilices, necesitarás saber dónde busca y guarda los
archivos. Si lo inicia desde el intérprete de comandos, (probablemente) utilizará su
directorio de trabajo actual como ubicación predeterminada. Si utilizas el menú de
inicio de tu ordenador, puede que quiera guardar los archivos en tu Escritorio o en el
directorio Documentos. Puedes cambiar esta opción desplazándote a otro directorio la
primera vez que "guardes como..."

::::::::::::::::::::::::::::::::::::::::::::::::::

Vamos a escribir unas líneas de texto.

![](fig/nano-screenshot.png){alt="captura de pantalla del editor de texto nano en acción
con el texto Ya no es publicar o perecer, es compartir y prosperar"}

Una vez que estemos satisfechos con nuestro texto, podemos pulsar
<kbd>Ctrl</kbd>\+<kbd>O</kbd> (pulsar la tecla <kbd>Ctrl</kbd> o <kbd>Control</kbd> y,
sin soltarla, pulsar la tecla <kbd>O</kbd>) para escribir nuestros datos en el disco. Se
nos pedirá que proporcionemos un nombre para el archivo que contendrá nuestro texto.
Pulse <kbd>Return</kbd> para aceptar el valor predeterminado sugerido de `draft.txt`.

Una vez guardado nuestro archivo, podemos usar <kbd>Ctrl</kbd>\+<kbd>X</kbd> para salir
del editor y volver al shell.

::::::::::::::::::::::::::::::::::::::::: callout

## Tecla Control, Ctrl o ^

La tecla Control también se denomina tecla "Ctrl". Existen varias formas de describir el
uso de la tecla Control. Por ejemplo, puede ver una instrucción para pulsar la tecla
<kbd>Control</kbd> y, mientras la mantiene pulsada, pulsar la tecla <kbd>X</kbd>,
descrita como cualquiera de:

- `Control-X`
- `Control+X`
- `Ctrl-X`
- `Ctrl+X`
- `^X`
- `C-x`

En nano, en la parte inferior de la pantalla verás `^G Get Help ^O WriteOut`. Esto
significa que puedes usar `Control-G` para obtener ayuda y `Control-O` para guardar tu
archivo.

::::::::::::::::::::::::::::::::::::::::::::::::::

`nano` no deja ninguna salida en pantalla después de salir, pero `ls` muestra ahora que
hemos creado un fichero llamado `draft.txt`:

```bash
$ ls
```

```output
draft.txt
```

::::::::::::::::::::::::::::::::::::::: challenge

## Crear archivos de otra manera

Hemos visto cómo crear archivos de texto utilizando el editor `nano`. Ahora, prueba el
siguiente comando:

```bash
$ touch my_file.txt
```

1. ¿Qué ha hecho el comando `touch`? Cuando miras en tu directorio actual usando el
   explorador de archivos GUI, ¿aparece el archivo?

2. Utilice `ls -l` para inspeccionar los archivos. ¿Qué tamaño tiene `my_file.txt`?

3. ¿Cuándo podría querer crear un archivo de esta forma?

::::::::::::::: solution

## Solución

1. El comando `touch` genera un nuevo fichero llamado `my_file.txt` en su directorio
   actual. Puede observar este archivo recién generado escribiendo `ls` en la línea de
   comandos. también puede ver `my_file.txt` en su explorador de archivos GUI.

2. Cuando inspeccione el archivo con `ls -l`, observe que el tamaño de `my_file.txt` es
   0 bytes. En otras palabras, no contiene datos. Si abre `my_file.txt` con su editor de
   texto, aparecerá en blanco.

3. Algunos programas no generan archivos de salida por sí mismos, sino que requieren que
   ya se hayan generado archivos vacíos. Cuando el programa se ejecuta, busca un archivo
   existente para rellenarlo con su salida. El comando touch le permite generar
   eficientemente un archivo de texto en blanco para ser utilizado por tales programas.

:::::::::::::::::::::::::

Para evitar confusiones más adelante, le sugerimos que elimine el archivo que acaba de
crear antes de continuar con el resto del episodio, ya que de lo contrario los
resultados futuros pueden variar de los que se dan en la lección. Para ello, utilice el
siguiente comando:

```bash
$ rm my_file.txt
```

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::: callout

## ¿Qué hay en un nombre?

Te habrás dado cuenta de que todos los archivos de Nelle se llaman "algo punto algo", y
en esta parte de la lección siempre hemos utilizado la extensión `.txt`. Esto es sólo
una convención; podemos llamar a un archivo `mythesis` o casi cualquier otra cosa que
queramos. Sin embargo, la mayoría de la gente utiliza nombres de dos partes la mayor
parte del tiempo para ayudarles (y a sus programas) a distinguir diferentes tipos de
ficheros. La segunda parte de un nombre de este tipo se denomina **extensión de nombre
de fichero** e indica qué tipo de datos contiene el fichero: `.txt` señala un fichero de
texto plano, `.pdf` indica un documento PDF, `.cfg` es un fichero de configuración lleno
de parámetros para algún programa u otro, `.png` es una imagen PNG, etc.

Se trata sólo de una convención, aunque importante. Los archivos sólo contienen bytes;
depende de nosotros y de nuestros programas interpretar esos bytes de acuerdo con las
reglas para archivos de texto plano, documentos PDF, archivos de configuración,
imágenes, etc.

Nombrar una imagen PNG de una ballena como `whale.mp3` no la convierte mágicamente en
una grabación del canto de una ballena, aunque *puede* hacer que el sistema operativo
asocie el archivo con un programa reproductor de música. En este caso, si alguien hace
doble clic en `whale.mp3` en un programa explorador de archivos, el reproductor de
música intentará automáticamente (y por error) abrir el archivo `whale.mp3`.

::::::::::::::::::::::::::::::::::::::::::::::::::

## Moviendo archivos y directorios

Volviendo al directorio `shell-lesson-data/exercise-data/writing`,

```bash
$ cd ~/Desktop/shell-lesson-data/exercise-data/writing
```

En nuestro directorio `thesis` tenemos un fichero `draft.txt` que no es un nombre
particularmente informativo, así que cambiemos el nombre del fichero usando `mv`, que es
la abreviatura de `mover`:

```bash
$ mv thesis/draft.txt thesis/quotes.txt
```

El primer argumento le dice a `mv` lo que estamos "moviendo", mientras que el segundo es
a dónde debe ir. En este caso, estamos moviendo `thesis/draft.txt` a
`thesis/quotes.txt`, lo que tiene el mismo efecto que renombrar el archivo.
Efectivamente, `ls` nos muestra que `thesis` contiene ahora un fichero llamado
`quotes.txt`:

```bash
$ ls thesis
```

```output
quotes.txt
```

Hay que tener cuidado al especificar el nombre del archivo de destino, ya que `mv`
sobrescribirá silenciosamente cualquier archivo existente con el mismo nombre, lo que
podría provocar la pérdida de datos. Por defecto, `mv` no pedirá confirmación antes de
sobrescribir archivos. Sin embargo, una opción adicional, `mv -i` (o `mv
--interactive`), hará que `mv` solicite dicha confirmación.

Tenga en cuenta que `mv` también funciona con directorios.

Movamos `quotes.txt` al directorio de trabajo actual. Usamos `mv` una vez más, pero esta
vez usaremos sólo el nombre de un directorio como segundo argumento para decirle a `mv`
que queremos mantener el nombre del fichero pero ponerlo en un lugar nuevo. (Por eso el
comando se llama 'mover'.) En este caso, el nombre de directorio que usamos es el nombre
de directorio especial `.` que mencionamos antes.

```bash
$ mv thesis/quotes.txt .
```

El efecto es mover el fichero del directorio en el que estaba al directorio de trabajo
actual. `ls` nos muestra ahora que `thesis` está vacío:

```bash
$ ls thesis
```

```output
$
```

Alternativamente, podemos confirmar que el fichero `quotes.txt` ya no está presente en
el directorio `thesis` intentando listarlo explícitamente:

```bash
$ ls thesis/quotes.txt
```

```error
ls: cannot access 'thesis/quotes.txt': No such file or directory
```

`ls` con un nombre de fichero o directorio como argumento sólo lista el fichero o
directorio solicitado. Si el fichero dado como argumento no existe, el shell devuelve un
error como vimos anteriormente. Podemos usar esto para ver que `quotes.txt` está ahora
presente en nuestro directorio actual:

```bash
$ ls quotes.txt
```

```output
quotes.txt
```

::::::::::::::::::::::::::::::::::::::: challenge

## Mover archivos a una nueva carpeta

Tras ejecutar los siguientes comandos, Jamie se da cuenta de que ha colocado los
archivos `sucrose.dat` y `maltose.dat` en la carpeta equivocada. Los archivos deberían
haberse colocado en la carpeta `raw`.

```bash
$ ls -F
 analyzed/ raw/
$ ls -F analyzed
fructose.dat glucose.dat maltose.dat sucrose.dat
$ cd analyzed
```

Rellene los espacios en blanco para mover estos archivos a la carpeta `raw/` (es decir,
aquella en la que olvidó ponerlos)

```bash
$ mv sucrose.dat maltose.dat ____/____
```

::::::::::::::: solution

## Solución

```bash
$ mv sucrose.dat maltose.dat ../raw
```

Recordemos que `..` se refiere al directorio padre (es decir, uno por encima del
directorio actual) y que `.` se refiere al directorio actual.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Copiar archivos y directorios

El comando `cp` funciona de forma muy parecida a `mv`, excepto que copia un fichero en
lugar de moverlo. Podemos comprobar que ha hecho lo correcto utilizando `ls` con dos
rutas como argumentos --- como la mayoría de los comandos Unix, a `ls` se le pueden dar
múltiples rutas a la vez:

```bash
$ cp quotes.txt thesis/quotations.txt
$ ls quotes.txt thesis/quotations.txt
```

```output
quotes.txt   thesis/quotations.txt
```

También podemos copiar un directorio y todo su contenido utilizando la opción
[recursiva](https://en.wikipedia.org/wiki/Recursion) `-r`, por ejemplo, para hacer una
copia de seguridad de un directorio:

```bash
$ cp -r thesis thesis_backup
```

Podemos comprobar el resultado listando el contenido de los directorios `thesis` y
`thesis_backup`:

```bash
$ ls thesis thesis_backup
```

```output
thesis:
quotations.txt

thesis_backup:
quotations.txt
```

Es importante incluir la bandera `-r`. Si desea copiar un directorio y omite esta
opción, aparecerá un mensaje indicando que el directorio se ha omitido porque `-r not
specified`.

``` bash
$ cp thesis thesis_backup
cp: -r not specified; omitting directory 'thesis'
```


::::::::::::::::::::::::::::::::::::::: challenge

## Renombrar archivos

Supongamos que ha creado un archivo de texto plano en su directorio actual para contener
una lista de las pruebas estadísticas que necesitará realizar para analizar sus datos, y
lo ha llamado `statstics.txt`

Después de crear y guardar este archivo, te das cuenta de que has escrito mal el nombre
del archivo Quieres corregir el error, ¿cuál de los siguientes comandos podrías utilizar
para hacerlo?

1. `cp statstics.txt statistics.txt`
2. `mv statstics.txt statistics.txt`
3. `mv statstics.txt .`
4. `cp statstics.txt .`

::::::::::::::: solution

## Solución

1. No. Aunque esto crearía un archivo con el nombre correcto, el archivo con el nombre
   incorrecto seguiría existiendo en el directorio y habría que borrarlo.
2. Sí, esto funcionaría para renombrar el archivo.
3. No, el punto(.) indica dónde mover el archivo, pero no proporciona un nuevo nombre de
   archivo; no se pueden crear nombres de archivo idénticos.
4. No, el punto(.) indica dónde copiar el archivo, pero no proporciona un nuevo nombre
   de archivo; no se pueden crear nombres de archivo idénticos.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Traslado y copia

¿Cuál es la salida del comando de cierre `ls` en la secuencia que se muestra a
continuación?

```bash
$ pwd
```

```output
/Users/jamie/data
```

```bash
$ ls
```

```output
proteins.dat
```

```bash
$ mkdir recombined
$ mv proteins.dat recombined/
$ cp recombined/proteins.dat ../proteins-saved.dat
$ ls
```

1. `proteins-saved.dat recombined`
2. `recombined`
3. `proteins.dat recombined`
4. `proteins-saved.dat`

::::::::::::::: solution

## Solución

Empezamos en el directorio `/Users/jamie/data`, y creamos una nueva carpeta llamada
`recombined`. La segunda línea mueve (`mv`) el fichero `proteins.dat` a la nueva carpeta
(`recombined`). La tercera línea hace una copia del fichero que acabamos de mover. La
parte complicada aquí es dónde se ha copiado el archivo. Recuerda que `..` significa
'subir un nivel', así que el fichero copiado está ahora en `/Users/jamie`. Tenga en
cuenta que `..` se interpreta con respecto al directorio de trabajo actual, **no** con
respecto a la ubicación del fichero que se está copiando. Así, lo único que se mostrará
usando ls (en `/Users/jamie/data`) es la carpeta recombinada.

1. No, véase la explicación anterior. `proteins-saved.dat` se encuentra en
   `/Users/jamie`
2. Sí
3. No, véase la explicación anterior. `proteins.dat` se encuentra en
   `/Users/jamie/data/recombined`
4. No, véase la explicación anterior. `proteins-saved.dat` se encuentra en
   `/Users/jamie`

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Eliminar archivos y directorios

Volviendo al directorio `shell-lesson-data/exercise-data/writing`, vamos a ordenarlo
eliminando el archivo `quotes.txt` que hemos creado. El comando Unix que utilizaremos
para ello es `rm` (abreviatura de 'remove'):

```bash
$ rm quotes.txt
```

Podemos confirmar que el archivo se ha ido usando `ls`:

```bash
$ ls quotes.txt
```

```error
ls: cannot access 'quotes.txt': No such file or directory
```

::::::::::::::::::::::::::::::::::::::::: callout

## Borrar es para siempre

El shell de Unix no tiene una papelera de la que podamos recuperar los archivos borrados
(aunque la mayoría de las interfaces gráficas de Unix sí la tienen). En su lugar, cuando
borramos archivos, éstos se desvinculan del sistema de archivos para que se pueda
reciclar su espacio de almacenamiento en disco. Existen herramientas para encontrar y
recuperar archivos borrados, pero no hay garantía de que funcionen en una situación
concreta, ya que el ordenador puede reciclar el espacio del archivo en el disco
inmediatamente.

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Utilizar `rm` con seguridad

¿Qué ocurre cuando ejecutamos `rm -i thesis_backup/quotations.txt`? ¿Por qué querríamos
esta protección cuando utilizamos `rm`?

::::::::::::::: solution

## Solución

```output
rm: remove regular file 'thesis_backup/quotations.txt'? y
```

La opción `-i` preguntará antes (de cada) eliminación (utilice <kbd>Y</kbd> para
confirmar la eliminación o <kbd>N</kbd> para conservar el archivo). El shell Unix no
tiene papelera, por lo que todos los archivos eliminados desaparecerán para siempre.
Utilizando la opción `-i`, tenemos la posibilidad de comprobar que estamos borrando sólo
los ficheros que queremos eliminar.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

Si intentamos eliminar el directorio `thesis` utilizando `rm thesis`, obtendremos un
mensaje de error:

```bash
$ rm thesis
```

```error
rm: cannot remove 'thesis': Is a directory
```

Esto ocurre porque `rm` por defecto sólo funciona en ficheros, no en directorios.

`rm` puede eliminar un directorio *y todo su contenido* si utilizamos la opción
recursiva `-r`, y lo hará *sin ninguna petición de confirmación*:

```bash
$ rm -r thesis
```

Dado que no hay forma de recuperar ficheros borrados usando el shell, `rm -r` *debe
usarse con mucha precaución* (puedes considerar añadir la opción interactiva `rm -r
-i`).

## Operaciones con múltiples ficheros y directorios

A menudo es necesario copiar o mover varios archivos a la vez. Esto se puede hacer
proporcionando una lista de nombres de ficheros individuales, o especificando un patrón
de nombres utilizando comodines. Los comodines son caracteres especiales que pueden
utilizarse para representar caracteres o conjuntos de caracteres desconocidos al navegar
por el sistema de ficheros Unix.

::::::::::::::::::::::::::::::::::::::: challenge

## Copia con varios nombres de archivo

Para este ejercicio, puede probar los comandos en el directorio
`shell-lesson-data/exercise-data`.

En el siguiente ejemplo, ¿qué hace `cp` cuando se le dan varios nombres de archivo y un
nombre de directorio?

```bash
$ mkdir backup
$ cp creatures/minotaur.dat creatures/unicorn.dat backup/
```

En el siguiente ejemplo, ¿qué hace `cp` cuando se le dan tres o más nombres de archivo?

```bash
$ cd creatures
$ ls -F
```

```output
basilisk.dat  minotaur.dat  unicorn.dat
```

```bash
$ cp minotaur.dat unicorn.dat basilisk.dat
```

::::::::::::::: solution

## Solución

Si se da más de un nombre de fichero seguido de un nombre de directorio (es decir, el
directorio de destino debe ser el último argumento), `cp` copia los ficheros en el
directorio nombrado.

Si se le dan tres nombres de fichero, `cp` lanza un error como el siguiente, porque
espera un nombre de directorio como último argumento.

```error
cp: target 'basilisk.dat' is not a directory
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

### Uso de comodines para acceder a varios archivos a la vez

::::::::::::::::::::::::::::::::::::::::: callout

## Comodines

`*` es un **código comodín**, que representa otros cero o más caracteres. Consideremos
el directorio `shell-lesson-data/exercise-data/alkanes`: `*.pdb` representa a
`ethane.pdb`, `propane.pdb` y a todos los archivos que terminan en '.pdb'. En cambio,
`p*.pdb` sólo representa a `pentane.pdb` y `propane.pdb`, ya que la 'p' que aparece al
principio sólo puede representar nombres de fichero que empiecen por la letra 'p'.

`?` también es un comodín, pero representa exactamente un carácter. Así, `?ethane.pdb`
podría representar `methane.pdb`, mientras que `*ethane.pdb` representa tanto
`ethane.pdb` como `methane.pdb`.

Los comodines pueden utilizarse combinados entre sí. Por ejemplo, `???ane.pdb` indica
tres caracteres seguidos de `ane.pdb`, dando `cubane.pdb ethane.pdb octane.pdb`.

Cuando el shell ve un comodín, lo expande para crear una lista de nombres de archivo
coincidentes *antes* de ejecutar el comando precedente. Como excepción, si una expresión
comodín no coincide con ningún archivo, Bash pasará la expresión como argumento al
comando tal cual. Por ejemplo, escribir `ls *.pdf` en el directorio `alkanes` (que
contiene sólo ficheros cuyos nombres terminan en `.pdb`) da como resultado un mensaje de
error que indica que no existe ningún fichero llamado `*.pdf`. Sin embargo, generalmente
comandos como `wc` y `ls` ven las listas de nombres de ficheros que coinciden con estas
expresiones, pero no los comodines en sí. Es el shell, no los otros programas, el que
expande los comodines.

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Lista de nombres de archivo que coinciden con un patrón

Cuando se ejecuta en el directorio `alkanes`, ¿qué comando(s) `ls` producirá(n) este
resultado?

`ethane.pdb methane.pdb`

1. `ls *t*ane.pdb`
2. `ls *t?ne.*`
3. `ls *t??ne.pdb`
4. `ls ethane.*`

::::::::::::::: solution

## Solución

La solución es `3.`

`1.` muestra todos los ficheros cuyos nombres contienen cero o más caracteres (`*`)
seguidos de la letra `t`, luego cero o más caracteres (`*`) seguidos de `ane.pdb`. Esto
da `ethane.pdb methane.pdb octane.pdb pentane.pdb`.

`2.` muestra todos los ficheros cuyos nombres empiezan por cero o más caracteres (`*`)
seguidos de la letra `t`, luego un solo carácter (`?`), luego `ne.` seguido de cero o
más caracteres (`*`). Esto nos dará `octane.pdb` y `pentane.pdb` pero no coincide con
nada que termine en `thane.pdb`.

`3.` soluciona los problemas de la opción 2 haciendo coincidir dos caracteres (`??`)
entre `t` y `ne`. Ésta es la solución.

`4.` sólo muestra los archivos que empiezan por `ethane.`.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Más sobre comodines

Sam tiene un directorio que contiene datos de calibración, conjuntos de datos y
descripciones de los conjuntos de datos:

```bash
.
├── 2015-10-23-calibration.txt
├── 2015-10-23-dataset1.txt
├── 2015-10-23-dataset2.txt
├── 2015-10-23-dataset_overview.txt
├── 2015-10-26-calibration.txt
├── 2015-10-26-dataset1.txt
├── 2015-10-26-dataset2.txt
├── 2015-10-26-dataset_overview.txt
├── 2015-11-23-calibration.txt
├── 2015-11-23-dataset1.txt
├── 2015-11-23-dataset2.txt
├── 2015-11-23-dataset_overview.txt
├── backup
│   ├── calibration
│   └── datasets
└── send_to_bob
    ├── all_datasets_created_on_a_23rd
    └── all_november_files
```

Antes de irse a otra excursión, quiere hacer una copia de seguridad de sus datos y
enviar algunos conjuntos de datos a su colega Bob. Sam utiliza los siguientes comandos
para realizar el trabajo:

```bash
$ cp *dataset* backup/datasets
$ cp ____calibration____ backup/calibration
$ cp 2015-____-____ send_to_bob/all_november_files/
$ cp ____ send_to_bob/all_datasets_created_on_a_23rd/
```

Ayuda a Sam rellenando los espacios en blanco.

La estructura de directorios resultante debería ser la siguiente

```bash
.
├── 2015-10-23-calibration.txt
├── 2015-10-23-dataset1.txt
├── 2015-10-23-dataset2.txt
├── 2015-10-23-dataset_overview.txt
├── 2015-10-26-calibration.txt
├── 2015-10-26-dataset1.txt
├── 2015-10-26-dataset2.txt
├── 2015-10-26-dataset_overview.txt
├── 2015-11-23-calibration.txt
├── 2015-11-23-dataset1.txt
├── 2015-11-23-dataset2.txt
├── 2015-11-23-dataset_overview.txt
├── backup
│   ├── calibration
│   │   ├── 2015-10-23-calibration.txt
│   │   ├── 2015-10-26-calibration.txt
│   │   └── 2015-11-23-calibration.txt
│   └── datasets
│       ├── 2015-10-23-dataset1.txt
│       ├── 2015-10-23-dataset2.txt
│       ├── 2015-10-23-dataset_overview.txt
│       ├── 2015-10-26-dataset1.txt
│       ├── 2015-10-26-dataset2.txt
│       ├── 2015-10-26-dataset_overview.txt
│       ├── 2015-11-23-dataset1.txt
│       ├── 2015-11-23-dataset2.txt
│       └── 2015-11-23-dataset_overview.txt
└── send_to_bob
    ├── all_datasets_created_on_a_23rd
    │   ├── 2015-10-23-dataset1.txt
    │   ├── 2015-10-23-dataset2.txt
    │   ├── 2015-10-23-dataset_overview.txt
    │   ├── 2015-11-23-dataset1.txt
    │   ├── 2015-11-23-dataset2.txt
    │   └── 2015-11-23-dataset_overview.txt
    └── all_november_files
        ├── 2015-11-23-calibration.txt
        ├── 2015-11-23-dataset1.txt
        ├── 2015-11-23-dataset2.txt
        └── 2015-11-23-dataset_overview.txt
```

::::::::::::::: solution

## Solución

```bash
$ cp *calibration.txt backup/calibration
$ cp 2015-11-* send_to_bob/all_november_files/
$ cp *-23-dataset* send_to_bob/all_datasets_created_on_a_23rd/
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Organización de directorios y archivos

Jamie está trabajando en un proyecto y ve que sus archivos no están muy bien
organizados:

```bash
$ ls -F
```

```output
analyzed/  fructose.dat    raw/   sucrose.dat
```

Los archivos `fructose.dat` y `sucrose.dat` contienen los resultados de su análisis de
datos. ¿Qué comando(s) de los tratados en esta lección necesita ejecutar para que los
comandos siguientes produzcan la salida mostrada?

```bash
$ ls -F
```

```output
analyzed/   raw/
```

```bash
$ ls analyzed
```

```output
fructose.dat    sucrose.dat
```

::::::::::::::: solution

## Solución

```bash
mv *.dat analyzed
```

Jamie necesita mover sus ficheros `fructose.dat` y `sucrose.dat` al directorio
`analyzed`. El intérprete de comandos expandirá \*.dat para que coincida con todos los
archivos .dat del directorio actual. A continuación, el comando `mv` mueve la lista de
archivos .dat al directorio `analizado`.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Reproducir una estructura de carpetas

Estás comenzando un nuevo experimento y te gustaría duplicar la estructura de
directorios de tu experimento anterior para poder añadir nuevos datos.

Supongamos que el experimento anterior está en una carpeta llamada `2016-05-18`, que
contiene una carpeta `data` que a su vez contiene carpetas llamadas `raw` y `processed`
que contienen ficheros de datos. El objetivo es copiar la estructura de carpetas de la
carpeta `2016-05-18` en una carpeta llamada `2016-05-20` de modo que la estructura final
de directorios tenga este aspecto:

```output
2016-05-20/
└── data
   ├── processed
   └── raw
```

¿Cuál de los siguientes comandos lograría este objetivo? ¿Qué harían los demás comandos?

```bash
$ mkdir 2016-05-20
$ mkdir 2016-05-20/data
$ mkdir 2016-05-20/data/processed
$ mkdir 2016-05-20/data/raw
```

```bash
$ mkdir 2016-05-20
$ cd 2016-05-20
$ mkdir data
$ cd data
$ mkdir raw processed
```

```bash
$ mkdir 2016-05-20/data/raw
$ mkdir 2016-05-20/data/processed
```

```bash
$ mkdir -p 2016-05-20/data/raw
$ mkdir -p 2016-05-20/data/processed
```

```bash
$ mkdir 2016-05-20
$ cd 2016-05-20
$ mkdir data
$ mkdir raw processed
```

::::::::::::::: solution

## Solución

Los dos primeros conjuntos de comandos logran este objetivo. El primer conjunto utiliza
rutas relativas para crear el directorio de nivel superior antes que los subdirectorios.

El tercer conjunto de comandos dará un error porque el comportamiento por defecto de
`mkdir` no creará un subdirectorio de un directorio inexistente: las carpetas de nivel
intermedio deben crearse primero.

El cuarto conjunto de comandos consigue este objetivo. Recuerde que la opción `-p`,
seguida de una ruta de uno o más directorios, hará que `mkdir` cree los subdirectorios
intermedios que sean necesarios.

El último conjunto de comandos genera los directorios "raw" y "processed" al mismo nivel
que el directorio "data".

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- `cp [old] [new]` copia un archivo.
- `mkdir [path]` crea un nuevo directorio.
- `mv [old] [new]` mueve (renombra) un archivo o directorio.
- `rm [path]` elimina (borra) un archivo.
- `*` coincide con cero o más caracteres en un nombre de archivo, por lo que `*.txt`
  coincide con todos los archivos que terminan en `.txt`.
- `?` coincide con cualquier carácter de un nombre de archivo, por lo que `?.txt`
  coincide con `a.txt` pero no con `any.txt`.
- El uso de la tecla Control puede describirse de muchas maneras, incluyendo `Ctrl-X`,
  `Control-X` y `^X`.
- El shell no tiene papelera: una vez que se borra algo, desaparece de verdad.
- La mayoría de los nombres de archivo son `something.extension`. La extensión no es
  obligatoria y no garantiza nada, pero normalmente se utiliza para indicar el tipo de
  datos del archivo.
- Dependiendo del tipo de trabajo que realices, puede que necesites un editor de texto
  más potente que Nano.

::::::::::::::::::::::::::::::::::::::::::::::::::

