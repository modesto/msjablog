+++
author = ""
title = "Consejos para una sesión de Event Storming"
draft = false
date = "2021-06-09T00:00:00+01:00"
slug = "consejos-para-una-sesion-de-event-storming"
tags = []
image = "images/event-storming.jpg"
comments = true     # set false to hide Disqus comments
share = true        # set false to share buttons
menu = ""           # set "main" to add this content to the main menu
+++

\
Hace unos días, algunos compañeros de trabajo realizaron unas sesiones de Event Storming y me gustaría recopilar algunos consejos que les ayudaron a hacer las sesiones más productivas y a eliminar ciertos bloqueos mentales. También añadiré algunos que no les di en ese momento pero que creo que pueden ser de utilidad ;)

### Lo primero, ¿Qué es Event Storming?

Para quien no conozca este tipo de sesión, podríamos resumirlo como un tipo de dinámica muy flexible que ayuda a explorar/descubrir/visualizar fácilmente aspectos complejos del negocio. Es una dinámica muy versátil que se puede utilizar en muchos ámbitos, pero no quiero profundizar sobre ello en este post.

Como hablar de [Event Storming](https://www.eventstorming.com/) daría para un montón de posts, de momento me voy a conformar con enlazar a la web oficial, enlazar un par de vídeos interesante y, a partir de ahí, se puede tirar del hilo.

### La dinámica está a nuestro servicio

Uno de los aspectos más importantes es asumir que Event Storming no es una dinámica rígida. Debemos tener muy claro que la dinámica está a nuestro servicio y que adaptarla es algo que nos ayudará a conseguir nuestros objetivos.

Su creador, [Alberto Brandolini](https://twitter.com/ziobrando), dice que Event Storming es como una pizza, en la que somos libres de ponerle nuestros propios ingredientes.

Y, para que quede claro, no tengo problemas con las pizzas con piña o brócoli. Imagina lo que puedo llegar a ponerle a un Event Storming ;)

### "Es que nos cuesta pensar en eventos"

A priori, esta afirmación podría parecer un problema si hablamos de una sesión con la palabra "event" en su nombre, pero nada más lejos de la realidad.

Cuando se empieza a utilizar esta dinámica en sesiones de descubrimiento del dominio, hay veces que se produce cierto bloqueo debido a una especie de parálisis por análisis. Esto suele deberse a que las personas técnicas asocian los post-it naranjas a eventos que deberán ser publicados. Es normal realizar esta asociación porque creo que el 99% de lo que hay escrito sobre Event Storming habla de los post-it naranjas como "eventos de dominio".

Recordemos que estamos hablando precisamente de un momento en el que estamos descubriendo el dominio, se trata de un momento de bastante plasticidad, en el que prima el pensamiento divergente, y pretender asociar directamente los post-its naranjas a eventos que van a ser publicados es precisamente añadir un componente de rigidez que entorpecerá la dinámica.

En estos casos, mi consejo es que tratemos de quitar de nuestras cabezas el concepto de "evento de domino" y pensemos en "cosas relevantes que han sucedido a nivel de negocio".

Además, si pensamos en las personas de negocio que estarán en la sesión, ¿qué concepto será más fácil de entender para ellas?¿cuál está más cargado de una connotación técnica?. En una sesión en la que el objetivo es hablar del negocio, cuantas menos connotaciones del ámbito del diseño técnico metamos, mejor.

Por supuesto, esto mismo aplica a los comandos. No todos los comandos van a terminar teniendo un reflejo como tal en el sistema.

### Debemos poner el foco en el lenguaje ubícuo

Si hay un momento ideal para definir el lenguaje ubicuo, es precisamente en una sesión en la que juntamos a las personas que tienen el conocimiento del negocio con las personas que van a plasmarlo en forma de código.

Esta es una oportunidad que no debemos desaprovechar, especialmente porque en este tipo de sesiones sucede algo que, aunque parece obvio, no pasa siempre: que hablen sobre el mismo ámbito de negocio personas que pertenecen a distintos equipos o departamentos del área de negocio, sin contar al área técnica.

### "¿Es que no estamos aplicando DDD?"

Es cierto que esta dinámica se asocia mucho a DDD y que hay mucho material en internet que habla de cómo utilizarla para el descubirmiento de los agregados, bounded contexts, etc.

Desde luego que podemos usarla en esos ámbitos, pero esta dinámica tiene mucho que aportar incluso si no estamos aplicando DDD o no estamos en ese momento de la fase de descubrimiento. No pasa nada, recordemos que la dinámica está a nuestro servicio.

### Hay que cogerle el truco

Como todo, este tipo de dinámicas requieren de cierta práctica para dinamizarlas con soltura. Por mi consejo es practicarla primero varias veces en espacios "seguros", con poca gente, preferiblemente del mismo equipo, sin involucrar a personas del negocio. Algo así como hacer una kata de código, pero aplicado a la práctica del Event Storming.

Otro de los aspectos que más ayudan es que la persona que lo dinamice no sea parte activa de la sesión. Algo interesante es que sea una persona de otro equipo la que dinamice la sesión. Así esa persona puede centrarse al 100% en su misión: mantener el flow, evitar que alguien monopolice la conversación y, en general, favorecer el proceso de descubrimiento.

### En resumen

Estamos ante una dinámica muy versátil que nos puede ayudar mucho en procesos de descubrimiento o de puesta en común de conocimiento del negocio y, sobre todo, no debe preocuparnos que al principio cueste un poco. Como todo, ¡se mejora con la práctica!

### Algunos enlaces

[Alberto Brandolini - 100,000 Orange Stickies Later | Øredev 2019](https://www.youtube.com/watch?v=fGm62ra_mQ8)\
[Alberto Brandolini — The Precision Blade](https://www.youtube.com/watch?v=lG46Yo_9DPc)\
[Foto de la cabecera](https://commons.wikimedia.org/wiki/File:Event_Storming_example_process.jpg)
