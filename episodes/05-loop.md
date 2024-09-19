---
title: Loops
teaching: 40
exercises: 10
---


::::::::::::::::::::::::::::::::::::::: objectives

- Escribe un bucle que aplique uno o más comandos por separado a cada archivo de un
  conjunto de archivos.
- Trazar los valores que toma una variable de bucle durante la ejecución del bucle.
- Explica la diferencia entre el nombre de una variable y su valor.
- Explique por qué no deben utilizarse espacios ni algunos caracteres de puntuación en
  los nombres de archivo.
- Demuestre cómo ver qué comandos se han ejecutado recientemente.
- Reejecuta los comandos ejecutados recientemente sin volver a escribirlos.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- ¿Cómo puedo realizar las mismas acciones en muchos archivos diferentes?

::::::::::::::::::::::::::::::::::::::::::::::::::

**Los bucles** son una construcción de programación que nos permite repetir un comando o
conjunto de comandos para cada elemento de una lista. Como tales, son fundamentales para
mejorar la productividad mediante la automatización. Al igual que los comodines y el
tabulador, los bucles reducen la cantidad de texto que hay que teclear (y, por tanto, el
número de errores).

Supongamos que tenemos varios cientos de archivos de datos del genoma llamados
`basilisk.dat`, `minotaur.dat` y `unicorn.dat`. Para este ejemplo, utilizaremos el
directorio `exercise-data/creatures` que sólo tiene tres archivos de ejemplo, pero los
principios pueden aplicarse a muchos más archivos a la vez.

La estructura de estos ficheros es la misma: el nombre común, la clasificación y la
fecha de actualización se presentan en las tres primeras líneas, con las secuencias de
ADN en las líneas siguientes. Veamos los ficheros

```bash
$ head -n 5 basilisk.dat minotaur.dat unicorn.dat
```

Queremos imprimir la clasificación de cada especie, que figura en la segunda línea de
cada fichero. Para cada fichero, tendríamos que ejecutar el comando `head -n 2` y
enviarlo a `tail -n 1`. Utilizaremos un bucle para resolver este problema, pero primero
veamos la forma general de un bucle, utilizando el pseudocódigo siguiente:

```bash
# The word "for" indicates the start of a "For-loop" command
for thing in list_of_things 
#The word "do" indicates the start of job execution list
do 
    # Indentation within the loop is not required, but aids legibility
    operation_using/command $thing 
# The word "done" indicates the end of a loop
done  
```

y podemos aplicarlo a nuestro ejemplo así:

```bash
$ for filename in basilisk.dat minotaur.dat unicorn.dat
> do
>     echo $filename
>     head -n 2 $filename | tail -n 1
> done
```

```output
basilisk.dat
CLASSIFICATION: basiliscus vulgaris
minotaur.dat
CLASSIFICATION: bos hominus
unicorn.dat
CLASSIFICATION: equus monoceros
```

::::::::::::::::::::::::::::::::::::::::: callout

## Siga las instrucciones

El prompt de la shell cambia de `$` a `>` y viceversa mientras tecleamos en nuestro
bucle. El segundo prompt, `>`, es diferente para recordarnos que aún no hemos terminado
de escribir un comando completo. Se puede usar un punto y coma, `;`, para separar dos
comandos escritos en una sola línea.


::::::::::::::::::::::::::::::::::::::::::::::::::

Cuando el shell ve la palabra clave `for`, sabe que debe repetir un comando (o grupo de
comandos) una vez por cada elemento de una lista. Cada vez que se ejecuta el bucle
(llamado iteración), se asigna un elemento de la lista a la **variable**, y se ejecutan
los comandos dentro del bucle, antes de pasar al siguiente elemento de la lista. Dentro
del bucle, pedimos el valor de la variable poniendo `$` delante de ella. El `$` le dice
al intérprete del shell que trate la variable como un nombre de variable y sustituya su
valor en su lugar, en lugar de tratarla como texto o un comando externo.

En este ejemplo, la lista son tres nombres de fichero: `basilisk.dat`, `minotaur.dat`, y
`unicorn.dat`. Cada vez que el bucle itera, primero usamos `echo` para imprimir el valor
que la variable `$filename` tiene actualmente. Esto no es necesario para el resultado,
pero nos beneficia aquí para seguirlo más fácilmente. A continuación, ejecutaremos el
comando `head` en el fichero referenciado actualmente por `$filename`. La primera vez a
través del bucle, `$filename` es `basilisk.dat`. El intérprete ejecuta el comando `head`
en `basilisk.dat` y envía las dos primeras líneas al comando `tail`, que a su vez
imprime la segunda línea de `basilisk.dat`. Para la segunda iteración, `$filename` se
convierte en `minotaur.dat`. Esta vez, el shell ejecuta `head` en `minotaur.dat` y envía
las dos primeras líneas al comando `tail`, que imprime la segunda línea de
`minotaur.dat`. Para la tercera iteración, `$filename` se convierte en `unicorn.dat`,
así que el shell ejecuta el comando `head` en ese fichero, y `tail` en la salida del
mismo. Como la lista sólo tenía tres elementos, el shell sale del bucle `for`.

::::::::::::::::::::::::::::::::::::::::: callout

## Mismos símbolos, distintos significados

Aquí vemos que `>` se utiliza como prompt del shell, mientras que `>` también se utiliza
para redirigir la salida. De forma similar, `$` se usa como prompt del shell, pero, como
vimos antes, también se usa para pedir al shell que obtenga el valor de una variable.

Si el *shell* imprime `>` o `$` entonces espera que escribas algo, y el símbolo es un
prompt.

Si *usted mismo* teclea `>` o `$`, es una instrucción suya para que el shell redirija la
salida u obtenga el valor de una variable.


::::::::::::::::::::::::::::::::::::::::::::::::::

Cuando se utilizan variables también es posible poner los nombres entre llaves para
delimitar claramente el nombre de la variable: `$filename` es equivalente a
`${filename}`, pero es diferente de `${file}name`. Puede que encuentres esta notación en
los programas de otras personas.

Hemos llamado a la variable de este bucle `filename` para que su propósito sea más claro
para los lectores humanos. Al shell en sí no le importa cómo se llama la variable; si
escribiéramos este bucle como:

```bash
$ for x in basilisk.dat minotaur.dat unicorn.dat
> do
>     head -n 2 $x | tail -n 1
> done
```

o:

```bash
$ for temperature in basilisk.dat minotaur.dat unicorn.dat
> do
>     head -n 2 $temperature | tail -n 1
> done
```

funcionaría exactamente igual. *Los programas sólo son útiles si la gente puede
entenderlos, por lo que los nombres sin sentido (como `x`) o los nombres engañosos (como
`temperature`) aumentan las probabilidades de que el programa no haga lo que sus
lectores piensan que hace.

En los ejemplos anteriores, las variables (`thing`, `filename`, `x` y `temperature`)
podrían haber recibido cualquier otro nombre, siempre que sea significativo tanto para
la persona que escribe el código como para la que lo lee.

Observe también que los bucles pueden utilizarse para otras cosas además de nombres de
archivo, como una lista de números o un subconjunto de datos.

::::::::::::::::::::::::::::::::::::::: challenge

## Escribe tu propio bucle

¿Cómo escribirías un bucle que repita los 10 números del 0 al 9?

::::::::::::::: solution

## Solución

```bash
$ for loop_variable in 0 1 2 3 4 5 6 7 8 9
> do
>     echo $loop_variable
> done
```

```output
0
1
2
3
4
5
6
7
8
9
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Variables en bucles

Este ejercicio se refiere al directorio `shell-lesson-data/exercise-data/alkanes`. `ls
*.pdb` da la siguiente salida:

```output
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
```

¿Cuál es el resultado del siguiente código?

```bash
$ for datafile in *.pdb
> do
>     ls *.pdb
> done
```

Ahora, ¿cuál es la salida del siguiente código?

```bash
$ for datafile in *.pdb
> do
>     ls $datafile
> done
```

¿Por qué estos dos bucles dan resultados diferentes?

::::::::::::::: solution

## Solución

El primer bloque de código da la misma salida en cada iteración del bucle. Bash expande
el comodín `*.pdb` dentro del cuerpo del bucle (así como antes de que el bucle comience)
para que coincida con todos los archivos que terminan en `.pdb` y luego los lista usando
`ls`. El bucle expandido tendría este aspecto

```bash
$ for datafile in cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> do
>     ls cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> done
```

```output
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
```

El segundo bloque de código lista un fichero diferente en cada iteración del bucle. El
valor de la variable `datafile` se evalúa con `$datafile`, y luego se lista con `ls`.

```output
cubane.pdb
ethane.pdb
methane.pdb
octane.pdb
pentane.pdb
propane.pdb
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Limitar conjuntos de archivos

¿Cuál sería el resultado de ejecutar el siguiente bucle en el directorio
`shell-lesson-data/exercise-data/alkanes`?

```bash
$ for filename in c*
> do
>     ls $filename
> done
```

1. No hay archivos en la lista.
2. Aparecen todos los archivos.
3. Sólo aparecen `cubane.pdb`, `octane.pdb` y `pentane.pdb`.
4. Sólo aparece `cubane.pdb`.

::::::::::::::: solution

## Solución

4 es la respuesta correcta.`*` coincide con cero o más caracteres, por lo que cualquier
nombre de archivo que empiece por la letra c, seguida de cero o más caracteres será
coincidente.


:::::::::::::::::::::::::

¿En qué se diferenciaría el resultado de utilizar este comando?

```bash
$ for filename in *c*
> do
>     ls $filename
> done
```

1. Aparecerían los mismos archivos.
2. Esta vez aparecen todos los archivos.
3. Esta vez no hay archivos en la lista.
4. Se listarán los ficheros `cubane.pdb` y `octane.pdb`.
5. Sólo se listará el fichero `octane.pdb`.

::::::::::::::: solution

## Solución

4 es la respuesta correcta.`*` coincide con cero o más caracteres, por lo que un nombre
de archivo con cero o más caracteres antes de la letra c y cero o más caracteres después
de la letra c será coincidente.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Guardar en un archivo en bucle - Primera parte

En el directorio `shell-lesson-data/exercise-data/alkanes`, ¿cuál es el efecto de este
bucle?

```bash
for alkanes in *.pdb
do
    echo $alkanes
    cat $alkanes > alkanes.pdb
done
```

1. Imprime `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, `pentane.pdb` y
   `propane.pdb`, y el texto de `propane.pdb` se guardará en un fichero llamado
   `alkanes.pdb`.
2. Imprime `cubane.pdb`, `ethane.pdb`, y `methane.pdb`, y el texto de los tres ficheros
   se concatena y se guarda en un fichero llamado `alkanes.pdb`.
3. Imprime `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, y `pentane.pdb`, y
   el texto de `propane.pdb` se guardará en un fichero llamado `alkanes.pdb`.
4. Ninguna de las anteriores.

::::::::::::::: solution

## Solución

1. El texto de cada fichero se escribe sucesivamente en el fichero `alkanes.pdb`. Sin
   embargo, el archivo se sobrescribe en cada iteración del bucle, por lo que el
   contenido final de `alkanes.pdb` es el texto del archivo `propane.pdb`.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Guardar en un archivo en bucle - Segunda parte

También en el directorio `shell-lesson-data/exercise-data/alkanes`, ¿cuál sería la
salida del siguiente bucle?

```bash
for datafile in *.pdb
do
    cat $datafile >> all.pdb
done
```

1. Todo el texto de `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb` y
   `pentane.pdb` se concatenaría y se guardaría en un fichero llamado `all.pdb`.
2. El texto de `ethane.pdb` se guardará en un archivo llamado `all.pdb`.
3. Todo el texto de `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`,
   `pentane.pdb` y `propane.pdb` sería concatenado y guardado en un fichero llamado
   `all.pdb`.
4. Todo el texto de `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`,
   `pentane.pdb` y `propane.pdb` se imprimiría en la pantalla y se guardaría en un
   fichero llamado `all.pdb`.

::::::::::::::: solution

## Solución

3 es la respuesta correcta. `>>` añade a un archivo, en lugar de sobrescribirlo con la
salida redirigida de un comando. Dado que la salida del comando `cat` ha sido
redirigida, no se imprime nada en la pantalla.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

Continuemos con nuestro ejemplo en el directorio
`shell-lesson-data/exercise-data/creatures`. Aquí tenemos un bucle un poco más
complicado:

```bash
$ for filename in *.dat
> do
>     echo $filename
>     head -n 100 $filename | tail -n 20
> done
```

El shell comienza expandiendo `*.dat` para crear la lista de ficheros que procesará. A
continuación, el **cuerpo de bucle** ejecuta dos órdenes para cada uno de esos ficheros.
El primer comando, `echo`, imprime sus argumentos de línea de comandos en la salida
estándar. Por ejemplo

```bash
$ echo hello there
```

imprime:

```output
hello there
```

En este caso, dado que el shell expande `$filename` para que sea el nombre de un
fichero, `echo $filename` imprime el nombre del fichero. Observe que no podemos escribir
esto como

```bash
$ for filename in *.dat
> do
>     $filename
>     head -n 100 $filename | tail -n 20
> done
```

porque entonces la primera vez a través del bucle, cuando `$filename` se expandiera a
`basilisk.dat`, el shell intentaría ejecutar `basilisk.dat` como un programa.
Finalmente, la combinación `head` y `tail` selecciona las líneas 81-100 de cualquier
fichero que se esté procesando (asumiendo que el fichero tiene al menos 100 líneas).

::::::::::::::::::::::::::::::::::::::::: callout

## Espacios en los nombres

Los espacios se utilizan para separar los elementos de la lista que vamos a recorrer en
bucle. Si uno de esos elementos contiene un espacio, debemos rodearlo de comillas y
hacer lo mismo con nuestra variable de bucle. Supongamos que nuestros ficheros de datos
se llaman

```source
red dragon.dat
purple unicorn.dat
```

Para realizar un bucle sobre estos archivos, tendríamos que añadir comillas dobles de la
siguiente manera:

```bash
$ for filename in "red dragon.dat" "purple unicorn.dat"
> do
>     head -n 100 "$filename" | tail -n 20
> done
```

Es más sencillo evitar el uso de espacios (u otros caracteres especiales) en los nombres
de archivo.

Los ficheros anteriores no existen, por lo que si ejecutamos el código anterior, el
comando `head` será incapaz de encontrarlos; sin embargo, el mensaje de error devuelto
mostrará el nombre de los ficheros que está esperando:

```error
head: cannot open ‘red dragon.dat' for reading: No such file or directory
head: cannot open ‘purple unicorn.dat' for reading: No such file or directory
```

Intente eliminar las comillas alrededor de `$filename` en el bucle anterior para ver el
efecto de las comillas sobre los espacios. Observe que obtenemos un resultado del
comando de bucle para unicorn.dat cuando ejecutamos este código en el directorio
`creatures`:

```output
head: cannot open ‘red' for reading: No such file or directory
head: cannot open ‘dragon.dat' for reading: No such file or directory
head: cannot open ‘purple' for reading: No such file or directory
CGGTACCGAA
AAGGGTCGCG
CAAGTGTTCC
...
```

::::::::::::::::::::::::::::::::::::::::::::::::::

Queremos modificar cada uno de los ficheros de
`shell-lesson-data/exercise-data/creatures`, pero también guardar una versión de los
ficheros originales. Queremos copiar los ficheros originales en nuevos ficheros llamados
`original-basilisk.dat` y `original-unicorn.dat`, por ejemplo. No podemos utilizar

```bash
$ cp *.dat original-*.dat
```

porque se ampliaría a:

```bash
$ cp basilisk.dat minotaur.dat unicorn.dat original-*.dat
```

Esto no haría una copia de seguridad de nuestros archivos, en su lugar obtenemos un
error:

```error
cp: target `original-*.dat' is not a directory
```

Este problema surge cuando `cp` recibe más de dos entradas. Cuando esto ocurre, espera
que la última entrada sea un directorio en el que pueda copiar todos los ficheros que se
le han pasado. Como no hay ningún directorio llamado `original-*.dat` en el directorio
`creatures`, obtenemos un error.

En su lugar, podemos utilizar un bucle:

```bash
$ for filename in *.dat
> do
>     cp $filename original-$filename
> done
```

Este bucle ejecuta el comando `cp` una vez por cada nombre de fichero. La primera vez,
cuando `$filename` se expande a `basilisk.dat`, el shell ejecuta

```bash
cp basilisk.dat original-basilisk.dat
```

La segunda vez, el comando es:

```bash
cp minotaur.dat original-minotaur.dat
```

La tercera y última vez, el comando es:

```bash
cp unicorn.dat original-unicorn.dat
```

Como el comando `cp` normalmente no produce ninguna salida, es difícil comprobar que el
bucle funciona correctamente. Sin embargo, antes aprendimos a imprimir cadenas
utilizando `echo`, y podemos modificar el bucle para utilizar `echo` para imprimir
nuestros comandos sin ejecutarlos realmente. Así podemos comprobar qué comandos *se
ejecutarían* en el bucle sin modificar.

El siguiente diagrama muestra lo que ocurre cuando se ejecuta el bucle modificado y
demuestra cómo el uso juicioso de `echo` es una buena técnica de depuración.

![](fig/shell_script_for_loop_flow_chart.svg){alt='El bucle for "for filename in .dat;
do echo cp $filename original-$filename;done" asignará sucesivamente los nombres de
todos los archivos ".dat" del directorio actual a la variable "$filename" y luego
ejecutará el comando. Con los archivos "basilisk.dat", "minotaur.dat" y "unicorn.dat" en
el directorio actual, el bucle llamará sucesivamente al comando echo tres veces e
imprimirá tres líneas: "cp basilisco.dat original-basilisco.dat", luego "cp
minotauro.datoriginal-minotauro.dat" y finalmente "cp
unicornio.datoriginal-unicornio.dat"'}

## Nelle's Pipeline: Procesamiento de archivos

Nelle ya está preparada para procesar sus archivos de datos utilizando `goostats.sh`, un
script escrito por su supervisor. Éste calcula algunas estadísticas a partir de un
archivo de muestra de proteínas y toma dos argumentos:

1. un archivo de entrada (que contiene los datos brutos)
2. un archivo de salida (para almacenar las estadísticas calculadas)

Como todavía está aprendiendo a usar el shell, decide crear los comandos necesarios por
etapas. Su primer paso es asegurarse de que puede seleccionar los archivos de entrada
correctos --- recuerda, son aquellos cuyos nombres terminan en 'A' o 'B', en lugar de
'Z'. En el directorio `north-pacific-gyre`, Nelle escribe:

```bash
$ cd
$ cd Desktop/shell-lesson-data/north-pacific-gyre
$ for datafile in NENE*A.txt NENE*B.txt
> do
>     echo $datafile
> done
```

```output
NENE01729A.txt
NENE01729B.txt
NENE01736A.txt
...
NENE02043A.txt
NENE02043B.txt
```

Su siguiente paso es decidir cómo llamar a los archivos que creará el programa de
análisis `goostats.sh`. Prefijar el nombre de cada fichero de entrada con 'stats' parece
sencillo, así que modifica su bucle para hacerlo:

```bash
$ for datafile in NENE*A.txt NENE*B.txt
> do
>     echo $datafile stats-$datafile
> done
```

```output
NENE01729A.txt stats-NENE01729A.txt
NENE01729B.txt stats-NENE01729B.txt
NENE01736A.txt stats-NENE01736A.txt
...
NENE02043A.txt stats-NENE02043A.txt
NENE02043B.txt stats-NENE02043B.txt
```

Aún no ha ejecutado `goostats.sh`, pero ahora está segura de que puede seleccionar los
archivos correctos y generar los nombres de archivo de salida adecuados.

Sin embargo, teclear los comandos una y otra vez se está volviendo tedioso, y a Nelle le
preocupa cometer errores, así que en lugar de volver a entrar en su bucle, pulsa
<kbd>↑</kbd>. En respuesta, el intérprete de comandos vuelve a mostrar todo el bucle en
una sola línea (utilizando punto y coma para separar las piezas):

```bash
$ for datafile in NENE*A.txt NENE*B.txt; do echo $datafile stats-$datafile; done
```

Utilizando el <kbd>←</kbd>, Nelle navega hasta el comando `echo` y lo cambia por `bash
goostats.sh`:

```bash
$ for datafile in NENE*A.txt NENE*B.txt; do bash goostats.sh $datafile stats-$datafile; done
```

Cuando pulsa <kbd>Intro</kbd>, el shell ejecuta el comando modificado. Sin embargo, no
parece ocurrir nada: no hay salida. Después de un momento, Nelle se da cuenta de que,
como su script ya no imprime nada en la pantalla, no tiene ni idea de si se está
ejecutando, y mucho menos de a qué velocidad. Ella mata el comando en ejecución
escribiendo <kbd>Ctrl</kbd>\+<kbd>C</kbd>, utiliza <kbd>↑</kbd> para repetir el comando,
y lo edita para que se lea:

```bash
$ for datafile in NENE*A.txt NENE*B.txt; do echo $datafile;
bash goostats.sh $datafile stats-$datafile; done
```

::::::::::::::::::::::::::::::::::::::::: callout

## Principio y fin

Podemos movernos al principio de una línea en el shell tecleando
<kbd>Ctrl</kbd>\+<kbd>A</kbd> y al final utilizando <kbd>Ctrl</kbd>\+<kbd>E</kbd>.


::::::::::::::::::::::::::::::::::::::::::::::::::

Cuando ejecuta su programa ahora, produce una línea de salida cada cinco segundos
aproximadamente:

```output
NENE01729A.txt
NENE01729B.txt
NENE01736A.txt
...
```

1518 veces 5 segundos, dividido por 60, le indica que su script tardará unas dos horas
en ejecutarse. Como última comprobación, abre otra ventana de terminal, entra en
`north-pacific-gyre` y utiliza `cat stats-NENE01729B.txt` para examinar uno de los
archivos de salida. Tiene buena pinta, así que decide tomarse un café y ponerse al día
con la lectura.

::::::::::::::::::::::::::::::::::::::::: callout

## Los que conocen la Historia pueden elegir repetirla

Otra forma de repetir el trabajo anterior es utilizar el comando `history` para obtener
una lista de los últimos cientos de comandos que se han ejecutado y, a continuación,
utilizar `!123` (donde `123` se sustituye por el número de comando) para repetir uno de
esos comandos. Por ejemplo, si Nelle escribe esto

```bash
$ history | tail -n 5
```

```output
456  for datafile in NENE*A.txt NENE*B.txt; do   echo $datafile stats-$datafile; done
457  for datafile in NENE*A.txt NENE*B.txt; do echo $datafile stats-$datafile; done
458  for datafile in NENE*A.txt NENE*B.txt; do bash goostats.sh $datafile stats-$datafile; done
459  for datafile in NENE*A.txt NENE*B.txt; do echo $datafile; bash goostats.sh $datafile
stats-$datafile; done
460  history | tail -n 5
```

entonces puede volver a ejecutar `goostats.sh` en los archivos simplemente escribiendo
`!459`.


::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::: callout

## Otros Comandos de Historia

Existen otros comandos abreviados para acceder al historial.

- <kbd>Ctrl</kbd>\+<kbd>R</kbd> entra en un modo de búsqueda en el historial <i-búsqueda
  inversa> y encuentra el comando más reciente de tu historial que coincida con el texto
  que introduzcas a continuación. Pulsa <kbd>Ctrl</kbd>\+<kbd>R</kbd> una o varias veces
  más para buscar coincidencias anteriores. A continuación, puedes utilizar las teclas
  de flecha izquierda y derecha para elegir esa línea y editarla, y luego pulsar
  <kbd>Return</kbd> para ejecutar el comando.
- `!!` recupera el comando inmediatamente anterior (puede que esto le resulte más cómodo
  o no que utilizar <kbd>↑</kbd>)
- `!$` recupera la última palabra del último comando. Esto es útil más a menudo de lo
  que esperas: después de `bash goostats.sh NENE01729B.txt stats-NENE01729B.txt`, puedes
  teclear `less !$` para mirar el fichero `stats-NENE01729B.txt`, lo que es más rápido
  que hacer <kbd>↑</kbd> y editar la línea de comandos.

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Hacer un simulacro

Un bucle es una forma de hacer muchas cosas a la vez --- o de cometer muchos errores a
la vez si hace lo incorrecto. Una forma de comprobar lo que un bucle *haría* es `echo`
los comandos que ejecutaría en lugar de ejecutarlos realmente.

Supongamos que queremos previsualizar los comandos que ejecutará el siguiente bucle sin
llegar a ejecutar dichos comandos:

```bash
$ for datafile in *.pdb
> do
>     cat $datafile >> all.pdb
> done
```

¿Cuál es la diferencia entre los dos bucles que aparecen a continuación y cuál de ellos
queremos ejecutar?

```bash
# Version 1
$ for datafile in *.pdb
> do
>     echo cat $datafile >> all.pdb
> done
```

```bash
# Version 2
$ for datafile in *.pdb
> do
>     echo "cat $datafile >> all.pdb"
> done
```

::::::::::::::: solution

## Solución

La segunda versión es la que queremos ejecutar. Esta imprime en pantalla todo lo que
está entre comillas, expandiendo el nombre de la variable de bucle porque le hemos
puesto el prefijo del signo dólar. Tampoco *modifica* ni crea el fichero `all.pdb`, ya
que el `>>` se trata literalmente como parte de una cadena y no como una instrucción de
redirección.

La primera versión añade la salida del comando `echo cat $datafile` al fichero,
`all.pdb`. Este fichero sólo contendrá la lista: `cat cubane.pdb`, `cat ethane.pdb`,
`cat methane.pdb` etc.

Pruebe ambas versiones para ver el resultado Asegúrese de abrir el archivo `all.pdb`
para ver su contenido.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Bucles anidados

Supongamos que queremos establecer una estructura de directorios para organizar algunos
experimentos que miden las constantes de velocidad de reacción con diferentes compuestos
*y* diferentes temperaturas. ¿Cuál sería el resultado del siguiente código:

```bash
$ for species in cubane ethane methane
> do
>     for temperature in 25 30 37 40
>     do
>         mkdir $species-$temperature
>     done
> done
```

::::::::::::::: solution

## Solución

Tenemos un bucle anidado, es decir, contenido dentro de otro bucle, de modo que para
cada especie en el bucle exterior, el bucle interior (el bucle anidado) itera sobre la
lista de temperaturas, y crea un nuevo directorio para cada combinación.

¡Intente ejecutar el código usted mismo para ver qué directorios se crean!



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::



:::::::::::::::::::::::::::::::::::::::: keypoints

- Un bucle `for` repite comandos una vez para cada cosa de una lista.
- Cada bucle `for` necesita una variable para referirse a lo que está operando en ese
  momento.
- Utilice `$name` para expandir una variable (es decir, obtener su valor). también se
  puede utilizar `${name}`.
- No utilice espacios, comillas ni caracteres comodín como '\*' o '?' en los nombres de
  archivo, ya que complica la expansión de las variables.
- Asigne a los archivos nombres coherentes que sean fáciles de emparejar con patrones
  comodín para facilitar su selección para el bucle.
- Utilice la tecla de flecha arriba para desplazarse por los comandos anteriores y
  editarlos y repetirlos.
- Utilice <kbd>Ctrl</kbd>\+<kbd>R</kbd> para buscar entre los comandos introducidos
  anteriormente.
- Utilice `history` para mostrar los comandos recientes, y `![number]` para repetir un
  comando por número.

::::::::::::::::::::::::::::::::::::::::::::::::::



