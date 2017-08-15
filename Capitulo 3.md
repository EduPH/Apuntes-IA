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


[Anterior](https://github.com/EduPH/Apuntes-IA/blob/master/Capitulo%202.md)



