# Capítulo 3: Planificación


[Anterior](https://github.com/EduPH/Apuntes-IA/blob/master/Capitulo%202.md)  [Siguiente](https://github.com/EduPH/Apuntes-IA/blob/master/Capitulo%204.md)



> **Planificar** es encontrar una secuencia de acciones que alcanzan
> un determinado objetivo si se ejecutan desde un determinado estado
> inicial. Dicha secuencia de estados componen lo que se llama
> **plan**.

Cuando trabajamos en un problema y queremos hacer un planning debemos
dar una representación del mundo y de las acciones que lo
modifican. Además, debemos pensar algoritmos de búsqueda de
planes. Por otro lado, también es necesario tener en cuenta la
complejidad en espacio y tiempo. 

En nuestro caso, consideraremos las siguientes condiciones que
simplificarán:

+ Número finito de estados.
+ Completamente observables.
+ Acciones deterministas, es decir, están completamente especificadas.
+ Las acciones actuarán sin duración (tiempo implícito).
+ Se planifica a priori.
+ Hipótesis del mundo cerrado. 

> Es claro que nosotros nos estamos limitando a problemas que podemos
> abordar con búsqueda en espacio de estados. En el mundo real hay
> además que tener en cuenta que los estados tendrán una
> representación muy compleja, la cantidad de acciones posibles puede
> ser muy grande ... 

Para trabajar con estos problemas se
empleará como herramienta la lógica que dará representación a estados,
acciones y objetivos, y algoritmos que actuarán sobre esa
representación. 

## PDDL

**PDDL** son las siglas de *Planning Domain Definition Language*, es
decir, se trata un lenguaje que describirá todos los elementos del
problema. Nosotros como dijimos antes, usaremos una representación
basada en la lógica. 

En un problema será necesario describir los distintos *objetos* que
conformen el mundo. Por ejemplo, si trabajamos en un almacen y tenemos
una serie de contenedores apilados, cada contenedor será un objeto y
los representaremos a través de constantes en mayúscula. 

Posteriormente, es sensato describir de alguna manera el *estado
inicial* de dichos objetos en forma de relaciones o propiedades que se
establecen entre ellos o con el mundo. En el caso de los contenedores,
sobre qué contenedor están, debajo de cual, si no tienen ninguno
debajo o si no tienen ninguno encima. Estas propiedades serán
representadas por medio de símbolos de predicados. 

Así, ya podemos representar un *estado inicial*, así como el final si
añadimos *objetos genéricos*, que serán representados por variables. 

Lo que sigue es poder expresar acciones a través de un esquema
compuesto por:
+ Un predicado que actúa sobre distintos objetos.
+ Una precondición, que define cuándo tiene sentido ejecutar la
  acción.
+ El resultado al llevar a cabo la acción, es decir, cómo se queda el
  mundo tras el efecto de la acción. 
 
Finalmente, el *objetivo* será una conjunción de estados. 

### Ejemplo de las torres de Hanói

El juego consiste en tres varillas con una cantidad indeterminada de
discos de distintos radios, nosotros consideraremos siete discos.  

1. Configuración inicial: siete discos situados en la primera varilla
   ordenados por orden creciente en radio. 
   
2. Predicados de posición: 
	+ Encima de (X,Y): describe que el disco X está sobre Y, siendo Y
	 otro disco o el suelo. 
	+ En varilla (X,n): que el disco X está sobre la varilla n. 

3. Movimientos posibles, consideramos X el disco al que se aplica la
   acción. Cada disco tendrá un nombre dado por una constante
   mayúscula.  
   
	  + Mover a la derecha (o izquierda) el disco (X).
	 	- Precondiciones: 
			* Hay una varilla a la derecha (o izquierda) de la que está situada.
			
	   		* Si existe un disco en la varilla a la que se desplaza, éste es de radio mayor que el de X. 
			
			* El disco X se encuentra en la posición más arriba en su varilla. 
			
		 - Efecto de la acción:
		 	* El disco X se encuentra ahora en la varilla adyacente correspondiente.  

4. Objetivo: colocar los siete discos en el mismo orden de la
   configuración inicial, pero en la tercera varilla. 

```
Init(Encima_de(A,B)∧Encima_de(B,C)∧Encima_de(C,D)∧Encima_de(D,E)∧Encima_de(F,G)∧Encima_de(G,Mesa)∧En_Varilla_1(A,B,C,D,E,F,G))
Goal(Encima_de(A,B)∧Encima_de(B,C)∧Encima_de(C,D)∧Encima_de(D,E)∧Encima_de(F,G)∧Encima_de(G,Mesa)∧En_Varilla_3(A,B,C,D,E,F,G))
Action(Mover_derecha(a),
	Precond: ¬(En_Varilla_3(a))	¬(Encima_de(b,a)∧libre_derecha(a)),
	Effect: Encima_de(a,x)∧(En_Varilla_(n+1)(a))
``` 

Hemos hecho un intento de formalización, no sería del todo correcto
pero es un acercamiento más que aceptable.

+ ```Encima_de(a,b)``` determina si a está encima de b.

+ ```En_Varilla_n(a)``` determina si a está en la varilla n.

+ ``` Libre_derecha(a)``` determina si en la varilla de la derecha hay
  un disco mayor que a encima del todo. 
  
Faltaría escribir la acción ```Mover_izquierda(a)``` pero es análoga a
  la descrita.
  
### Búsqueda hacia delante

Hemos visto cómo un problema de planificación se puede plantear como
un problema de espacio de estados. Con lo visto hasta ahora, no es de
extrañar que una de las estrategias para la *búsqueda* de planes sean
algoritmos de búsqueda vistos con anterioridad. Comentemos el
algoritmo de búsqueda hacia delante en profundidad. 

El **algoritmo de búsqueda hacia delante en profundidad** es un
algoritmo de tipo *Backtracking*, es decir, se ordenan por heurística
los sucesores del estado actual. 

*Nota*: Los sucesores son los estados posibles alcanzables desde el
estado actual. 

Presenta las siguientes propiedades:

+ Es completo y siempre termina aunque no garantiza la solución más
  corta.
+ Tiene una complejidad en tiempo exponencial. 
+ Tiene complejidad en espacio lineal . 
+ Su eficiencia depende de la bondad de la heurística. 

Presentamos una descripción en pseudocódigo:

```
FUNCION BUSQUEDA-EN-PROFUNDIDAD-H(ESTADO-INICIAL,OBJETIVO,ACCIONES)
 Devolver BEP-H-REC({},{},ESTADO-INICIAL,OBJETIVO,ACCIONES)

FUNCION BEP-H-REC(PLAN,VISITADOS,ACTUAL,OBJ,ACCIONES)
1. Si ACTUAL satisface OBJ, devolver PLAN
2. Hacer APLICABLES igual a la lista de acciones que sean instancias
   de una acción de ACCIONES, que sean aplicables a ACTUAL y cuya
   aplicación no resulte en un estado de VISITADOS
3. Hacer ORD-APLICABLES igual a ORDENA-POR-HEURISTICA(APLICABLES)
4. Para cada ACCION en ORD-APLICABLES
   4.1 Hacer E’ el resultado de aplicar ACCION a ACTUAL
   4.2 Hacer RES igual a 
	   BEP-H-REC(PLAN·ACCION,VISITADOS U {E’},E’,OBJ,ACCIONES) 
   4.3 Si RES no es FALLO, devolver RES y terminar
5. Devolver FALLO
```


### Búsqueda hacia atrás 

Como alternativa a la búsqueda hacia delante, surge la búsqueda hacia
atrás. La antes descrita presenta el problema de que si uno no da con
la heurística adecuada pueden generarse múltiples ramificaciones con
acciones que no nos acercan al estado objetivo. 

En el caso de la búsqueda hacia atrás, como es predecible, se comienza
desde el estado objetivo y se aplican acciones inversas pretendiendo
alcanzar el estado inicial. Las acciones que se irán llevando a cabo
serán aquellas que sean consideradas relevantes respecto el objetivo. 

Necesitamos describir con mayor detenimiento lo que se considera una
**acción relevante**:

+ Al menos uno de los efectos de la acción está en el objetivo. 
+ Ninguno de los efectos de la acción aparece en el objetivo con
  distinto signo. 
  
Son acciones que podrían parecer ser la última acción de un plan que
llevara al objetivo. 
  

Antes comentamos los sucesores y es normal y necesario hablar de lo
que se denominan predecesores. El objetivo predecesor de un objetivo **G**
respecto de una acción **A** será
![formula](http://latex.codecogs.com/gif.latex?%28G-%5Ctext%7Befectos%7D%28A%29%29%20%5Ccup%20%5Ctext%7Bprecond%7D%28A%29)
. 

Presentamos una descripción en pseudocódigo:

```
FUNCION BUSQUEDA-HACIA-ATRÁS-H(ESTADO-INICIAL,OBJ,ACCIONES)
  Devolver BHA-H-REC({},{},ESTADO-INICIAL,OBJ,ACCIONES)

FUNCION BHA-H-REC(PLAN,VISITADOS,ESTADO-INICIAL,G-ACTUAL,ACCIONES)
1. Si ESTADO-INICIAL satisface G-ACTUAL, devolver PLAN
2. Hacer RELEVANTES igual a la lista de acciones que sean instancias
    de una acción de ACCIONES, que sean relevantes para G-ACTUAL
    y tal que el predecesor de G-ACTUAL respecto de la acción
    no sea un objetivo que contiene a alguno de VISITADOS
3. Hacer RELEVANTES-ORDENADOS
     igual a ORDENA-POR-HEURISTICA(RELEVANTES)
4. Para cada ACCION en RELEVANTES-ORDENADOS
   4.1 Hacer G’ el objetivo predecesor de G-ACTUAL respecto a ACCION
   4.2 Hacer RES igual a
	   BHA-H-REC(ACCION·PLAN,VISITADOS U {G’}, ESTADO-INICIAL,G’,ACCIONES)
   4.4 Si RES no es FALLO, devolver RES y terminar
5. Devolver FALLO
```


Hasta ahora no hemos introducido variables en este algoritmo de
búsqueda, pero puede darse (seguramente) que requiramos
introducirlas. De hecho, a veces es conveniente no instanciar
completamente, es decir, en vez de dar una lista de acciones
relevantes de la siguiente manera: ``` ACCION(A1,n), ... ,
ACCION(Am,n)```, podemos dejar el primer argumento sin especificar, es
	decir, ```ACCION(x,n)```. Para trabajar con esto, es necesario
	usar **unificadores de máxima generalidad** quedando el siguiente
	pseudocódigo: 
	
```
FUNCION BUSQUEDA-HACIA-ATRÁS-H-U(ESTADO-INICIAL,OBJ,ACCIONES)
  Devolver BHA-H-U-REC({},{},ESTADO-INICIAL,OBJ,ACCIONES)

FUNCION BHA-H-U-REC(PLAN,VISITADOS,ESTADO-INICIAL,G-ACTUAL,ACCIONES)
1. Si ESTADO-INICIAL satisface G-ACTUAL, devolver PLAN
2. Hacer RELEVANTES igual a la lista de pares (A,SIGMA) tales que:
   * A es una acción (estandarizada) de ACCIONES,
      relevante para ESTADO
   * SIGMA es umg entre los efectos de A que hacen relevante
      a la acción respecto de los correspondientes literales
      de G-ACTUAL
   * El predecesor de ACTUAL respecto de la acción no es un
      objetivo con un subconjunto de literales que unifica con
      alguno de los objetivos VISITADOS
3. Hacer RELEVANTES-ORDENADOS
    igual a ORDENA-POR-HEURISTICA(RELEVANTES)
4. Para cada (A,SIGMA) en RELEVANTES-ORDENADOS
   4.1 Hacer G’ el objetivo predecesor de G-ACTUAL
        respecto a (A,SIGMA); es decir,
        G’=(SIGMA(G-ACTUAL) - Efectos(SIGMA(A)))
                U Precondiciones(SIGMA(A))
   4.2 Hacer RES igual a
	   BHA-H-U-REC((A,SIGMA)·SIGMA(PLAN),VISITADOS U {G’}, ESTADO-INICIAL,G’,ACCIONES)
   4.3 Si RES no es FALLO, devolver RES y terminar
5. Devolver FALLO
```

### Heurística para planificación 

No hemos descrito en ambos algoritmos anteriores (hacia delante y
hacia atrás) la función que ordenaba por heurística. La heurística se
convierte en un elemento esencial que determinará la eficiencia de los
algoritmos. Como vimos en el capítulo anterior, la heurística debe
estimar la distancia entre estados y objetivos, en nuestro caso ésto
se cumple tal cual en la búsqueda hacia delante, pero en la búsqueda
hacia atrás hay que matizar. La heurística en la búsqueda hacia atrás
mide la distancia desde cada objetivo hacia el estado inicial. Si las
estimaciones de distancias están por debajo del número de acciones
real, diremos que la heurística es *admisible*. 

#### Heurística ![formula](http://latex.codecogs.com/gif.latex?%5Clarge%20%5CDelta_0)

A veces nos basamos en problemas relajados, es decir, que simplifican
el problema para el cálculo de heurísticas. Una de ellas es la
heurística
![formula](http://latex.codecogs.com/gif.latex?%5Clarge%20%5CDelta_0)
que definimos a continuación: 

![formula](http://latex.codecogs.com/gif.latex?%5Clarge%20%5CDelta_0%28e%2Cp%29%20%3D%20%5Cleft%5Clbrace%20%5Cbegin%7Barray%7D%7Bll%7D%200%20%26%20%5Ctextup%7Bsi%20%7D%20p%20%5Ctextup%7B%20aparece%20en%20%7De.%20%5C%5C%20&plus;%20%5Cinfty%20%26%20%5Ctextup%7Bsi%20%7D%20p%20%5Ctextup%7B%20no%20aparece%20en%20%7De%20%5Ctextup%7B%20ni%20en%20los%20efectos%20positivos%20de%20ninguna%20accion.%7D%20%5C%5C%20min_A%5C%7B%201&plus;%20%5Csum_%7Bq%5Cin%20precond%5E&plus;%28A%29%7D%20%5CDelta_0%28e%2Cq%29%7C%20p%5Cin%20efectos%5E&plus;%28A%29%5C%7D%20%26%20%5Ctextup%7Ben%20otro%20caso.%7D%20%5Cend%7Barray%7D%20%5Cright.)


![formula](http://latex.codecogs.com/gif.latex?%5Clarge%20%5CDelta_0%20%28e%2Cg%29%20%3D%20%5Csum_%7Bp%5Cin%20g%7D%20%5CDelta_0%28e%2Cp%29)


Donde e es un estado, p es un átomo y g un objetivo que solo tiene
literales positivos. El superíndice positivo en precond y efectos
indica literales positivos. 

Así, se están relajando las condiciones del problema, suponiendo que
no hay precondiciones ni efectos negativas. 


[Anterior](https://github.com/EduPH/Apuntes-IA/blob/master/Capitulo%202.md)  [Siguiente](https://github.com/EduPH/Apuntes-IA/blob/master/Capitulo%203.md)



