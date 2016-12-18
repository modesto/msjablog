+++
author = ""
title = "La arquitectura de software como habilidad"
draft = false
date = "2016-12-17T15:34:32+01:00"
slug = "la-arquitectura-de-software-como-habilidad"
tags = []
image = "images/dand.jpg"
comments = true     # set false to hide Disqus comments
share = true        # set false to share buttons
menu = ""           # set "main" to add this content to the main menu
+++

## El pícaro en AD&D 2ª edición

Advanced Dungeons & Dragons 2ª edición tenía un sistema de reglas bastante restrictivo que puede ser resumido con un ejemplo bastante sencillo: el pícaro era el único tipo de personaje que tenía permitido intentar esconderse entre la sombras o moverse sigilosamente.

Si, es así de fácil. Si no eras un pícaro, esas dos habilidades estaban fuera de tu alcance.

En su disculpa he de decir que AD&D 2ªed. es del año 1989 y las reglas originales son del 74. Algunos años después, este despropósito se corrigío porque no tenía ningún sentido.

Y es que resulta mucho más interesante jugar bajo un sistema de reglas en el que cualquier jugador puede intentar moverse sigilosamente, dependiendo esta acción de su nivel de habilidad y de su contexto. Evidentemente, no es lo mismo intentarlo si llevas una armadura de placas metálica que si vas desnudo con calcetines de felpa.

## El arquitecto de software

A día de hoy, a las puertas del año 2017, muchas empresas se empeñan en establecer sistemas de reglas que rigen el funcionamiento de sus equipos de desarrollo como si de partidas de AD&D 2ªed. se tratase.

Me refiero especialmente a esos equipos en los que existe una figura de "arquitecto" que, en la línea del pícaro, es el único que tiene permitido desempeñar tareas relacionadas con la arquitectura del software.

## La arquitectura como habilidad

Estoy totalmente convencido de que la arquitectura de software debería tratarse como una habilidad en lugar de como un rol.

Como toda habilidad, es posible aprenderla, cultivarla y ponerla en práctica sea cual sea nuestro nivel en ella. Por supuesto, dependiendo de nuestro nivel, los resultados podrán ser desde desastrosos ha plenamente satisfactorios, como pasa con la práctica de cualquier otra habilidad.

Enfocar la arquitectura de software como una habilidad en lugar de como un rol tiene muchas ventajas, tanto para el proyecto como para los integrantes del equipo.

### Propiedad colectiva del código

Extreme Programming nos habla de la [propiedad colectiva del código](http://www.extremeprogramming.org/rules/collective.html) como una forma de alentar a todos los miembros del equipo a participar en cualquier parte del código, sin limitarse a aquellas partes que ellos han desarrollado.

Nos invita a cuestionar el diseño, la arquitectura, a modificar cualquier parte del código, siempre con responsabilidad, por supuesto.

Las ventajas son múltiples y los beneficios compensan con creces:

* Elimina cuellos de botella.- Dejar la arquitectura en manos de una única persona no escala y además es malísimo para el bus factor.
* Reduce silos de conocimiento.
* Ayuda a crear equipo.- Cuando un equipo se construye a base de un sistema de castas, dificulta tener sensación de pertenencia.
* Promueve el incremento de la calidad del software. Evidentemente no lo garantiza, pero es que no hay NADA que lo garantice.
* Facilita que suba el nivel del equipo.- Promover la propiedad colectiva del código hace que aquellos que tienen su "skill" más baja, tiendan a subirla si realmente tienen interés.
* Mejora el crecimiento del equipo.- Cuando el negocio necesite que el equipo crezca, nada como que la mayoría del equipo tenga un conocimiento amplio del software. Situaciones del tipo "esto lo tenemods que ver con el arquitecto" desaparecen por completo. Esto está totalmente relacionado con la existencia de silos de conocimiento.

## Solucionando un problema que no es de software

Muchas veces se dan casos como estos:

* Soy arquitecto porque así gano mas dinero.- En este caso estamos hablando de solucionar un problema económico del ámbito personal, no del software. Esto es algo típico en grandes corps en las que el crecimiento económico está vinculado a una tabla de roles clásica. Como anécdota curiosa, hace muchos años fuí contratado como director de arte por una razón similar. Aún así, que te te denominen "arquitecto" no implica que debas ejercer de dictador de la arquitectura, se pueden seguir aplicando los criterios de la propiedad colectiva del código.
* El cliente demanda esa figura.- Una posible respuesta sería: "pues no trabajes con ese cliente". Evidentemente es una respuesta válida, pero no siempre es realista. En esos casos nada te impide hacer lo mismo que en el punto anterior, es simplemente una fachada de cara a trabajar con un cliente.  
* El equipo está muy verde.- El problema ha sido montar un equipo mal balanceado. Puede que esta sea una buena solución a corto plazo, pero un equipo no puede estar verde para siempre, se debería hacer un ejercicio proactivo en balancear el equipo y buscar su madurez.

Los dos primeros puntos tienen mucho que ver con una cuestión de "hacking cultural". Para mi es vital regular bien la cantidad de energía empleada para realizar estas acciones de hacking. En ocasiones puede merecer la pena cambiar de cliente/trabajo y dedicar esa energía a activades más provechosas.

## Una cuestión cultural

¿Quiero decir con esto que todo el mundo debería hacer de todo y saber de todo?

No necesariamente. Todo equipo está compuesto por personas cuyas habilidades están a diferentes niveles. Puede que unos sean más fuertes en un punto que otros, es normal. Pero también es normal que emerjan figuras de liderazgo de facto, algo muy diferente a la figura del arquitecto que ya he mencionado.

Un problema habitual es que, en el momento en el que responsabilizas a una persona de un aspecto del desarrollo, automáticamente el resto de los miembros del equipo pasan a tener una actitud totalmente pasiva respecto a ese aspecto.

## No es fácil

Por supuesto, hay mil casos y situaciones que pueden llevar a que tener una figura de este tipo sea una buena idea, no lo niego. Pero creo que todos ellos son siempre casos coyunturales con fecha de caducidad que deberían tender a evolucionar en forma de un equipo sólido, maduro y equilibrado. 

Está claro que no es fácil tener un equipo equilibrado, en el que la mayoría de sus integrantes no sólo se preocupen de la arquitectura, sino que además tengan la capacidad de hacerlo. ¿Pero quién ha dicho que sea fácil? Hacer software que perdure en el tiempo, que se mantenga estable y con un nivel de mantenilibilidad adecuado no es fácil.

Si el enfoque de tu empresa es tener una máquina de hacer churros en la que se contrata programadores a kilo, tal vez la idea de tener arquitectos que sobrevuelan por los proyectos tallando la arquitectura sobre tablas de piedra resulte lo más normal. Pero es que esto nos lleva al punto anterior, adoptar una solución para un problema que no debería existir.

** La imagen de la cabecera es de [Svetlana Vorobyeva](http://svetlaya777.deviantart.com/)