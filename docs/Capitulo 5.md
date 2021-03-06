# Capítulo 5: Representación y razonamiento con conocimiento probabilístico

[Anterior](https://github.com/EduPH/Apuntes-IA/blob/master/docs/Capitulo%204.md)


> Los agentes deben, en muchas ocasiones, hacer frente a distintos grados de incertidumbre debido a que no pueden ser observados todos los datos, indeterminismo, o una combinación de ambos. Por ello, lo correcto será tomar la decisión tomando en consideración la relevancia de distintos factores, el objetivo... Para ello definimos los llamados **Sistemas Basados en el Conocimiento (SBC)**.

Estos SBC también son llamados usualmente *Sistemas expertos*. Un SBC tiene distintos componentes que enumeramos a continuación:

+ **Conocimiento**: Necesitará ser representado y, posteriormente comentaremos distintas opciones y tomaremos una elección.
+ Mecanismos para **inferir nuevo conocimiento**.

Estos dos elementos constituyen módulos independientes. Además un SBC consta de la interfaz de usuario, subsistema de explicación del conocimiento inferido, de la adquisición de nuevo conocimiento y herramientas de aprendizaje. 

## Representación del conocimiento

Para la elección de un formalismo de representación se busca:
+ Potencia expresiva.
+ Facilidad de interpretación.
+ Eficiencia deductiva.
+ Posibilidad de explicación y justificación.

La lógica de primer orden, reglas, redes bayesianas... constituyen algunos formalismos de representación.

En muchas ocasiones realizar deducciones categóricas a partir del conocimiento que tenemos, ya sea por el trabajo excesivo que representa ser exhaustivo, porque no se tiene suficiente información, o no se puede llevar a cabo de manera práctica, no nos sirve . Solventaremos estos obstáculos aquí expuestos mediante una nueva forma de expresar el conocimiento, el grado de creencia. Para esta expresión del *conocimiento incierto* se emplea bagaje propio de la teoría de la probabilidad (TP). 

## Representación del conocimiento basado en la probabilidad

La probabilidad representará nuestro grado de creencia de que ocurra algo. Por ejemplo, si tenemos un dado de 10 caras. Si todas las caras tienen la misma probabilidad de salir, entonces decimos que nuestro grado de creencia de que salga, por ejemplo, el número 1 será de 0.1. Sea la **variable aleatoria** x que es *número que sale al lanzar el dado*, se tiene lo siguiente:

![formula](https://raw.githubusercontent.com/EduPH/Apuntes-IA/master/formulas/CodeCogsEqn.gif)

Es decir, decimos que la probabilidad de que la variable aleatoria x tome el valor 1 en su dominio es de 0.1 porque nuestro grado de creencia de que salga 1 es de esa probabilidad.

> La variable aleatoria formaliza las magnitudes numéricas asociadas a los resultados de un experimento aleatorio. 

Según el dominio del que tome sus valores una variable aleatoria, la denotaremos por variable aleatoria booleana, discreta o continua. Nosotros usaremos sobre todo variables aleatorias discretas (como el caso del dado). 

Vistas las variables aleatorias, el conocimiento será representado a través de éstas y de conectivas lógicas, configurando así las **proposiciones**. Antes hemos empleado una proposición muy sencilla, *que salga un 1 al lanzar el dado*. Y para determinar el 0.1 hemos adoptado la aproximación frecuentista, es decir, número de casos favorables entre número de casos totales. 


*Definición*: Una **función de probabilidad** sobre un espacio muestral ![formula](https://raw.githubusercontent.com/EduPH/Apuntes-IA/master/formulas/CodeCogsEqn-2.gif), es una función:

![formula](https://raw.githubusercontent.com/EduPH/Apuntes-IA/master/formulas/CodeCogsEqn-3.gif)

que satisface las siguientes propiedades:

+ ![formula](https://raw.githubusercontent.com/EduPH/Apuntes-IA/master/formulas/CodeCogsEqn-4.gif) para toda proposición a.
+ ![formula](https://raw.githubusercontent.com/EduPH/Apuntes-IA/master/formulas/CodeCogsEqn-5.gif) donde Prop denotará el conjunto de las proposiciones. 
+ La probabilidad de una tautología es 1 y la de una proposición insatisfacible 0. 


De esta definición y las tres propiedades axiomáticas a partir de la que hemos definido la función de probabilidad se infieren las siguientes propiedades:

+ ![formula](https://raw.githubusercontent.com/EduPH/Apuntes-IA/master/formulas/CodeCogsEqn-6.gif)
+ ![formula](https://raw.githubusercontent.com/EduPH/Apuntes-IA/master/formulas/CodeCogsEqn-7.gif) donde a y b son disjuntos.
+ ![formula](https://raw.githubusercontent.com/EduPH/Apuntes-IA/master/formulas/CodeCogsEqn-8.gif) donde ![formula](https://raw.githubusercontent.com/EduPH/Apuntes-IA/master/formulas/CodeCogsEqn-9.gif) son los posibles valores de la variable aleatoria X.



[Anterior](https://github.com/EduPH/Apuntes-IA/blob/master/docs/Capitulo%204.md)


