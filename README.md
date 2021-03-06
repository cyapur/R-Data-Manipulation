# R - Guía Básica

## Objetivos y requisitos

1) Proveer de una guía práctica de R en español para aprender manipulación de datos.
2) Se requiere instalar la última versión de R y de Rstudio.

## Índice
1. Conceptos básicos
2. Funciones básicas
3. Funciones de manipulación de datos
4. Ejercicios prácticos

## 1. Conceptos básicos

### Estructura de datos

En R todo es un objeto. Un objeto tiene una estructura de datos determinada. La estructura de datos de cualquier lenguaje de programación define cómo se pueden representar los datos en dicho lenguaje de programación. En R, los 6 tipos básicos de estructuras de datos se pueden organizar en si son homogéneos (todos los datos deben ser del mismo tipo) o heterogéneos (los datos pueden ser de distintos tipos) y, a su vez, cada tipo de datos pertenecerá una dimensionalidad determinada:

|Dimensionalidad|Homogéneos|Heterogéneos|
|:---:|:---:|:---:|
|1|Atomic vector|List|
|2|Matrix|Data frame|
|n|Array|-|

En esta gruía utilizaremos data frame y vectores.

Los tipos de datos más comunes son:
- `logical`: Equivalente a Boolean: TRUE FALSE
- `integer`: Números enteros
- `double`: Números reales
- `character`: Strings o texto
- `factor`: Usado para datos categóricos

Las relaciones de los conceptos anteriores son las siguientes:
- Al evaluar la clase de un vector en R, `integer` y `double` se clasifican indistintamente como numéricos.
- Un vector (1 dimensión) puede ser un vector atómico o una lista, aunque comúnmente se refiere a un vector atómico. 
- Los vectores atómicos se definen con `c()` como abreviatura de *concatenate*. Una lista se define por la función `list()`. 
- Un dataframe es una lista de vectores atómicos de la misma cantidad de elementos. 
- Un array es un objeto multidimensional de n dimensiones, una matriz es un array de dos dimensiones, y un vector atómico es una array de una dimensión. 
- Un objeto atómico es aquel que tiene un solo tipo de dato.
- Cada vector de un dataframe tiene un tipo de datos determinado.


## 2. Funciones básicas

R tiene datasets internas, por lo que podemos utilizarlas sin cargar ni crear base de datos alguna. Aquí utilizaremos `iris` y `mtcars`.

```r
head(iris) # Muestra las primeras 6 filas
```

```r
str(iris) # Muestra la estructura de la base
##'data.frame':	150 obs. of  5 variables:
## $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
## $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
## $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
## $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
## $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
```
Podemos ver que la estructura de la base `iris` tiene cuatro vectores numéricos y un vector categórico (factor). Seguimos con la base `mtcars`:

```r
str(mtcars) # Muestra la estructura de la base
head(mtcars) # Primeras 6 filas de la base
mean(mtcars$mpg) # Promedio de la columna mpg: 20.09 
sum(mtcars$cyl) # Promedio de la columna cyl: 6.19
max(c(1,2,3,4,5) # 5
min(c(1,2,3,4,5) # 1
mean(c(1,2,NA,4,5)) # NA
mean(c(1,2,NA,4,5), na.rm=TRUE) # 3
```
`NA` significa *Not Available*, lo que también es interpretado como un *missing value* o valores vacíos. Por regla general, en R cualquier cálculo determinado que uno quiera hacer sobre un conjunto de datos determinados, dará como resultado `NA` si es que tiene al menos un *missing value* dentro de los valores que calcula. Para ignorar los `NAs` hay que agregar el argumento `na.rm=TRUE` el cual significaría *NA remove = TRUE*.


## 3. Funciones básicas para la manipulación de datos

R viene con funciones pre-instaladas por default, como las que utlizamos anteriormente. Sin embargo, las funciones que más tienden a utilizarse en R (por su eficiencia y facilidad) son externas y, en consecuencia, para poder utilizarlas se debe instalar y abrir el "paquete" que contiene dicha función. Por definición, un paquete *es una colección de funciones de R, datos, y código compilado en un formato bien definido.* El directorio donde los paquetes son guardados se llama **library**. Las funciones que utlizamos anteriormente están en los paquetes pre-instalados, y las funciones que utilizaremos ahora están en un paquete llamado `tidyverse`.

```r
install.packages("tidyverse") # Instalamos el paquete tidyverse
library("tidyverse") # Abrimos el paquete
df <- read.csv("df.csv", sep=";", dec=",") # Leemos la base de datos y se lo asignamos a 'df'
head(df,5) # Selecciona las primeras 5 filas
```

```r
str(df)
##'data.frame':	100 obs. of  7 variables:
## $ Employee           : int  1 1 1 1 1 1 1 1 2 2 ...
## $ idProject          : int  9 83 96 20 48 27 28 49 89 58 ...
## $ ProjectType        : Factor w/ 3 levels "X","Y","Z": 2 3 2 3 3 3 3 1 1 1 ...
## $ Period             : int  1 5 2 5 1 5 5 1 2 4 ...
## $ Project_Performance: num  0.2 -0.2 0.02 0.54 0.92 0.58 0.03 -0.04 0.95 -0.53 ...
## $ Project_Size       : int  861 996 622 796 418 277 995 693 670 671 ...
## $ Boss               : Factor w/ 3 levels "A","B","C": 1 1 1 1 1 1 1 1 1 1 ...
```

**Estructura general de la base**
* La base tiene 100 proyectos con tamaños diferentes, cada proyecto está asociado a un empleado, y cada empleado está asociado a un único jefe.
* Los 100 proyectos se realizaron dentro de un total de 5 períodos.
* Cada proyecto puede ser de tres tipos: *X*, *Y*, o *Z*.
* Existen 12 empleados y 3 jefes (*A*, *B*, *C*)


|Función | ¿Qué hace?|
| :---:|:---:|
|`select()` | Selecciona columnas|
|`filter()` | Filtra filas|
|`arrange()` | Reordena|
|`mutate()` | Crea nuevas columnas|
|`summarize()`| Sintetiza valores|
|`group_by()`| Permite agrupar operaciones|


Con el fin de entender cómo aprender y entender cómo funciona cada función del R, es necesario acostumbrarse a ir a la documentación:

|Ejemplo| ¿Qué hace?|
|:---:| :---|
|`?select()`| Va directamente a la documentación especificada|
|`??select()`| Realiza un buscar en la documentación con la palabra especificada|

`help(select)` es equivalente a `?select`..
`help.search(select)` es equivalente a `??select`.


### Slicing, filtrar, y seleccionar

Slicing es una forma de seleccionar/filtrar un dataframe según los valores/condiciones que uno requiera, determinado por el/los índice/s de filas y columnas separados por una coma. Por ejemplo.
```r
df[3,] # Selecciona la fila 3 y todas la columnas de df
df[, 4] # Selecciona todas las filas de df y la columna 4
df[2,4] # Selecciona la fila número 2, y la columna número 4
df[3,1] # Selecciona la fila número 3, y la columna número 1
df[1:3,4] # Selecciona el rango de filas 1 a 3, y la columna 4
df[4,2:4] # Selecciona la fila 4, y el rango de columnas 2 a 4
df[1:4,3:5] # Selecciona el rango de filas 1 a 4, y el rango de columnas 3 a 5.
df[1:3, c(1,5,2:4)] # Selecciona el rango de filas 1 a 3, las columnas 1,5, y el rango de rolumnas 2 a 4
df[c(4,10,50,60,1:3), c(1,3,2,4:5)] # Selecciona las filas 4,10,50,60, el rango de filas 1 a 3, las columnas 1,3,2 y el rango de columnas 4 a 5
```
`c()` es la definición de un vector en R y se denota con una `c` de *concatenate*. En R, se pueden realizar combinaciones de selecciones de números de columnas como uno prefiera. Por tanto, siendo `a` y `b` vectores de una misma cantidad de elementos de números enteros, `df[a,b]`  extraerá las filas que indiquen los números enteros del vector `a`, y las columnas que indiquen los números enteros del vector `b`. 


Las columnas, además de su posición, también se pueden seleccionar por su nombre:
```r
df[c(1,4,3), c("Period", "Boss", "Employee")] # Selecciona las filas 1,4,3 con las columnas Period, Boss y Employee
```

Además, se pueden filtrar los valores de las columnas (las filas) según determinados valores con vectores `BOOLEAN`. Un vector `BOOLEAN` es el resultado de una o más condiciones:
```r
df$Period # Seleccionamos el vector Period del dataframe df
df$Period == 3 # Si los valores del vector Period son igual a 3 retornará TRUE, y FALSE en caso contrario.
x <- df$Period == 3 # Asignamos dicho vector a x 
df[x,] # Selecciona todas las filas donde el vector x sea TRUE
df[df$Period == 3,] # Equivalente a df[x,]

df$Project_Performance # Seleccionamos el vector Project_Performance del dataframe df
df$Project_Performance > 0.5 # Si el Performance del proyecto es mayor a 0.5 retornará TRUE, y FALSE en caso contrario
df[df$Project_Performance > 0.5,] # Selecciona todas las filas donde se cumpla la condición correspondiente

df[df$Period == 3 & df$Project_Performance > 0.5,] # Selecciona todas las filas donde el Period == 3 Y que el Performance del proyecto sea mayor a 0.5
df[df$Period == 3 & df$Project_Performance > 0.5,c("Employee")] # Selecciona todos los empleados que durante el período 3 hayan tenido un performance mayor a 0.5 en su proyecto

df[df$Period == 3 | df$Project_Performance > 0.5,] # Selecciona todas las filas donde el Period == 3 Ó que el Performance del proyecto sea mayor a 0.5
df[df$Period == 3 | df$Project_Performance > 0.5,c("Employee", "Boss")] # Selecciona todos los ids de los empleados, y su jefe respectivo, que hayan tenido algún proyecto durante el período 3 ó que hayan tenido un performance mayor a 0.5 en cualquier período

# NOTA: En caso de utilizar & y | en una misma condición, se evalúa primero &. En consecuencia, se hace necesario evaluar el uso de paréntesis en caso de utilizar ambas operaciones en una misma condición.
```


### select(), filter(), arrange()
```r
select(df, Period, Boss, Employee) # Selecciona las columnas Period, Boss, y Employee del dataframe df
select(df, idProject:Employee) # Selecciona las columnas desde idProject hasta Employee
select(df, -Period) # Selecciona todas las columnas menos la columna Period
select(df, ends_with("d")) # Selecciona todas las columnas cuyo nombre termine con "d"

# Variaciones útiles
?starts_with() # Selecciona columnas que comienzan con un character string
?ends_with() # Selecciona columnas que terminan con un character string
?contains() # Selecciona columnas que contienen un character string
?matches() # Selecciona columnas que coinciden con una regular expression

#Variaciones útiles
?select_if
?select_all
?select_at

filter(df, Period==3) # Selecciona todas las filas cuyo período sea igual a 3
filter(df, Period==3 & Boss == "A") # Selecciona todas las filas cuyo período sea igual a 3, y su jefe sea A
filter(df, Period==3 & (Boss == "A" | Boss == "C")) # Selecciona todas las filas cuyo período sea igual a 3, y su jefe sea A o C

#Variaciones útiles
?filter_if
?filter_all
?filter_at

arrange(df, idProject) # Ordena df de forma ascendente según la columna idProject
arrange(df, desc(idProject)) # Ordena df de forma descendente según la columna idProject

?subset # Otra metodología para filtrar y extraer columnas
subset(df, # dataframe
       subset = Period==3 & Employee == 10, # Condiciones para filtrar filas
       select = c(idProject, Project_Performance)) # Columnas a seleccionar

```


### mutate() y summarise()
```r
mutate(df, Nombre = Project_Performance*100) # Crea nuevas columnas
summarise(df, Promedio = mean(Project_Performance)) # Para cálculos de tendencia central
#NOTA: summarise() es distinto a summarize()


summarise(group_by(df, Employee),Promedio = mean(Project_Performance)) # Promedio por empleado
summarise(group_by(df, Boss),Promedio = mean(Project_Performance)) # Promedio por Jefe
summarise(group_by(df, Employee, Boss),Promedio = mean(Project_Performance)) # Promedio por empleado y por jefe
```

Pipe Operator (`%>%`) inserta en el primer argumento de la función a seguir, el resultado anterior. Matemáticamente f(x) es lo mismo que `x %>% f`, o g(f(x)) es lo mismo que `x %>% f() %>% g()`
```r
df %>%
  group_by(Employee) %>%
  summarise(Promedio = mean(Project_Performance)) # Promedio por empleado
df %>%
  group_by(Boss, Employee) %>%
  summarise(Promedio = mean(Project_Performance)) # Promedio por empleado y por jefe

#Adicionalmente, se puede modificar la inserción en el primer argumento de la función a seguir, especificando la posición con un '.':
# f(y, z = x) es lo mismo que x %>% f(y, z = .)
# f(x, y = nrow(x), z = ncol(x)) es lo mismo que x %>% f(y = nrow(.), z = ncol(.))
```


## 4. Ejercicios prácticos

Calcule:
1) La media simple de cada proyecto, por jefe
2) La media simple de cada proyecto, por período
3) La media simple de los proyectos tipo X
4) La media simple de los proyectos tipo Y ó Z
5) La media ponderada de los proyectos X y Z
6) La cantidad de proyectos asociado a cada jefe
7) El porcentaje de proyectos asociado a cada jefe, según el total de proyectos realizados
8) El desempeño de cada empleado, el cual está definido como el promedio ponderado del tamaño de los proyectos realizados por su nota respectiva
9) El desempeño de cada jefe, el cual está definido como el promedio ponderado del tamaño de los proyectos realizados asociados a sus empleados, por su nota respectiva
10) El nivel de liderazgo de cada jefe, el cual está definido como el promedio del desempeño de sus empleados asociados 

Documentación útil adicional: `?round` `?n`


### Resultados esperados

#### Ejercicio 1:

| Boss  |Promedio|
| :---: |   :---:|
| A     |    0.03|
| B     |    0   |
| C     |    0.11|
  
#### Ejercicio 2:
|  Period| Promedio|
| :---: |   :---:|
|      1|     0.14|
|      2|     0.11|
|      3|     0.03|
|      4|    -0.06|
|      5|    -0.05|

#### Ejercicio 3:

 | Promedio|
 |:---:|
|0.15|

#### Ejercicio 4:
  |Promedio|
  |:---:|
| -0.01|

#### Ejercicio 5:
  |Promedio|
|:---:|
|NaN|

#### Ejercicio 6:
|  Boss | n_Count|
| :---: |   :---:|
| A     |     44 |
| B     |     33 |
| C     |     23 |

#### Ejercicio 7:
|  Boss | p_Count|
| :---: |:---:|
| A     |   0.44|
| B     |   0.33|
| C     |   0.23|

#### Ejercicio 8:
|Employee |Employee_Performance|
|:---:|:---:|
|        1|                 0.17|
|        2|                -0.07|
|        3|                 0   |
|        4|                 0.17|
|        5|                -0.17|
|        6|                -0.04|
|        7|                -0.07|
|        8|                 0.19|
|        9|                -0.25|
|       10|                -0.09|
|       11|                 0.1 |
|       12|                 0.4 |

#### Ejercicio 9:
  |Boss|  Boss_Performance|
|:---:|:---:|
|A               |  0.05|
|B               | -0.05|
|C               |  0.12|

#### Ejercicio 10:
|  Boss | Boss_Leadership|
|:---:|:---:|
| A        |        0.02|
| B        |       -0.04|
| C        |        0.14|


### Problema

En los últimos años la empresa ha tenido significativas ganancias, por lo que ha decidido repartir 100 millones de pesos para bonos de forma retrospectiva a jefes y empleados de forma independiente, según las siguientes condiciones:
- Por cada período que el desempeño de un jefe o de un empleado supera el 0%, a dicho jefe o empleado se le dará un bono. 
- El tamaño de cada bono es ponderado por el tamaño total de todos los proyectos, cuyo Project_Performance sea mayor a 0%, asociados a todos los empleados y jefes que recibirán bono. 
- Lo máximo que puede ganar un empleado por período es 3MM y un jefe es 5MM. 

#### Calcule: 

**(a)** el bono más grande de los jefes.

**(b)** el bono más pequeño de los empleados.

**(c)** la diferencia total entre los bonos pagados a los jefes y los bonos pagados a los empleados.

**(d)** el excedente de los 100MM a repartir aproximado a la unidad de mil.


**Consideraciones:** Si un proyecto tiene un tamaño de 100, tiene un Project_Performance mayor a 0%, y el jefe y empleado asociados a dicho proyecto recibirán bono por dicho proyecto, al total del ponderado se le suma 200 (100 por el empleado y 100 por el jefe).



#### (a):

|Boss | Bono|
|:---:|:---:|
|C|16.680.648|

#### (b):

|Employee|Bono |
|:---:|:---:|
|2|1.475.219|	

#### (c):
|Diferencia|
|:---:|
|-5.769.712|

#### (d):
|Excedente|
|:---:|
|10.869.000|
