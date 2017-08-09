## Capítulo 2: Resolviendo problemas mediante búsqueda

[Anterior](https://github.com/EduPH/Apuntes-IA/blob/master/Capitulo%201.md)

> En este capítulo introducimos herramientas para que un agente encuentre una secuencia de acciones que lo lleven a alcanzar su objetivo.

Necesitamos introducir los *problem-solving
agent*, es decir, agentes que resuelven problemas. Éstos usan
representaciones atómicas, cada estado del mundo ambiente se considera
indivisible, sin estructura interna.

### Problema como espacio de estados

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

#### 8 puzzle

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
2. **Acciones**: Intercambiar hueco por número adyacente.
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

[Anterior](https://github.com/EduPH/Apuntes-IA/blob/master/Capitulo%201.md)
