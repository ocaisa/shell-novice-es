---
title: Shell Scripts
teaching: 30
exercises: 15
---


::::::::::::::::::::::::::::::::::::::: objectives

- Escribe un script de shell que ejecute un comando o una serie de comandos para un
  conjunto fijo de archivos.
- Ejecuta un script de shell desde la línea de comandos.
- Escribe un script de shell que opere sobre un conjunto de ficheros definidos por el
  usuario en la línea de comandos.
- Cree pipelines que incluyan scripts de shell que usted, y otros, hayan escrito.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- ¿Cómo puedo guardar y reutilizar comandos?

::::::::::::::::::::::::::::::::::::::::::::::::::

Por fin estamos listos para ver qué hace del shell un entorno de programación tan
potente. Vamos a tomar los comandos que repetimos con frecuencia y guardarlos en
archivos para poder volver a ejecutar todas esas operaciones más tarde tecleando un solo
comando. Por razones históricas, un montón de comandos guardados en un archivo se suele
llamar **script de shell**, pero no te equivoques: en realidad son pequeños programas.

Escribir guiones de shell no sólo agilizará tu trabajo, sino que además no tendrás que
volver a escribir los mismos comandos una y otra vez. También lo hará más preciso (menos
posibilidades de errores tipográficos) y más reproducible. Si vuelves a tu trabajo más
tarde (o si alguien más encuentra tu trabajo y quiere basarse en él), podrás reproducir
los mismos resultados simplemente ejecutando tu script, en lugar de tener que recordar o
volver a escribir una larga lista de comandos.

Empecemos volviendo a `alkanes/` y creando un nuevo fichero, `middle.sh` que se
convertirá en nuestro script de shell:

```bash
$ cd alkanes
$ nano middle.sh
```

El comando `nano middle.sh` abre el fichero `middle.sh` dentro del editor de texto
'nano' (que se ejecuta dentro del shell). Si el fichero no existe, se creará. Podemos
utilizar el editor de texto para editar directamente el archivo insertando la siguiente
línea:

```source
head -n 15 octane.pdb | tail -n 5
```

Esta es una variación de la tubería que construimos antes, que selecciona las líneas
11-15 del fichero `octane.pdb`. Recuerde que *no* lo estamos ejecutando como un comando
todavía; sólo estamos incorporando los comandos en un archivo.

A continuación guardamos el fichero (`Ctrl-O` en nano) y salimos del editor de texto
(`Ctrl-X` en nano). Comprueba que el directorio `alkanes` contiene ahora un fichero
llamado `middle.sh`.

Una vez guardado el fichero, podemos pedir al intérprete de órdenes que ejecute los
comandos que contiene. Nuestro shell se llama `bash`, así que ejecutamos el siguiente
comando:

```bash
$ bash middle.sh
```

```output
ATOM      9  H           1      -4.502   0.681   0.785  1.00  0.00
ATOM     10  H           1      -5.254  -0.243  -0.537  1.00  0.00
ATOM     11  H           1      -4.357   1.252  -0.895  1.00  0.00
ATOM     12  H           1      -3.009  -0.741  -1.467  1.00  0.00
ATOM     13  H           1      -3.172  -1.337   0.206  1.00  0.00
```

Con toda seguridad, la salida de nuestro script es exactamente la que obtendríamos si
ejecutáramos directamente esa tubería.

::::::::::::::::::::::::::::::::::::::::: callout

## Texto vs. Lo que sea

Solemos llamar "editores de texto" a programas como Microsoft Word o LibreOffice Writer,
pero hay que tener un poco más de cuidado cuando se trata de programar. Por defecto,
Microsoft Word utiliza archivos `.docx` para almacenar no sólo texto, sino también
información de formato sobre fuentes, encabezados, etcétera. Esta información adicional
no se almacena como caracteres y no significa nada para herramientas como `head`, que
espera que los archivos de entrada sólo contengan las letras, dígitos y signos de
puntuación de un teclado de ordenador estándar. Por lo tanto, al editar programas, debe
utilizar un editor de texto sin formato o tener cuidado de guardar los archivos como
texto sin formato.


::::::::::::::::::::::::::::::::::::::::::::::::::

¿Qué pasa si queremos seleccionar líneas de un fichero arbitrario? Podríamos editar
`middle.sh` cada vez para cambiar el nombre del fichero, pero eso probablemente nos
llevaría más tiempo que escribir el comando de nuevo en el shell y ejecutarlo con un
nuevo nombre de fichero. En su lugar, vamos a editar `middle.sh` y hacerlo más versátil:

```bash
$ nano middle.sh
```

Ahora, dentro de "nano", sustituye el texto `octane.pdb` por la variable especial
llamada `$1`:

```source
head -n 15 "$1" | tail -n 5
```

Dentro de un script de shell, `$1` significa `el primer nombre de archivo (u otro
argumento) de la línea de comandos`. Ahora podemos ejecutar nuestro script así:

```bash
$ bash middle.sh octane.pdb
```

```output
ATOM      9  H           1      -4.502   0.681   0.785  1.00  0.00
ATOM     10  H           1      -5.254  -0.243  -0.537  1.00  0.00
ATOM     11  H           1      -4.357   1.252  -0.895  1.00  0.00
ATOM     12  H           1      -3.009  -0.741  -1.467  1.00  0.00
ATOM     13  H           1      -3.172  -1.337   0.206  1.00  0.00
```

o en un archivo diferente como éste:

```bash
$ bash middle.sh pentane.pdb
```

```output
ATOM      9  H           1       1.324   0.350  -1.332  1.00  0.00
ATOM     10  H           1       1.271   1.378   0.122  1.00  0.00
ATOM     11  H           1      -0.074  -0.384   1.288  1.00  0.00
ATOM     12  H           1      -0.048  -1.362  -0.205  1.00  0.00
ATOM     13  H           1      -1.183   0.500  -1.412  1.00  0.00
```

::::::::::::::::::::::::::::::::::::::::: callout

## Comillas dobles alrededor de los argumentos

Por la misma razón por la que ponemos la variable de bucle entre comillas dobles, en
caso de que el nombre de archivo contenga algún espacio, rodeamos `$1` con comillas
dobles.


::::::::::::::::::::::::::::::::::::::::::::::::::

Actualmente, tenemos que editar `middle.sh` cada vez que queremos ajustar el rango de
líneas que se devuelve. Arreglémoslo configurando nuestro script para que utilice tres
argumentos de línea de comandos. Después del primer argumento de línea de comandos
(`$1`), cada argumento adicional que proporcionemos será accesible a través de las
variables especiales `$1`, `$2`, `$3`, que se refieren al primer, segundo y tercer
argumento de línea de comandos, respectivamente.

Sabiendo esto, podemos utilizar argumentos adicionales para definir el rango de líneas
que se pasarán a `head` y `tail` respectivamente:

```bash
$ nano middle.sh
```

```source
head -n "$2" "$1" | tail -n "$3"
```

Ya podemos correr:

```bash
$ bash middle.sh pentane.pdb 15 5
```

```output
ATOM      9  H           1       1.324   0.350  -1.332  1.00  0.00
ATOM     10  H           1       1.271   1.378   0.122  1.00  0.00
ATOM     11  H           1      -0.074  -0.384   1.288  1.00  0.00
ATOM     12  H           1      -0.048  -1.362  -0.205  1.00  0.00
ATOM     13  H           1      -1.183   0.500  -1.412  1.00  0.00
```

Cambiando los argumentos de nuestro comando, podemos cambiar el comportamiento de
nuestro script:

```bash
$ bash middle.sh pentane.pdb 20 5
```

```output
ATOM     14  H           1      -1.259   1.420   0.112  1.00  0.00
ATOM     15  H           1      -2.608  -0.407   1.130  1.00  0.00
ATOM     16  H           1      -2.540  -1.303  -0.404  1.00  0.00
ATOM     17  H           1      -3.393   0.254  -0.321  1.00  0.00
TER      18              1
```

Esto funciona, pero puede que la próxima persona que lea `middle.sh` tarde un momento en
darse cuenta de lo que hace. Podemos mejorar nuestro script añadiendo algunos
**comentarios** en la parte superior:

```bash
$ nano middle.sh
```

```source
# Select lines from the middle of a file.
# Usage: bash middle.sh filename end_line num_lines
head -n "$2" "$1" | tail -n "$3"
```

Un comentario empieza con un carácter `#` y llega hasta el final de la línea. El
ordenador ignora los comentarios, pero tienen un valor incalculable para ayudar a la
gente (incluido tu futuro yo) a entender y utilizar los scripts. La única advertencia es
que cada vez que modifiques el script, debes comprobar que el comentario sigue siendo
correcto. Una explicación que lleve al lector en la dirección equivocada es peor que
ninguna.

¿Y si queremos procesar muchos archivos en una sola cadena? Por ejemplo, si queremos
ordenar nuestros ficheros `.pdb` por longitud, teclearíamos

```bash
$ wc -l *.pdb | sort -n
```

porque `wc -l` lista el número de líneas de los ficheros (recuerde que `wc` significa
'recuento de palabras', añadiendo la opción `-l` significa 'recuento de líneas' en su
lugar) y `sort -n` ordena las cosas numéricamente. Podríamos poner esto en un fichero,
pero entonces sólo ordenaría una lista de ficheros `.pdb` en el directorio actual. Si
queremos obtener una lista ordenada de otros tipos de ficheros, necesitamos una forma de
introducir todos esos nombres en el script. No podemos usar `$1`, `$2`, etc. porque no
sabemos cuántos ficheros hay. En su lugar, usamos la variable especial `$@`, que
significa, 'Todos los argumentos de línea de comandos del script de shell'. También
debemos poner `$@` entre comillas dobles para manejar el caso de argumentos que
contengan espacios (`"$@"` es sintaxis especial y es equivalente a `"$1"` `"$2"` ...).

He aquí un ejemplo:

```bash
$ nano sorted.sh
```

```source
# Sort files by their length.
# Usage: bash sorted.sh one_or_more_filenames
wc -l "$@" | sort -n
```

```bash
$ bash sorted.sh *.pdb ../creatures/*.dat
```

```output
9 methane.pdb
12 ethane.pdb
15 propane.pdb
20 cubane.pdb
21 pentane.pdb
30 octane.pdb
163 ../creatures/basilisk.dat
163 ../creatures/minotaur.dat
163 ../creatures/unicorn.dat
596 total
```

::::::::::::::::::::::::::::::::::::::: challenge

## Lista de especies únicas

Leah tiene varios cientos de archivos de datos, cada uno de los cuales tiene el
siguiente formato:

```source
2013-11-05,deer,5
2013-11-05,rabbit,22
2013-11-05,raccoon,7
2013-11-06,rabbit,19
2013-11-06,deer,2
2013-11-06,fox,1
2013-11-07,rabbit,18
2013-11-07,bear,1
```

Un ejemplo de este tipo de archivo aparece en
`shell-lesson-data/exercise-data/animal-counts/animals.csv`.

Podemos utilizar el comando `cut -d , -f 2 animals.csv | sort | uniq` para producir la
especie única en `animals.csv`. Para evitar tener que escribir esta serie de comandos
cada vez, un científico puede optar por escribir un script en su lugar.

Escriba un script de shell llamado `species.sh` que tome cualquier número de nombres de
archivo como argumentos de línea de comandos y utilice una variación del comando
anterior para imprimir una lista de las especies únicas que aparecen en cada uno de esos
archivos por separado.

::::::::::::::: solution

## Solución

```bash
# Script to find unique species in csv files where species is the second data field
# This script accepts any number of file names as command line arguments

# Loop over all files
for file in $@
do
    echo "Unique species in $file:"
    # Extract species names
    cut -d , -f 2 $file | sort | uniq
done
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

Supongamos que acabamos de ejecutar una serie de comandos que han hecho algo útil, por
ejemplo, crear un gráfico que nos gustaría utilizar en un artículo. Nos gustaría poder
volver a crear el gráfico más adelante si lo necesitamos, así que queremos guardar los
comandos en un archivo. En lugar de escribirlos de nuevo (y potencialmente equivocarnos)
podemos hacer esto:

```bash
$ history | tail -n 5 > redo-figure-3.sh
```

El archivo `redo-figure-3.sh` contiene ahora:

```source
297 bash goostats.sh NENE01729B.txt stats-NENE01729B.txt
298 bash goodiff.sh stats-NENE01729B.txt /data/validated/01729.txt > 01729-differences.txt
299 cut -d ',' -f 2-3 01729-differences.txt > 01729-time-series.txt
300 ygraph --format scatter --color bw --borders none 01729-time-series.txt figure-3.png
301 history | tail -n 5 > redo-figure-3.sh
```

Después de un momento de trabajo en un editor para eliminar los números de serie de los
comandos, y para eliminar la línea final donde llamamos al comando `history`, tenemos un
registro completamente exacto de cómo creamos esa figura.

::::::::::::::::::::::::::::::::::::::: challenge

## ¿Por qué registrar los comandos en el historial antes de ejecutarlos?

Si ejecuta el comando:

```bash
$ history | tail -n 5 > recent.sh
```

el último comando del archivo es el propio comando `history`, es decir, el shell ha
añadido `history` al registro de comandos antes de ejecutarlo. De hecho, el shell
*siempre* añade comandos al registro antes de ejecutarlos. ¿Por qué crees que lo hace?

::::::::::::::: solution

## Solución

Si un comando hace que algo se bloquee o se cuelgue, puede ser útil saber cuál era ese
comando para investigar el problema. Si el comando sólo se registrara después de
ejecutarlo, no tendríamos un registro del último comando ejecutado en caso de que se
produjera un bloqueo.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

En la práctica, la mayoría de las personas desarrollan guiones de shell ejecutando
comandos en el indicador del shell unas cuantas veces para asegurarse de que están
haciendo lo correcto y, a continuación, guardándolos en un archivo para reutilizarlos.
Este estilo de trabajo permite a la gente reciclar lo que descubren sobre sus datos y su
flujo de trabajo con una llamada a `history` y un poco de edición para limpiar la salida
y guardarla como un script de shell.

## Nelle's Pipeline: Creación de un guión

El supervisor de Nelle insistió en que todos sus análisis debían ser reproducibles. La
forma más fácil de capturar todos los pasos es en un guión.

Primero volvemos al directorio del proyecto de Nelle:

```bash
$ cd ../../north-pacific-gyre/
```

Ella crea un archivo usando `nano` ...

```bash
$ nano do-stats.sh
```

...que contiene lo siguiente:

```bash
# Calculate stats for data files.
for datafile in "$@"
do
    echo $datafile
    bash goostats.sh $datafile stats-$datafile
done
```

Lo guarda en un archivo llamado `do-stats.sh` para poder volver a realizar la primera
fase de su análisis escribiendo:

```bash
$ bash do-stats.sh NENE*A.txt NENE*B.txt
```

También puede hacerlo:

```bash
$ bash do-stats.sh NENE*A.txt NENE*B.txt | wc -l
```

para que la salida sea sólo el número de ficheros procesados en lugar de los nombres de
los ficheros procesados.

Cabe destacar que el script de Nelle permite a la persona que lo ejecuta decidir qué
archivos procesar. Podría haberlo escrito como

```bash
# Calculate stats for Site A and Site B data files.
for datafile in NENE*A.txt NENE*B.txt
do
    echo $datafile
    bash goostats.sh $datafile stats-$datafile
done
```

La ventaja es que siempre selecciona los archivos correctos: no tiene que acordarse de
excluir los archivos "Z". La desventaja es que *siempre* selecciona sólo esos archivos:
no puede ejecutarlo en todos los archivos (incluidos los archivos "Z"), o en los
archivos "G" o "H" que producen sus colegas en la Antártida, sin editar el guión. Si
quisiera ser más atrevida, podría modificar el script para comprobar si hay argumentos
en la línea de comandos y utilizar `NENE*A.txt NENE*B.txt` si no se proporciona ninguno.
Por supuesto, esto introduce otro compromiso entre flexibilidad y complejidad.

::::::::::::::::::::::::::::::::::::::: challenge

## Variables en Shell Scripts

En el directorio `alkanes`, imagine que tiene un script de shell llamado `script.sh` que
contiene los siguientes comandos:

```bash
head -n $2 $1
tail -n $3 $1
```

Mientras se encuentra en el directorio `alkanes`, escriba el siguiente comando:

```bash
$ bash script.sh '*.pdb' 1 1
```

¿Cuál de las siguientes salidas esperaría ver?

1. Todas las líneas entre la primera y la última línea de cada fichero que termina en
   `.pdb` en el directorio `alkanes`
2. La primera y la última línea de cada fichero que termine en `.pdb` en el directorio
   `alkanes`
3. La primera y la última línea de cada fichero del directorio `alkanes`
4. Error debido a las comillas alrededor de `*.pdb`

::::::::::::::: solution

## Solución

La respuesta correcta es 2.

Las variables especiales `$1`, `$2` y `$3` representan los argumentos de línea de
comandos dados al script, de tal forma que los comandos ejecutados son:

```bash
$ head -n 1 cubane.pdb ethane.pdb octane.pdb pentane.pdb propane.pdb
$ tail -n 1 cubane.pdb ethane.pdb octane.pdb pentane.pdb propane.pdb
```

El shell no expande `'*.pdb'` porque está entre comillas. Como tal, el primer argumento
del script es `'*.pdb'` que se expande dentro del script por `head` y `tail`.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Buscar el archivo más largo con una extensión dada

Escribe un script de shell llamado `longest.sh` que tome como argumentos el nombre de un
directorio y una extensión de fichero, e imprima el nombre del fichero con más líneas en
ese directorio con esa extensión. Por ejemplo

```bash
$ bash longest.sh shell-lesson-data/exercise-data/alkanes pdb
```

imprimiría el nombre del fichero `.pdb` en `shell-lesson-data/exercise-data/alkanes` que
tiene más líneas.

No dude en probar su script en otro directorio, por ejemplo

```bash
$ bash longest.sh shell-lesson-data/exercise-data/writing txt
```

::::::::::::::: solution

## Solución

```bash
# Shell script which takes two arguments:
#    1. a directory name
#    2. a file extension
# and prints the name of the file in that directory
# with the most lines which matches the file extension.

wc -l $1/*.$2 | sort -n | tail -n 2 | head -n 1
```

La primera parte del proceso, `wc -l $1/*.$2 | sort -n`, cuenta las líneas de cada
fichero y las ordena numéricamente (la mayor en último lugar). Cuando hay más de un
fichero, `wc` también genera una línea de resumen final, que da el número total de
líneas de *todos* los ficheros. Usamos `tail -n 2 | head -n 1` para desechar esta última
línea.

Con `wc -l $1/*.$2 | sort -n | tail -n 1` veremos la línea de resumen final: podemos
construir nuestro pipeline por partes para estar seguros de que entendemos la salida.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Comprensión de lectura de guiones

Para esta pregunta, considere de nuevo el directorio
`shell-lesson-data/exercise-data/alkanes`. Este contiene un número de archivos `.pdb`
además de cualquier otro archivo que pueda haber creado. Explique qué haría cada uno de
los tres scripts siguientes al ejecutarse como `bash script1.sh *.pdb`, `bash script2.sh
*.pdb` y `bash script3.sh *.pdb` respectivamente.

```bash
# Script 1
echo *.*
```

```bash
# Script 2
for filename in $1 $2 $3
do
    cat $filename
done
```

```bash
# Script 3
echo $@.pdb
```

::::::::::::::: solution

## Soluciones

En cada caso, el shell expande el comodín en `*.pdb` antes de pasar la lista resultante
de nombres de archivo como argumentos al script.

El script 1 imprimiría una lista de todos los archivos que contienen un punto en su
nombre. Los argumentos pasados al script no se utilizan realmente en ninguna parte del
script.

Script 2 imprimiría el contenido de los 3 primeros ficheros con extensión `.pdb`. `$1`,
`$2`, y `$3` se refieren al primer, segundo y tercer argumento respectivamente.

Script 3 imprimiría todos los argumentos del script (es decir, todos los archivos
`.pdb`), seguidos de `.pdb`.`$@` se refiere a *todos* los argumentos dados a un script
de shell.

```output
cubane.pdb ethane.pdb methane.pdb octane.pdb pentane.pdb propane.pdb.pdb
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Scripts de depuración

Suponga que ha guardado el siguiente script en un archivo llamado `do-errors.sh` en el
directorio `north-pacific-gyre` de Nelle:

```bash
# Calculate stats for data files.
for datafile in "$@"
do
    echo $datfile
    bash goostats.sh $datafile stats-$datafile
done
```

Cuando se ejecuta desde el directorio `north-pacific-gyre`:

```bash
$ bash do-errors.sh NENE*A.txt NENE*B.txt
```

la salida está en blanco. Para averiguar por qué, vuelva a ejecutar el script utilizando
la opción `-x`:

```bash
$ bash -x do-errors.sh NENE*A.txt NENE*B.txt
```

¿Qué muestra la salida? ¿Qué línea es la responsable del error?

::::::::::::::: solution

## Solución

La opción `-x` hace que `bash` se ejecute en modo depuración. Esto imprime cada comando
a medida que se ejecuta, lo que le ayudará a localizar errores. En este ejemplo, podemos
ver que `echo` no imprime nada. Hemos cometido un error tipográfico en el nombre de la
variable de bucle, y la variable `datfile` no existe, por lo que devuelve una cadena
vacía.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::



:::::::::::::::::::::::::::::::::::::::: keypoints

- Guarda comandos en archivos (normalmente llamados shell scripts) para reutilizarlos.
- `bash [filename]` ejecuta los comandos guardados en un archivo.
- `$@` se refiere a todos los argumentos de línea de comandos de un script de shell.
- `$1`, `$2`, etc., se refieren al primer argumento de la línea de comandos, al segundo
  argumento de la línea de comandos, etc.
- Coloque las variables entre comillas si los valores pueden contener espacios.
- Dejar que los usuarios decidan qué archivos procesar es más flexible y coherente con
  los comandos integrados de Unix.

::::::::::::::::::::::::::::::::::::::::::::::::::



