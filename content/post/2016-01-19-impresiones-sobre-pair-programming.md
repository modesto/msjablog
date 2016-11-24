---
title: Impresiones sobre pair programming
author: msanjuan
layout: post
date: 2016-01-19T01:13:56+00:00
url: /impresiones-sobre-pair-programming/
dsq_thread_id:
  - 4503270234
categories:
  - Sin categoría

---
El viernes mantuve una conversación bastante entretenida sobre desarrollo de software con un compañero de profesión. Trabaja en una entidad financiera y al hablar de cosas como TDD, Agile, XP, etc. tenía ciertos sentimientos enfrentados. Precisamente uno de los temas que le causaban controversia era el pair programming. Su opinión básicamente se resumía en una frase: &#8220;yo lo que no compro es que dos tíos senior sentados juntos a programar van a ser más productivos que si programan por separado&#8221;.

Así que me he animado a escribir un post con mi opinión sobre este tema y aportar así mi punto de vista (aunque a nadie le importe xD).

# Antecedentes

Me encanta programar solo. Disfruto muchísimo sentado delante del teclado mientras me enfrento a un reto técnico o busco una aproximación de diseño que me convenza. También me encanta pasarme horas leyendo código de proyectos open source para ver cómo hacen las cosas otros equipos de desarrollo. Disfruto haciendo generadores de código que generan código para generar generadores de código. En resumen, me encanta liarme la manta a la cabeza y perderme las horas muertas en todo lo que tenga que ver con el desarrollo de software.

Pero que me guste no significa que piense que sea lo mejor cuando hablo de un contexto profesional. El desarrollo de software no es únicamente mi profesión, es mi afición. Por eso todas estas cosas que me gustan, las hago principalmente cuando tengo libre y, cuando estoy trabajando, decido lo que hacer buscando el criterio que más le convenga al proyecto.

Actualmente llevo algo más de un año trabajando en un proyecto en el que hacemos mucho pair programming, así que no puedo decir que sea un veterano en esto, pero creo que ha pasado un tiempo lo suficientemente grande como para tener una opinión desde la experiencia.

# Lo bueno

Para mi los aspectos positivos más interesantes de esta práctica son:

  * Beneficia a la **propiedad colectiva del código**.- Esto facilita la eliminación del efecto &#8220;es que eso lo hizo fulanito y sólo él puede meterle mano&#8221;. Si además de pair programming, introducimos rotación dentro de los equipos, el beneficio es aún mayor.
  * El **código es mejor**.- Nada como 4 ojos pendientes del código que se está generando. Como mínimo, permitirá garantizar que al menos que ha existido consenso entre dos de cara a la legibilidad, diseño y estructura del código.
  * Las **pruebas** son **mejores**.- Me cuesta recordar una sesión de pair programming en la que, fruto del diálogo entre ambas partes, no haya surgido algún test de triangulación para probar un caso límite.
  * Promueve la definición y utilización del **lenguaje ubícuo**.- Cuando programas solo prestas menos atención a los nombres y, aunque se la prestes, careces de una figura muy importante de cara a encontrar el nombre más adecuado, el debate.

# Lo malo

Como todo, también tiene algunos aspectos negativos, para mi estos son los principales:

  * Es **difícil** practicarlo correctamente.- Quien piense que el pair programming se limita a que dos tíos se sienten al lado y se intercambien el teclado, va muy desorientado. Al final pondré algunos enlaces con consejos al respecto.
  * **Fatiga**.- Si, 4 horas de pairing cansan mucho. Y más si no se practica bien. Un truco, es importante respetar los descansos o el cerebro termina frito.

# ¿Cuándo?

Dicho esto, tampoco creo que deba hacerse el 100% del tiempo. Creo que esta práctica aporta mucho valor cuando estamos hablando de escribir código de negocio y mucho menos cuando hablamos de UI e infraestructura. Cuando hablamos de dominios que estamos explorando y es evidente que el cuello de botella no va a ser el ritmo al que escribamos código, es precisamente uno de los escenarios en los que más beneficios nos va a proporcionar esta práctica.

Normalmente no me parece práctico recurrir al pair programming para escribir código de infraestructura o código de una vista. A no ser que estemos haciendo mentoring, pero eso sería otro tema.

Por supuesto, siempre hay excepciones y ya sabemos que la respuesta casi siempre es &#8220;depende&#8221;.

# Más información

Sobre el tema hay mucho escrito, pero tengo recientes en mi cabeza estos dos posts que a su vez enlazan a otro montón de ellos:

<a href="http://juandavidvega.es/blog/?p=194" target="_blank">Pair programming: Mi guía práctica</a>

<a href="http://www.carlosble.com/2015/07/productive-pair-programming/" target="_blank">Productive Pair Programming</a>

&nbsp;