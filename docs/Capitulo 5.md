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

La probabilidad representará nuestro grado de creencia de que ocurra algo. Por ejemplo, si tenemos un dado, 

![formula](https://raw.githubusercontent.com/EduPH/Apuntes-IA/master/formulas/CodeCogsEqn.gif)





[Anterior](https://github.com/EduPH/Apuntes-IA/blob/master/docs/Capitulo%204.md)


