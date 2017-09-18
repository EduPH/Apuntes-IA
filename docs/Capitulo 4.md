# Capítulo 4: Búsqueda local y algoritmos genéticos

[Anterior](https://github.com/EduPH/Apuntes-IA/blob/master/docs/Capitulo%203.md) [Siguiente](https://github.com/EduPH/Apuntes-IA/blob/master/docs/Capitulo%205.md)



### Tabla de contenidos: 

+ [Búsqueda local](#búsqueda-local)
  - [Búsqueda en escalada](#búsqueda-en-escalada)
  - [Enfriamiento simulado](#enfriamiento-simulado)
  - [Algoritmos genéticos](#algoritmos-genéticos)


Las prácticas en python relativas a este capítulo se encuentran [aquí](https://github.com/EduPH/IA-Practicas/blob/master/practica-03.py).


En este capítulo relajaremos las asumpciones que nos simplificaban los problemas, 
consiguiendo así un acercamiento a los problemas del mundo real. 

Primero trabajaremos la **búsqueda local** en espacio de estados, evaluando y 
modificando uno o más estados en vez de la exploración sistemática de caminos
desde un estado inicial. Estos algoritmos son adecuados para aquellos problemas 
en los que prima llegar al estado final más que el coste de alcanzarlo.

Posteriormente, relajamos las asumpciones de determinismo y observabilidad.
El agente no consigue predecir lo que va a recibir y tendrá que adaptarse a posibles contingencias. 

## Búsqueda local

Trabaja con un solo nodo y se desplaza, generalmente, a nodos vecinos. Presenta las siguientes propiedades:

+ Utiliza poca memoria. 
+ Son capaces de manejar problemas con una cantidad de estados infinita. 
+ Se emplea en problemas en los que importa la configuración final, no el orden en el que se van ejecutando acciones. 

Para comprender la búsqueda local, podríamos imaginarnos una gráfica en la que en el eje de abscisas se representan
los distintos estados y el eje de ordenadas representa costes o un determinado valor que asocie lo buena que sería esa solución.
De esta manera, la solución será el mínimo en la gráfica si hablábamos de costes y el máximo si se trata de esa "función valor".

### Búsqueda en escalada

Introduzcamos este tipo de búsqueda local a través de una descripción con el siguiente pseudocódigo:

```
FUNCION ALGORITMO-ESCALADA(problema) devolver (estado que es máximo local)

	nodo_actual <- CREAR-NODO(problema-estado-inicial)
    loop do
		vecino <- (sucesor con mayor valor que nodo_actual)
		if vecino-valor <= nodo_actual-valor then return nodo_actual
		nodo_actual <- vecino
```

Este algoritmo se va desplazando entre nodos, yendo siempre a un vecino que tenga un valor más alto.
Cuando encuentra un nodo cuyos vecinos tienen valor más bajo que él se
para, ha alcanzado un pico. Se puede apreciar que este algoritmo no
mantiene en la memoria un árbol de búsqueda, sólamente necesita
recordar el estado y el valor de la función objetivo. 

Un gran inconveniente de este algoritmo es que no aseguro la solución
óptima. La manera de sobrellevar este problema suele ser tomar estados
iniciales aleatorios e iterar el proceso para, posteriormente, tomar
el mejor de los resultados obtenidos. Además, cabe mencionar que la
eficiencia depende en gran medida de la función que genera los
sucesores. 

### Enfriamento simulado

Este algoritmo pretende combinar la búsqueda en escalada y un random
walk o paseo aleatorio (introducción a procesos estocásticos
[aquí](https://matesland.wordpress.com/2017/07/06/introduccion-procesos-estocasticos/)),
obteniendo así un algoritmo completo y eficiente. Veamos su
descripción en pseudocódigo:

```
FUNCION ENFRIAMIENTO-SIMULADO(T-INICIAL,FACTOR-DESCENSO, N-ENFRIAMIENTOS,N-ITERACIONES)
1. Crear las siguientes variables locales:

   1.1 TEMPERATURA (para almacenar la temperatura actual),
       inicialmente con valor T-INICIAL.
   1.2 ACTUAL (para almacenar el estado actual), cuyo valor
       inicial es GENERA-ESTADO-INICIAL().
   1.3 VALOR-ACTUAL igual a F-VALORACIÓN(ACTUAL)
   1.4 MEJOR (para almacenar el mejor estado
       encontrado hasta el momento), inicialmente ACTUAL.
   1.5 VALOR-MEJOR (para almacenar el valor de MEJOR),
       inicialmente igual a VALOR-ACTUAL
2. Iterar un número de veces igual a N-ENFRIAMIENTOS:

   2.1 Iterar un número de veces igual a N-ITERACIONES:
	   2.1.1 Crear las siguientes variables locales:
	   
		 2.1.1.1 CANDIDATA, una solución vecina de ACTUAL, generada por GENERA-SUCESOR.
		 2.1.1.2 VALOR-CANDIDATA, el valor de CANDIDATA.
		 2.1.1.3 INCREMENTO, la diferencia entre VALOR-CANDIDATA y VALOR-ACTUAL
       
	   2.1.2 Cuando INCREMENTO es negativo, o se acepta probabilísticamente la solución candidata,
              hacer ACTUAL igual a VECINA y VALOR-ACTUAL igual a VALOR-VECINA.
       
	   2.1.3 Si VALOR-ACTUAL es mejor que VALOR-MEJOR,
              actualizar MEJOR con ACTUAL
              y VALOR-MEJOR con VALOR-ACTUAL.
   2.2  Disminuir TEMPERATURA usando FACTOR-DESCENSO

3. Devolver MEJOR y VALOR-MEJOR
```

Está inspirado en el proceso físico-químico de enfriamiento de
metales. De manera intuitiva, se aceptan probabilísticamente estados
"peores", dicha probabilidad de estos estados varía según el
incremento en la función de valoración. Con esto se consigue salir de
óptimos locales pero sin salir del óptimo global. 

### Algoritmos genéticos

En estos algoritmos los sucesores son producto de la combinación de
dos estados. Se comienza con la **población** que es un conjunto de
*k* estados generados aleatoriamente, siendo cada **individuo**
representado por una cadena en un alfabeto finito. 

No existe un solo algoritmo genético. En estos algoritmos la función
de valoración se suele denominar **fitness**. 

Veamos su descripción en pseudocódigo:
```
INICIAR población
EVALUAR cada individuo de la población
Repetir hasta CONDICIÓN_DE_TERMINACIÓN
   SELECCIONAR padres
   COMBINAR pares de padres
   MUTAR hijos resultantes
   EVALUAR nuevos individuos
   SELECCIONAR individuos para la siguiente generación
Devolver el mejor de la última generación
```
Ahora comentemos con más detenimiento las distintas fases del
algoritmo. 

+ *Población inicial*: generalmente **N** individuos completamente
  aleatorios. 

+ *Combinación de individuos*: Se trata de la combinación de la
  información de los padres para obtener nuevos hijos a partir de unos
  operadores. 
	  - Hay distintas posibilidades de cruce: en un punto, multipunto,
		  uniforme...

+ *Mutación*: Se trata de con una determinada probabilidad cambiar
  algunos genes, siendo aleatorio también el gen que es
  modificado. Los genes no son más que el material genético básico de
  la representación de los individuos. 

+  *Terminación del algoritmo*: puede determinarse por cantidad de
   generaciones, cuando se completen un número dado de generaciones
   sin mejorar o hasta un valor prefijado de la función de
   valoración. 
  
 Finalmente, es importante el sistema de selección de los individuos
de la población. Esta selección debe favorecer a los mejores, permitir
la diversidad y tener cierta aleatoriedad. 

Algunos ejemplos son: proporcional a su valoración, torneo y
élite+aleatoriedad. 

**Selección proporcional a su valoración**: Se pretende seleccionar
aleatoriamente los individuos de manera que cada uno tenga una
probabilidad de ser seleccionado acorde a su valoración. Así, la
probabilidad de que cada individuo sea seleccionado es 
![formula](http://latex.codecogs.com/gif.latex?%5Clarge%20P%28i%29%3D%20%5Cfrac%7BF%28i%29%7D%7B%5Csum_%7Bj%3D1%7D%5En%20F%28j%29%7D).


*Nota*: este método sólo se puede emplear para problemas de
maximización. 

**Selección por torneo**: Para cada selección se extraen **k**
individuos y se selecciona el mejor. Cuanto mayor sea **k**, mayor es
la *presión evolutiva*. 

**Selección elitista**: se escoge un porcentaje de los mejores y el
resto se toma de manera aleatoria entre los restantes. 


[Anterior](https://github.com/EduPH/Apuntes-IA/blob/master/docs/Capitulo%203.md)   [Siguiente](https://github.com/EduPH/Apuntes-IA/blob/master/docs/Capitulo%205.md)



