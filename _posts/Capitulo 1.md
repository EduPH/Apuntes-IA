---
layout: post
title:  "Capítulo 1"
date:   2017-9-1
---

# Capítulo 1: Agentes inteligentes


El primer elemento interesante que necesitamos distinguir y definir son los agentes. Un *agente* percibe el ambiente a través de **sensores** 
y actúa sobre lo que le rodea a través de *actuadores*. Cuando hablamos de percibir, nos referimos a las entradas de información recibidas en cualquier
instante dado, siendo así una *secuencia de percepción* un historial completo de todo lo que el agente haya percibido.


Con todos estos elementos, el agente puede actuar de distinta manera, y su comportamiento estará descrito por la *función del agente* que está influida
por la secuencia de percepción, llevándolo a actuar de una determinada manera. Aquí necesitamos distinguir dos elementos clave, la anteriormente definida
*función del agente* que es un elemento externo, y el *programa del agente*, es decir, la implementación interna que funciona con algún sistema físico.

Hemos caracterizado un agente genérico, y ahora debemos calificar su comportamiento.

## Buen comportamiento: Racionalidad

Un agente racional es aquel que actúa de manera adecuada, ampliémos este concepto. Hemos dicho que cuando un agente actúa lo hace sobre el ambiente que
lo rodea, y eso implica que lo modifica. Cuando el efecto sobre el ambiente es el deseado, entonces se ha comportado correctamente. La forma
de cuantificar si el efecto es el deseado se llama *medida de actuación*, a veces será que el ambiente se quede en un estado concreto, y otras querremos
optimizar ciertas acciones, se adaptará a las circunstancias y objetivos del agente.


Lo que es racional depende de cuatro elementos:
	+ La medida de actuación que define el criterio de éxito.
	+ El conocimiento (a priori) del agente sobre el ambiente.
	+ Las acciones que el agente puede realizar.
	+ La percepción secuencial del agente hasta el momento.

Con esto podemos definir lo que es un **agente racional**:
> Para cada posible percepción secuencial, un agente racional debe elegir una acción que maximice su medida de actuación, basándose en la evidencia dada por su percepción y cualquier conocimiento que el agente tenga.

[Índice](https://github.com/EduPH/Apuntes-IA/blob/master/README.md)  [Siguiente](https://github.com/EduPH/Apuntes-IA/blob/master/posts/Capitulo%202.md)


