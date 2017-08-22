# Capítulo 3: Planificación


[Anterior](https://github.com/EduPH/Apuntes-IA/blob/master/Capitulo%202.md)



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
 
[Anterior](https://github.com/EduPH/Apuntes-IA/blob/master/Capitulo%202.md)



