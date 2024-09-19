---
title: Finding Things
teaching: 25
exercises: 20
---


::::::::::::::::::::::::::::::::::::::: objectives

- Utilice `grep` para seleccionar líneas de archivos de texto que coincidan con patrones
  simples.
- Utilice `find` para buscar archivos y directorios cuyos nombres coincidan con patrones
  simples.
- Utilizar la salida de un comando como argumento(s) de otro comando.
- Explique qué se entiende por archivos "de texto" y "binarios", y por qué muchas
  herramientas habituales no gestionan bien estos últimos.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- ¿Cómo puedo encontrar archivos?
- ¿Cómo puedo encontrar cosas en los archivos?

::::::::::::::::::::::::::::::::::::::::::::::::::

Del mismo modo que muchos de nosotros utilizamos ahora "Google" como verbo que significa
"encontrar", los programadores de Unix utilizan a menudo la palabra "grep". grep" es una
contracción de "global/regular expression/print", una secuencia de operaciones habitual
en los primeros editores de texto de Unix. También es el nombre de un programa de línea
de comandos muy útil.

`grep` encuentra e imprime líneas en archivos que coinciden con un patrón. Para nuestros
ejemplos, utilizaremos un archivo que contiene tres haiku tomados de un [concurso de
1998](https://web.archive.org/web/19991201042211/http://salon.com/21st/chal/1998/01/26chal.html)
en la revista *Salon* (Créditos a los autores Bill Torcaso, Howard Korder y Margaret
Segall, respectivamente. Ver Haiku Error Messsages archivados [Página
1](https://web.archive.org/web/20000310061355/http://www.salon.com/21st/chal/1998/02/10chal2.html)
y [Página
2](https://web.archive.org/web/20000229135138/http://www.salon.com/21st/chal/1998/02/10chal3.html)
.). Para este conjunto de ejemplos, vamos a trabajar en el subdirectorio de escritura:

```bash
$ cd
$ cd Desktop/shell-lesson-data/exercise-data/writing
$ cat haiku.txt
```

```output
The Tao that is seen
Is not the true Tao, until
You bring fresh toner.

With searching comes loss
and the presence of absence:
"My Thesis" not found.

Yesterday it worked
Today it is not working
Software is like that.
```

Busquemos líneas que contengan la palabra "no":

```bash
$ grep not haiku.txt
```

```output
Is not the true Tao, until
"My Thesis" not found
Today it is not working
```

Aquí, `not` es el patrón que estamos buscando. El comando grep busca en el fichero las
coincidencias con el patrón especificado. Para utilizarlo, escriba `grep`, a
continuación el patrón que estamos buscando y, por último, el nombre del archivo (o
archivos) en el que estamos buscando.

La salida son las tres líneas del fichero que contienen las letras "no".

Por defecto, grep busca un patrón distinguiendo entre mayúsculas y minúsculas. Además,
el patrón de búsqueda que hemos seleccionado no tiene por qué formar una palabra
completa, como veremos en el siguiente ejemplo.

Busquemos el patrón: 'El'.

```bash
$ grep The haiku.txt
```

```output
The Tao that is seen
"My Thesis" not found.
```

Esta vez se muestran dos líneas que incluyen las letras "The", una de las cuales
contiene nuestro patrón de búsqueda dentro de una palabra más grande, "Thesis".

Para restringir las coincidencias a las líneas que contengan la palabra "La" sola,
podemos dar a `grep` la opción `-w`. Esto limitará las coincidencias a los límites de
las palabras.

Más adelante en esta lección, también veremos cómo podemos cambiar el comportamiento de
búsqueda de grep con respecto a su sensibilidad a mayúsculas y minúsculas.

```bash
$ grep -w The haiku.txt
```

```output
The Tao that is seen
```

Tenga en cuenta que un "límite de palabra" incluye el principio y el final de una línea,
es decir, no sólo las letras rodeadas de espacios. A veces no queremos buscar una sola
palabra, sino una frase. También podemos hacerlo con `grep` poniendo la frase entre
comillas.

```bash
$ grep -w "is not" haiku.txt
```

```output
Today it is not working
```

Ya hemos visto que no es necesario entrecomillar las palabras sueltas, pero es útil
hacerlo cuando se buscan varias palabras. También ayuda a distinguir más fácilmente
entre el término o frase de búsqueda y el fichero buscado. En el resto de los ejemplos
utilizaremos comillas.

Otra opción útil es `-n`, que numera las líneas que coinciden:

```bash
$ grep -n "it" haiku.txt
```

```output
5:With searching comes loss
9:Yesterday it worked
10:Today it is not working
```

Aquí, podemos ver que las líneas 5, 9 y 10 contienen las letras "it".

Podemos combinar opciones (es decir, banderas) como hacemos con otros comandos Unix. Por
ejemplo, busquemos las líneas que contienen la palabra 'the'. Podemos combinar la opción
`-w` para encontrar las líneas que contienen la palabra 'the' y `-n` para numerar las
líneas que coinciden:

```bash
$ grep -n -w "the" haiku.txt
```

```output
2:Is not the true Tao, until
6:and the presence of absence:
```

Ahora queremos utilizar la opción `-i` para que nuestra búsqueda no distinga entre
mayúsculas y minúsculas:

```bash
$ grep -n -w -i "the" haiku.txt
```

```output
1:The Tao that is seen
2:Is not the true Tao, until
6:and the presence of absence:
```

Ahora, queremos utilizar la opción `-v` para invertir nuestra búsqueda, es decir,
queremos que aparezcan las líneas que no contienen la palabra 'the'.

```bash
$ grep -n -w -v "the" haiku.txt
```

```output
1:The Tao that is seen
3:You bring fresh toner.
4:
5:With searching comes loss
7:"My Thesis" not found.
8:
9:Yesterday it worked
10:Today it is not working
11:Software is like that.
```

Si utilizamos la opción `-r` (recursivo), `grep` puede buscar un patrón recursivamente a
través de un conjunto de ficheros en subdirectorios.

Busquemos recursivamente `Yesterday` en el directorio
`shell-lesson-data/exercise-data/writing`:

```bash
$ grep -r Yesterday .
```

```output
./LittleWomen.txt:"Yesterday, when Aunt was asleep and I was trying to be as still as a
./LittleWomen.txt:Yesterday at dinner, when an Austrian officer stared at us and then
./LittleWomen.txt:Yesterday was a quiet day spent in teaching, sewing, and writing in my
./haiku.txt:Yesterday it worked
```

`grep` tiene muchas otras opciones. Para saber cuáles son, podemos escribir

```bash
$ grep --help
```

```output
Usage: grep [OPTION]... PATTERN [FILE]...
Search for PATTERN in each FILE or standard input.
PATTERN is, by default, a basic regular expression (BRE).
Example: grep -i 'hello world' menu.h main.c

Regexp selection and interpretation:
  -E, --extended-regexp     PATTERN is an extended regular expression (ERE)
  -F, --fixed-strings       PATTERN is a set of newline-separated fixed strings
  -G, --basic-regexp        PATTERN is a basic regular expression (BRE)
  -P, --perl-regexp         PATTERN is a Perl regular expression
  -e, --regexp=PATTERN      use PATTERN for matching
  -f, --file=FILE           obtain PATTERN from FILE
  -i, --ignore-case         ignore case distinctions
  -w, --word-regexp         force PATTERN to match only whole words
  -x, --line-regexp         force PATTERN to match only whole lines
  -z, --null-data           a data line ends in 0 byte, not newline

Miscellaneous:
...        ...        ...
```

::::::::::::::::::::::::::::::::::::::: challenge

## Usando `grep`

¿Qué comando daría como resultado la siguiente salida?

```output
and the presence of absence:
```

1. `grep "of" haiku.txt`
2. `grep -E "of" haiku.txt`
3. `grep -w "of" haiku.txt`
4. `grep -i "of" haiku.txt`

::::::::::::::: solution

## Solución

La respuesta correcta es 3, porque la opción `-w` sólo busca coincidencias con palabras
completas. Las otras opciones también coinciden con "de" cuando forma parte de otra
palabra.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::: callout

## Comodines

El verdadero poder de `grep` no proviene de sus opciones, sino del hecho de que los
patrones pueden incluir comodines. (El nombre técnico para éstos es **expresiones
regulares**, que es lo que significa la "re" en "grep") Las expresiones regulares son
complejas y potentes; si quieres hacer búsquedas complejas, consulta la lección en
[nuestro sitio
web](https://librarycarpentry.org/lc-data-intro/01-regular-expressions.html). Como
muestra, podemos encontrar líneas que tengan una 'o' en la segunda posición de la
siguiente manera:

```bash
$ grep -E "^.o" haiku.txt
```

```output
You bring fresh toner.
Today it is not working
Software is like that.
```

Utilizamos la opción `-E` y ponemos el patrón entre comillas para evitar que el shell
intente interpretarlo. (Si el patrón contuviera un `*`, por ejemplo, el shell intentaría
expandirlo antes de ejecutar `grep`) La `^` en el patrón ancla la coincidencia al inicio
de la línea. `.` coincide con un único carácter (igual que `?` en el intérprete de
comandos), mientras que `o` coincide con una 'o' real.


::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Seguimiento de una especie

Leah tiene varios cientos de archivos de datos guardados en un directorio, cada uno de
ellos con el siguiente formato:

```source
2012-11-05,deer,5
2012-11-05,rabbit,22
2012-11-05,raccoon,7
2012-11-06,rabbit,19
2012-11-06,deer,2
2012-11-06,fox,4
2012-11-07,rabbit,16
2012-11-07,bear,1
```

Quiere escribir una secuencia de comandos que tome una especie como primer argumento de
la línea de comandos y un directorio como segundo argumento. El script debe devolver un
fichero llamado `<species>.txt` que contenga una lista de fechas y el número de esa
especie observada en cada fecha. Por ejemplo, usando los datos mostrados arriba,
`rabbit.txt` contendría:

```source
2012-11-05,22
2012-11-06,19
2012-11-07,16
```

A continuación, cada línea contiene un comando individual, o tubería. Ordena su
secuencia en un comando para lograr el objetivo de Leah:

```bash
cut -d : -f 2
>
|
grep -w $1 -r $2
|
$1.txt
cut -d , -f 1,3
```

Sugerencia: utilice `man grep` para buscar cómo grep texto recursivamente en un
directorio y `man cut` para seleccionar más de un campo en una línea.

Un ejemplo de este tipo de archivo se proporciona en
`shell-lesson-data/exercise-data/animal-counts/animals.csv`

::::::::::::::: solution

## Solución

```source
grep -w $1 -r $2 | cut -d : -f 2 | cut -d , -f 1,3 > $1.txt
```

En realidad, puedes intercambiar el orden de los dos comandos de corte y seguirá
funcionando. En la línea de comandos, prueba a cambiar el orden de los comandos de
corte, y echa un vistazo a la salida de cada paso para ver por qué es así.

Llamarías al script anterior de la siguiente manera:

```bash
$ bash count-species.sh bear .
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Mujercitas

Tú y tu amiga, que acabáis de terminar de leer *Mujercitas* de Louisa May Alcott, estáis
discutiendo. De las cuatro hermanas del libro, Jo, Meg, Beth y Amy, tu amiga cree que Jo
es la más mencionada. Tú, sin embargo, estás segura de que fue Amy. Por suerte, tienes
un fichero `LittleWomen.txt` que contiene el texto completo de la novela
(`shell-lesson-data/exercise-data/writing/LittleWomen.txt`). Utilizando un bucle `for`,
¿cómo tabularías el número de veces que se menciona a cada una de las cuatro hermanas?

Sugerencia: una solución puede emplear los comandos `grep` y `wc` y un `|`, mientras que
otra puede utilizar las opciones `grep`. A menudo hay más de una forma de resolver una
tarea de programación, por lo que se suele elegir una solución concreta basándose en una
combinación de rendimiento del resultado correcto, elegancia, legibilidad y velocidad.

::::::::::::::: solution

## Soluciones

```source
for sis in Jo Meg Beth Amy
do
    echo $sis:
    grep -ow $sis LittleWomen.txt | wc -l
done
```

Solución alternativa ligeramente inferior:

```source
for sis in Jo Meg Beth Amy
do
    echo $sis:
    grep -ocw $sis LittleWomen.txt
done
```

Esta solución es inferior porque `grep -c` sólo informa del número de líneas
coincidentes. El número total de coincidencias informado por este método será menor si
hay más de una coincidencia por línea.

Los observadores más perspicaces se habrán dado cuenta de que los nombres de los
personajes a veces aparecen en mayúsculas en los títulos de los capítulos (por ejemplo,
"MEG GOES TO VANITY FAIR"). Si quiere contarlos también, puede añadir la opción `-i`
para no distinguir entre mayúsculas y minúsculas (aunque en este caso no afecta a la
respuesta sobre qué hermana se menciona con más frecuencia).



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

Mientras que `grep` busca líneas en ficheros, el comando `find` busca ficheros en sí. De
nuevo, tiene muchas opciones; para mostrar cómo funcionan las más simples, usaremos el
árbol de directorios `shell-lesson-data/exercise-data` que se muestra a continuación.

```output
.
├── animal-counts/
│   └── animals.csv
├── creatures/
│   ├── basilisk.dat
│   ├── minotaur.dat
│   └── unicorn.dat
├── numbers.txt
├── alkanes/
│   ├── cubane.pdb
│   ├── ethane.pdb
│   ├── methane.pdb
│   ├── octane.pdb
│   ├── pentane.pdb
│   └── propane.pdb
└── writing/
    ├── haiku.txt
    └── LittleWomen.txt
```

El directorio `exercise-data` contiene un fichero, `numbers.txt` y cuatro directorios:
`animal-counts`, `creatures`, `alkanes` y `writing` que contienen varios ficheros.

Para nuestro primer comando, ejecutemos `find .` (recuerde ejecutar este comando desde
la carpeta `shell-lesson-data/exercise-data`).

```bash
$ find .
```

```output
.
./writing
./writing/LittleWomen.txt
./writing/haiku.txt
./creatures
./creatures/basilisk.dat
./creatures/unicorn.dat
./creatures/minotaur.dat
./animal-counts
./animal-counts/animals.csv
./numbers.txt
./alkanes
./alkanes/ethane.pdb
./alkanes/propane.pdb
./alkanes/octane.pdb
./alkanes/pentane.pdb
./alkanes/methane.pdb
./alkanes/cubane.pdb
```

Como siempre, `.` por sí solo significa el directorio de trabajo actual, que es donde
queremos que empiece nuestra búsqueda. la salida de `find` son los nombres de cada
archivo **y** directorio bajo el directorio de trabajo actual. Esto puede parecer inútil
al principio, pero `find` tiene muchas opciones para filtrar la salida y en esta lección
descubriremos algunas de ellas.

La primera opción de nuestra lista es `-type d` que significa 'cosas que son
directorios'. Efectivamente, la salida de `find` son los nombres de los cinco
directorios (incluido `.`):

```bash
$ find . -type d
```

```output
.
./writing
./creatures
./animal-counts
./alkanes
```

Observa que los objetos encontrados por `find` no aparecen en ningún orden en
particular. Si cambiamos `-type d` por `-type f`, obtendremos un listado de todos los
ficheros:

```bash
$ find . -type f
```

```output
./writing/LittleWomen.txt
./writing/haiku.txt
./creatures/basilisk.dat
./creatures/unicorn.dat
./creatures/minotaur.dat
./animal-counts/animals.csv
./numbers.txt
./alkanes/ethane.pdb
./alkanes/propane.pdb
./alkanes/octane.pdb
./alkanes/pentane.pdb
./alkanes/methane.pdb
./alkanes/cubane.pdb
```

Ahora probemos a buscar por nombre:

```bash
$ find . -name *.txt
```

```output
./numbers.txt
```

Esperábamos que encontrara todos los archivos de texto, pero sólo imprime
`./numbers.txt`. El problema es que el shell expande caracteres comodín como `*` *antes*
de que se ejecuten los comandos. Dado que `*.txt` en el directorio actual se expande a
`./numbers.txt`, el comando que realmente ejecutamos fue:

```bash
$ find . -name numbers.txt
```

`find` hizo lo que le pedimos; sólo que le pedimos lo equivocado.

Para conseguir lo que queremos, hagamos lo que hicimos con `grep`: poner `*.txt` entre
comillas para evitar que el shell expanda el comodín `*`. De esta forma, `find` obtiene
realmente el patrón `*.txt`, no el nombre de fichero expandido `numbers.txt`:

```bash
$ find . -name "*.txt"
```

```output
./writing/LittleWomen.txt
./writing/haiku.txt
./numbers.txt
```

::::::::::::::::::::::::::::::::::::::::: callout

## Listado vs. Búsqueda

Se puede hacer que `ls` y `find` hagan cosas similares dadas las opciones adecuadas,
pero en circunstancias normales, `ls` lista todo lo que puede, mientras que `find` busca
cosas con ciertas propiedades y las muestra.


::::::::::::::::::::::::::::::::::::::::::::::::::

Como hemos dicho antes, el poder de la línea de comandos reside en la combinación de
herramientas. Ya hemos visto cómo hacerlo con las tuberías; veamos otra técnica. Como
acabamos de ver, `find . -name "*.txt"` nos da una lista de todos los archivos de texto
dentro o debajo del directorio actual. ¿Cómo podemos combinarlo con `wc -l` para contar
las líneas de todos esos ficheros?

La forma más sencilla es poner el comando `find` dentro de `$()`:

```bash
$ wc -l $(find . -name "*.txt")
```

```output
  21022 ./writing/LittleWomen.txt
     11 ./writing/haiku.txt
      5 ./numbers.txt
  21038 total
```

Cuando el shell ejecuta este comando, lo primero que hace es ejecutar lo que hay dentro
de `$()`. Luego reemplaza la expresión `$()` con la salida de ese comando. Como la
salida de `find` son los tres nombres de fichero `./writing/LittleWomen.txt`,
`./writing/haiku.txt`, y `./numbers.txt`, el shell construye el comando:

```bash
$ wc -l ./writing/LittleWomen.txt ./writing/haiku.txt ./numbers.txt
```

que es lo que queríamos. Esta expansión es exactamente lo que hace el shell cuando
expande comodines como `*` y `?`, pero nos permite usar cualquier comando que queramos
como nuestro propio 'comodín'.

Es muy común utilizar `find` y `grep` a la vez. El primero busca archivos que coincidan
con un patrón; el segundo busca líneas dentro de esos archivos que coincidan con otro
patrón. Aquí, por ejemplo, podemos encontrar archivos txt que contengan la palabra
"searching" buscando la cadena 'searching' en todos los archivos `.txt` del directorio
actual:

```bash
$ grep "searching" $(find . -name "*.txt")
```

```output
./writing/LittleWomen.txt:sitting on the top step, affected to be searching for her book, but was
./writing/haiku.txt:With searching comes loss
```

::::::::::::::::::::::::::::::::::::::: challenge

## Emparejar y restar

La opción `-v` de `grep` invierte la coincidencia de patrones, de modo que sólo se
imprimen las líneas que *no* coinciden con el patrón. Teniendo esto en cuenta, ¿cuál de
los siguientes comandos encontrará todos los ficheros .dat en `creatures` excepto
`unicorn.dat`? Una vez hayas pensado tu respuesta, puedes probar los comandos en el
directorio `shell-lesson-data/exercise-data`.

1. `find creatures -name "*.dat" | grep -v unicorn`
2. `find creatures -name *.dat | grep -v unicorn`
3. `grep -v "unicorn" $(find creatures -name "*.dat")`
4. Ninguna de las anteriores.

::::::::::::::: solution

## Solución

La opción 1 es correcta. Poner la expresión de coincidencia entre comillas evita que el
shell la expanda, por lo que se pasa al comando `find`.

La opción 2 también funciona en este caso porque el shell intenta expandir `*.dat` pero
no hay ficheros `*.dat` en el directorio actual, así que la expresión comodín se pasa a
`find`. Encontramos esto por primera vez en el [episodio 3](03-create.md).

La opción 3 es incorrecta porque busca en el contenido de los ficheros las líneas que no
coinciden con "unicornio", en lugar de buscar en los nombres de los ficheros.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::: callout

## Archivos binarios

Nos hemos centrado exclusivamente en la búsqueda de patrones en archivos de texto. ¿Y si
sus datos están almacenados en imágenes, bases de datos o cualquier otro formato?

Un puñado de herramientas amplían `grep` para manejar algunos formatos que no son de
texto. Pero un enfoque más generalizable es convertir los datos en texto, o extraer los
elementos similares al texto de los datos. Por un lado, facilita las cosas sencillas.
Por otro, las cosas complejas suelen ser imposibles. Por ejemplo, es bastante fácil
escribir un programa que extraiga las dimensiones X e Y de archivos de imagen para que
`grep` juegue con ellas, pero ¿cómo escribirías algo para encontrar valores en una hoja
de cálculo cuyas celdas contuvieran fórmulas?

Una última opción es reconocer que el shell y el procesamiento de texto tienen sus
límites, y utilizar otro lenguaje de programación. Cuando llegue el momento de hacerlo,
no seas demasiado duro con el shell. Muchos lenguajes de programación modernos han
tomado prestadas muchas ideas de él, y la imitación es también la forma más sincera de
elogio.


::::::::::::::::::::::::::::::::::::::::::::::::::

El shell de Unix es más antiguo que la mayoría de las personas que lo utilizan. Ha
sobrevivido tanto tiempo porque es uno de los entornos de programación más productivos
jamás creados, quizá incluso *el* más productivo. Su sintaxis puede ser críptica, pero
la gente que la domina puede experimentar con diferentes comandos de forma interactiva,
y luego utilizar lo que han aprendido para automatizar su trabajo. Las interfaces
gráficas de usuario pueden ser más fáciles de usar al principio, pero una vez
aprendidas, la productividad del shell es insuperable. Y como escribió Alfred North
Whitehead en 1911, 'La civilización avanza ampliando el número de operaciones
importantes que podemos realizar sin pensar en ellas'

::::::::::::::::::::::::::::::::::::::: challenge

## `find` Pipeline Comprensión lectora

Escriba un breve comentario explicativo para el siguiente script de shell:

```bash
wc -l $(find . -name "*.dat") | sort -n
```

::::::::::::::: solution

## Solución

1. Buscar todos los archivos con extensión `.dat` recursivamente desde el directorio
   actual
2. Cuente el número de líneas que contiene cada uno de estos ficheros
3. Ordena numéricamente la salida del paso 2

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::



:::::::::::::::::::::::::::::::::::::::: keypoints

- `find` encuentra archivos con propiedades específicas que coinciden con patrones.
- `grep` selecciona líneas en archivos que coinciden con patrones.
- `--help` es una opción soportada por muchos comandos bash, y programas que pueden ser
  ejecutados desde dentro de Bash, para mostrar más información sobre cómo usar estos
  comandos o programas.
- `man [command]` muestra la página del manual de un comando determinado.
- `$([command])` inserta la salida de un comando en su lugar.

::::::::::::::::::::::::::::::::::::::::::::::::::



