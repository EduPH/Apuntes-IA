# Capítulo 2: Resolviendo problemas mediante búsqueda

[Anterior](https://github.com/EduPH/Apuntes-IA/blob/master/Capitulo%201.md)

> En este capítulo introducimos herramientas para que un agente encuentre una secuencia de acciones que lo lleven a alcanzar su objetivo.

Necesitamos introducir los *problem-solving
agent*, es decir, agentes que resuelven problemas. Éstos usan
representaciones atómicas, cada estado del mundo ambiente se considera
indivisible, sin estructura interna.


## Tabla de contenidos

+ [Problema como espacio de estados](#problema-como-espacio-de-estados)
  - [Problema del 8-puzzle](#8-puzzle)
  - [Problema de las jarras](#problema-de-las-jarras)
  
+ [Espacio de estados como un
  grafo](#espacio-de-estados-como-un-grafo)
  
+ [Buscando soluciones](#buscando-soluciones)

+ [Algoritmos de búsqueda](#algoritmos-de-búsqueda)
	- [Estrategias de búsqueda desinformada](#estrategias-de-búsqueda-desinformada)
	 	* [Búsqueda en anchura](#búsqueda-en-anchura)
		 
	 	* [Búsqueda en profundidad](#búsqueda-en-profundidad)
	- [Heurística en algoritmos de búsqueda informada](#heurística-en-algoritmos-de-búsqueda-informada)

## Problema como espacio de estados

La primera característica indispensable es fijar un *objetivo* para
poder, en función a lo que se quiere alcanzar, **formular el
problema**. Formalmente consta de cinco componentes:
1. **Estado inicial** en el que empieza el agente. 
2. **Acciones** que puede efectuar el agente.
3. **Modelo de transición** que describe el efecto de las acciones del agente. 
4. **Test final** que determina si el estado final concuerda con el
   objetivo fijado.
5. **Coste** que asocia a cada lista de acciones un valor numérico de coste.   
	

> Los tres primeros elementos componen el **espacio de estados**.


Así, una solución será una lista de acciones que desembocan en
alcanzar el objetivo. 

Tenemos las definiciones básicas para trabajar con unos cuantos
ejemplos. Por ello, a continuación describiremos una serie de
problemas juguete. 

En python, podemos definir previamente una clase para un problema de
estados, y así, cada problema heredará las propiedades.

```
	class Problema(object):
		"""Clase abstracta para un problema de espacio de estados. Los problemas
    concretos habría que definirlos como subclases de Problema, implementando
    acciones, aplica y eventualmente __init__, es_estado_final y
    coste_de_aplicar_accion. Una vez hecho esto, se han de crear instancias de
    dicha subclase, que serán la entrada a los distintos algoritmos de
    resolución mediante búsqueda."""  


    def __init__(self, estado_inicial, estado_final=None):
        """El constructor de la clase especifica el estado inicial y
        puede que un estado_final, si es que es único. Las subclases podrían
        añadir otros argumentos"""
        
        self.estado_inicial = estado_inicial
        self.estado_final = estado_final

    def acciones(self, estado):
        """Devuelve las acciones aplicables a un estado dado. Lo normal es
        que aquí se devuelva una lista, pero si hay muchas se podría devolver
        un iterador, ya que sería más eficiente."""
        pass

    def aplica(self, estado, accion):
        """ Devuelve el estado resultante de aplicar accion a estado. Se
        supone que accion es aplicable a estado (es decir, debe ser una de las
        acciones de self.acciones(estado)."""
        pass

    def es_estado_final(self, estado):
        """Devuelve True cuando estado es final. Por defecto, compara con el
        estado final, si éste se hubiera especificado al constructor. Si se da
        el caso de que no hubiera un único estado final, o se definiera
        mediante otro tipo de comprobación, habría que redefinir este método
        en la subclase.""" 
        return estado == self.estado_final

    def coste_de_aplicar_accion(self, estado, accion):
        """Devuelve el coste de aplicar accion a estado. Por defecto, este
        coste es 1. Reimplementar si el problema define otro coste """ 
        return 1
```

### 8 puzzle

El problema del 8-puzzle se trata de una tabla de valores como la siguiente:

2 | 8 | 3
--- | --- | ---
1 | 6 | 4
7 |  | 5

tal que el hueco puede intercambiarse con las casillas que tocan un
lado con él. Así, hay que ir desplazando el hueco de manera que
podamos conseguir una tabla con la siguiente pinta:


1 | 2 | 3
--- | --- | ---
8 |  | 4
7 | 6 | 5


Esta sería una descripción informal del problema, desgranemos los
elementos definidos en la sección anterior.

1. **Estado inicial**: Una configuración como la antes dada.
2. **Acciones**: Intercambiar hueco por número adyacente. En
   particular
	   + Mover hueco arriba.
	   + Mover hueco abajo.
	   + Mover hueco izquierda.
	   + Mover hueco derecha.
3. **Modelo de transición**: Configuración resultado de realizar la
   acción.
4. **Test final**: Comprobación de que la configuración corresponde
   con la deseada.
5. **Coste**: Se fija como uno por acción.

Una vez definido el problema, podemos heredar las propiedades del
problema genérico anterior. Se ha tomado como medio de representación
de las configuraciones el tipo de dato tupla.

```
	class Ocho_Puzzle(Problema):
    """Problema a del 8-puzzle.  Los estados serán tuplas de nueve elementos,
    permutaciones de los números del 0 al 8 (el 0 es el hueco). Representan la
    disposición de las fichas en el tablero, leídas por filas de arriba a
    abajo, y dentro de cada fila, de izquierda a derecha. Por ejemplo, el
    estado final será la tupla (1, 2, 3, 8, 0, 4, 7, 6, 5). Las cuatro
    acciones del problema las representaremos mediante las cadenas:
    "Mover hueco arriba", "Mover hueco abajo", "Mover hueco izquierda" y
    "Mover hueco derecha", respectivamente."""""

    def __init__(self,tablero_inicial):
        self.estado_inicial = tablero_inicial
        self.estado_final = (1, 2, 3, 8, 0, 4, 7, 6, 5)

    def acciones(self,estado):
        pos_hueco=estado.index(0)
        accs=list()
        if pos_hueco not in (0,1,2):
            accs.append("Mover hueco arriba")
        if pos_hueco not in (0,3,6): 
            accs.append("Mover hueco izquierda")
        if pos_hueco not in (6,7,8): 
             accs.append("Mover hueco abajo")
        if pos_hueco not in (2,5,8):
             accs.append("Mover hueco derecha")
        return accs     

    def aplica(self,estado,accion):
        pos_hueco = estado.index(0)
        l = list(estado)
        if accion == "Mover hueco arriba":
            l[pos_hueco] = l[pos_hueco-3]
            l[pos_hueco-3] = 0
        if accion == "Mover hueco abajo":
            l[pos_hueco] = l[pos_hueco+3]
            l[pos_hueco+3] = 0
        if accion == "Mover hueco derecha":
            l[pos_hueco] = l[pos_hueco+1]
            l[pos_hueco+1] = 0
        if accion == "Mover hueco izquierda":
            l[pos_hueco] = l[pos_hueco-1]
            l[pos_hueco-1] = 0
        return tuple(l)
```


### Problema de las jarras

Hagamos una descripción del problema según los parámetros establecidos
con anterioridad:

1. **Estado inicial**: Dos jarras vacías. Una de 4 litros y otra de
   3 litros.
2. **Acciones**: 
   + Llenar jarra de 4 litros.
   + Llenar jarra de 3 litros.
   + Verter contenido de jarra de 4 litros en la de 3 litros.
   + Verter contenido de jarra de 3 litros en la de 3 litros.
   + Vaciar jarra de 3 litros.
   + Vaciar jarra de 4 litros.
3. **Modelo de transición**: Cantidad restante en cada jarra tras
   llevar a cabo una de las acciones.
4. **Test final**: Que en la jarra de 4 litros tengamos 2 litros.
5. **Coste**: Se fija como uno por acción.

Así, podemos implementarla en la clase Problema:

```
class Jarras(Problema):
	 """Problema de las jarras:
    Representaremos los estados como tuplas (x,y) de dos números enteros,
    donde x es el número de litros de la jarra de 4 e y es el número de litros
    de la jarra de 3"""

    def __init__(self):
        self.estado_inicial = (0,0)

    def acciones(self,estado):
        jarra_de_4=estado[0]
        jarra_de_3=estado[1]
        accs=list()
        if jarra_de_4 > 0:
            accs.append("vaciar jarra de 4")
            if jarra_de_3 < 3:
                accs.append("trasvasar de jarra de 4 a jarra de 3")
        if jarra_de_4 < 4:
            accs.append("llenar jarra de 4")
            if jarra_de_3 > 0:
                accs.append("trasvasar de jarra de 3 a jarra de 4")
        if jarra_de_3 > 0:
            accs.append("vaciar jarra de 3")
        if jarra_de_3 < 3:
            accs.append("llenar jarra de 3")
        return accs

    def aplica(self,estado,accion):
        j4=estado[0]
        j3=estado[1]
        if accion=="llenar jarra de 4":
            return (4,j3)
        elif accion=="llenar jarra de 3":
            return (j4,3)
        elif accion=="vaciar jarra de 4":
            return (0,j3)
        elif accion=="vaciar jarra de 3":
            return (j4,0)
        elif accion=="trasvasar de jarra de 4 a jarra de 3":
            return (j4-3+j3,3) if j3+j4 >= 3 else (0,j3+j4)
        else: #  "trasvasar de jarra de 3 a jarra de 4"
            return (j3+j4,0) if j3+j4 <= 4 else (4,j3-4+j4)

    def es_estado_final(self,estado):
        return estado[0]==2
```


## Espacio de estados como un grafo

Un espacio de estados se puede ver como un grafo dirigido. La raíz es
el estado inicial, y dicho nodo raíz se expande siendo sus sucesores
los estados resultantes de aplicar cada acción posible. En python
implementamos los árboles de búsqueda de la siguiente manera:

```
class Nodo:
    """Nodos de un árbol de búsqueda. Un nodo se define como:
       - Un estado
       - Un puntero al estado desde el que viene (padre)
       - La acción que se ha aplicado al padre para que se obtenga el
         estado del nodo
       - Profundidad del nodo
       - Coste del camino desde el estado inicial hasta el nodo.
       
       Definimos además, entre otros, los siguientes métodos que se
       necesitarán para generar el aŕbol de búsqueda: 
       - Sucesor y sucesores de un nodo (respesctivamente por una acción 
         o por todas las acciones aplicables al estado del nodo). Estos
         métodos reciben como entrada un  problema de espacio de estados.  
       - Camino (secuencia de nodos) que lleva del estado inicial al estado del
         nodo.
       - Solución (secuencia de acciones que llevan al estado) de un nodo.   
       """  

    def __init__(self, estado, padre=None, accion=None, coste_camino=0):
        self.estado=estado
        self.padre=padre
        self.accion=accion
        self.coste_camino=coste_camino
        self.profundidad=0
        if padre: 
            self.profundidad= padre.profundidad + 1

    def __repr__(self):
        return "<Nodo {0}>".format(self.estado)

    def sucesor(self, problema, accion):
        """Sucesor de un nodo por una acción aplicable"""
        estado_suc = problema.aplica(self.estado, accion)
        return Nodo(estado_suc, self, accion,
                    problema.coste_de_aplicar_accion(self.estado,accion)+self.coste_camino)

    def sucesores(self, problema):
        """Lista de los nodos sucesores por todas las acciones que le sean
           aplicables""" 
        return [self.sucesor(problema, accion)
                for accion in problema.acciones(self.estado)]

    def camino(self):
        """Lista de nodos que forman el camino desde el inicial hasta el
           nodo.""" 
        nodo_aux, camino_inverso = self, []
        while nodo_aux:
            camino_inverso.append(nodo_aux)
            nodo_aux = nodo_aux.padre
        return list(reversed(camino_inverso))

    def solucion(self):
        """Secuencia de acciones desde el nodo inicial"""
        return [nodo.accion for nodo in self.camino()[1:]]

    def __eq__(self, other):
        """ Dos nodos son iguales si sus estados son iguales. Esto significa que
        cuando comprobemos pertenecia a una lista o a un conjunto (con"in"), sólo
        miramos los estados. Si hay que nirar, por ejemplo, algo del coste,
        habrá que hacerlo expresamente, como se hará en la
        buśqueda_con_prioridad"""
        
        return isinstance(other, Nodo) and self.estado == other.estado

    def __lt__(self, other): 
        """La definición del menor entre nodos se necesita porque cuando se
        introduce un nodo en la cola de prioridad, con la misma valoración que
        uno ya existente, se van a comparar los nodos y por tanto es necesario que
        esté definido el operador <"""  
        return True
    
    def __hash__(self):
        """Nótese que esta definición obliga a que los estados sean de un tipo
        de dato hashable"""
        return hash(self.estado)  
``` 

En el caso del 8-puzzle, si partimos de la siguiente configuración
inicial:

	 p8p_1 = Ocho_Puzzle((2, 8, 3, 1, 6, 4, 7, 0, 5))

2 | 8 | 3
--- | --- | ---
1 | 6 | 4
7 |  | 5


Entonces tenemos tres acciones posibles. Podemos mover el hueco
arriba, derecha e izquierda. Y el resultado de ejecutar cada una de
las acciones generará un nodo sucesor. Es decir,

Primer sucesor:

	>>> p8p_1.aplica(p8p_1.estado_inicial, "Mover hueco arriba")
	(2, 8, 3, 1, 0, 4, 7, 6, 5)
	
2 | 8 | 3
--- | --- | ---
1 |  | 4
7 | 6 | 5


Segundo sucesor:

	>>> p8p_1.aplica(p8p_1.estado_inicial, "Mover hueco izquierda")
	(2, 8, 3, 1, 6, 4, 0, 7, 5)

2 | 8 | 3
--- | --- | ---
1 | 6 | 4
 | 7 | 5


Tercer sucesor:

	>>> p8p_1.aplica(p8p_1.estado_inicial, "Mover hueco derecha")
	(2, 8, 3, 1, 6, 4, 7, 5, 0)

2 | 8 | 3
--- | --- | ---
1 | 6 | 4
7 | 5 | 


Posteriormente se expandiría a su vez cada nodo generando el árbol. 

## Buscando soluciones

Una *solución* es una secuencia de acciones, y un *algoritmo de
búsqueda* funciona considerando distintas secuencias de acciones
posibles. Dichas secuencias generan árboles de búsqueda con el estado
inicial en la raíz, basándonos en la interpretación como grafos dada
antes. 

> Es importante tener en cuenta que pueden generarse bucles en nuestra
> búsqueda. Por ejemplo, en el caso del 8-puzzle, se pueden repetir
> estados al ser posible revertir las acciones.  


Podemos evaluar la actuación de un algoritmo de cuatro maneras:

+ **Completitud**: mide si el algoritmo garantiza encontrar una solución.
+ **Optimalidad**: mide si el algoritmo encuentra el mejor resultado.
+ **Complejidad temporal**: mide lo que tarda el algoritmo en
encontrar una solución.
+ **Complejidad de espacio**: mide el coste del algoritmo en términos
de memoria. 

Teniendo en cuenta esto, comentemos distintos algoritmos de búsqueda:

### Algoritmos de búsqueda

A lo largo de estas implementaciones se emplea código auxiliar sobre
colas, pilas y colas con prioridad. Se puede visualizar en el
siguiente [repositorio](https://github.com/EduPH/IA-Practicas), en
concreto en el documento
[algoritmos_de_busqueda](https://github.com/EduPH/IA-Practicas/blob/master/algoritmos_de_busqueda.py).

Ahora damos la definición en Python de una búsqueda genérica:

```
def busqueda_generica(problema, abiertos):
    """Búsqueda genérica, tal y como se ha visto en clase; aquí
    abiertos es una cola que se puede gestionar de varias maneras. 
    Cuando se llama a la función, el argumento abiertos debe ser la cola
    vacía. 
    Téngase en cuenta que en esta búsqueda, para ver si se repite un nodo,
    sólo se mira el estado. Por tanto, las búsquedas que usan coste (coste uniforme o
    A*, por ejemplo), no se pueden obtener como caso particular de ésta (serán
    casos particulares de búsqueda_con_prioridad)"""


    abiertos.append(Nodo(problema.estado_inicial))
    cerrados = set()
    while abiertos:
        actual = abiertos.pop()
        if problema.es_estado_final(actual.estado):
            return actual
        cerrados.add(actual.estado)
        nuevos_sucesores=(sucesor for sucesor in actual.sucesores(problema)
                          if sucesor.estado not in cerrados
                          and sucesor not in abiertos)
        abiertos.extend(nuevos_sucesores)
    return None
```


#### Estrategias de búsqueda desinformada

Se trata de algoritmos que no son capaces de distinguir si una vía va
a ser más prometedora que otra, si no que simplemente expanden nodos
siguiendo un cierto orden establecido, comprobando si han alcanzado su
meta. 

> Cuando decimos que se expande un nodo, en dicho nodo se describe un
> estado, y los distintos nodos sucesores que se generan al expandir
> son aquellos resultados de las distintas acciones posibles.


##### Búsqueda en anchura

En este algoritmo se expande primero la raíz y luego los de
profundidad siguiente, iterando dicha expansión ordenada por
nivel. Además, no se baja en profundidad hasta que todos los de un
determinado nivel están expandidos. Se comprueba si hemos alcanzado el
objetivos tras cada expansión de nodo. En la siguiente
[imagen](https://commons.wikimedia.org/wiki/File:Breadth-first-tree.svg)
podemos entender cómo funciona de manera intuitiva. 


![image](https://upload.wikimedia.org/wikipedia/commons/3/33/Breadth-first-tree.svg)



Es un algoritmo que resulta ser óptimo si el coste es una función no
decreciente a lo largo de la profundidad. Por ejemplo, cuando todas
las acciones tienen el mismo coste (el caso del 8-puzzle y las
jarras).

Es interesante estudiar su complejidad y para ello veamos el número de
operaciones necesarias si la solución se alcanzara a profundidad d y,
en el peor de los casos, en el último nodo de dicha
profundidad. Además, consideremos que el número de acciones por nodo
es b, apareciendo, por lo tanto, b nodos por expansión. 


![equation](http://latex.codecogs.com/gif.latex?%5Cfn_cm%20%5Clarge%20b&plus;b%5E2&plus;b%5E3&plus;%20...%20&plus;%20b%5Ed%20%3D%20O%28b%5Ed%29)  


es decir, hablamos de una complejidad exponencial. Además, visionando
una tabla del libro que usamos de referencia obtenemos lo siguiente:

1. La memoria es un problema mayor que el tiempo de ejecución.
2. Problemas de búsqueda de complejidad exponencial no pueden ser
   resueltos por métodos uniformes a no ser que sean los de menor
   tamaño. 
   
   > Para ciertos problemas, el tiempo de ejecución podría ser
   >abordable, pero no existen ordenadores con capacidad suficiente.
   
Su implementación en Python empleando la búsqueda genérica antes dada:

```
def busqueda_en_anchura(problema):
		return busqueda_generica(problema, ColaFIFO())
```

##### Búsqueda en profundidad

Este algoritmo expande siempre el nodo a mayor profundidad, y si hay
varios a misma profundidad, los hace siguiendo un orden. Lo
entenderemos en el siguiente [esquema](https://commons.wikimedia.org/wiki/File:Depth-first-tree.svg):

![image](https://upload.wikimedia.org/wikipedia/commons/1/1f/Depth-first-tree.svg)

Se emplea una cola LIFO mientras que en el algoritmo de búsqueda en
anchura se empleaba una cola FIFO. 

Su implementación en Python: 

	def busqueda_en_profundidad(problema):
		return busqueda_generica(problema, PilaLIFO())


#### Heurística en algoritmos de búsqueda informada

> Los **algoritmos de búsqueda informada** tienen más información útil
> sobre el problema que la que se destila de la formulación del
> mismo. 

La forma general de abordar un algoritmo de este tipo es a través de
la llamada **mejor búsqueda primero** o **búsqueda con prioridad**, que es una variante del
algoritmo de búsqueda genérico en árboles que antes se introdujo, pero
empleando una *función de evaluación* ![formula](http://latex.codecogs.com/gif.latex?%5Clarge%20f%28n%29) que está definida como una
estimación de costes. Así, el nodo con una evaluación menor es el que
se expande primero y la elección de dicha función de evaluación
establece la estrategia del algoritmo. 

En Python lo implementamos de la siguiente manera:

```
def busqueda_con_prioridad(problema, f):
    """Búsqueda que gestiona la cola de abiertos ordenando los nodos de menor
    a mayor valor de f. Tanto la búsqueda por primero el mejor, como la
    búsqueda óptima y la búsqueda A* son casos particulares de esta búsqueda,
    usando distintas f's (heurística, coste y coste más heurística,
    respectivamente)."""
    
    actual = Nodo(problema.estado_inicial)
    if problema.es_estado_final(actual.estado):
        return actual
    abiertos = ColaPrioridad(min, f)
    abiertos.append(actual)
    cerrados = set()
    while abiertos:
        actual = abiertos.pop()
        if problema.es_estado_final(actual.estado):
            return actual
        cerrados.add(actual.estado)
        for sucesor in actual.sucesores(problema):
            if sucesor.estado not in cerrados and sucesor not in abiertos:
                abiertos.append(sucesor)
            elif sucesor in abiertos:
                nodo_con_mismo_estado = abiertos[sucesor]
                if f(sucesor) < f(nodo_con_mismo_estado): 
                    del abiertos[nodo_con_mismo_estado]
                    abiertos.append(sucesor)
    return None
```

> Los algoritmos de *búsqueda con prioridad* suelen tener como
> componente de la función de evaluación la que se llama **función
> heurística** ![formula](http://latex.codecogs.com/gif.latex?%5Clarge%20h%28n%29).

![formula](http://latex.codecogs.com/gif.latex?%5Clarge%20h%28n%29%20%3D%20%5Ctext%7Bcoste%20estimado%20del%20camino%20con%20menor%20coste%20desde%20el%20estado%20%7D%20n%20%5Ctext%7B%20hasta%20el%20estado%20final%7D)

Existen distintos algoritmos de búsqueda con prioridad. Los
implementamos en Python:

```
def busqueda_coste_uniforme(problema):
    """Busqueda óptima: gestiona la cola en orden creciente de coste del camino"""
    return busqueda_con_prioridad(problema, lambda x: x.coste_camino)


def busqueda_primero_el_mejor(problema, h=None):

    """Búsqueda por primero el mejor: gestionar la cola de menor a mayor
    heurística.  
    NOTA: en realidad, al hacer que primero_el_mejor sea un caso
    particular de búsqueda_con_prioridad, estamos poniendo en este algoritmo
    un test innecesario. Cuando el estado de un nodo generado es el mismo que
    el del un nodo que está en abiertos o en cerrados, su valoración (su
    heurística en este caso) es la misma que el nodo que ya está en la lista,
    ya que la heurística sólo depende del estado. Sin embargo,
    búsqueda_con_prioridad comprueba si su valoración es menor o no (en este
    caso inútilmente). Hemos preferido dejarlo así para enfatizar la
    estructura común de estos algoritmos"""

    return busqueda_con_prioridad(problema,lambda n: h(n.estado))


def busqueda_a_estrella(problema, h=None):
    """A*: la cola se ordena por f(n) = g(n)+h(n) (coste más heurística)."""

    return busqueda_con_prioridad(problema,lambda n: n.coste_camino + h(n.estado))
```





[Anterior](https://github.com/EduPH/Apuntes-IA/blob/master/Capitulo%201.md)
