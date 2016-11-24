---
title: Nuestra programación es mala para el desarrollo de software
author: msanjuan
layout: post
date: 2015-12-28T22:22:29+00:00
url: /nuestra-programacion-es-mala-para-el-desarrollo-de-software/
dsq_thread_id:
  - 4441826012
categories:
  - Sin categoría

---
# La programación de mi color favorito

El otro día mi hija me preguntó cuál era mi color favorito. Respondí inmediatamente sin pensarlo: &#8220;el azul&#8221;. No era la primera vez que me lo preguntaban y es un dato que tengo claro desde que era niño. O no.

En esta ocasión, no se exactamente porqué, después de responder a mi hija algo en mi cabeza empezó a retorcerse: &#8220;¿es el azul mi color favorito?&#8221;. De repente me vi cuestionando algo que estaba grabado a fuego en mi cerebro desde hace tanto tiempo que no soy capaz de recordar cuando decidí que el azul era mi color favorito.

Si me paro a pensar detenidamente, en mi caso esta respuesta está completamente programada desde mi infancia y ha perdurado hasta hace unos días como una verdad absoluta e incuestionable. Pero si lo analizo, cada vez que tengo que tomar una decisión que implique decantarme por un color, el azul nunca es mi elección. Seguro que en algún momento de mi infancia el azul era mi color favorito y este dato se hizo hueco en mi cerebro, acomodándose de tal modo que no había vuelto a cuestionarme su vigencia hasta ahora.

Alguno podría estarse preguntando ahora mismo: &#8220;¿y qué carajo tiene esto que ver con el desarrollo del software?&#8221;

Personalmente creo que tiene mucho que ver, porque es algo que me encuentro constantemente cuando trabajo con equipos de desarrollo. Cada cierto tiempo me enfrento a &#8220;programaciones&#8221; de este tipo que, en lugar de estar relacionadas con el color favorito o el equipo de fútbol preferidos, están relacionadas con el uso de una tecnología, la aplicación de un patrón o la preferencia a la hora de aplicar una determinada práctica en detrimento de otra.

# Los principios sobre las prácticas

Es frecuente que, después de que un equipo de desarrollo tome una decisión, se olviden las razones que llevaron la toma de esa decisión. Es más, es frecuente que esa decisión se convierta en una verdad inamovible e incuestionable hasta tal punto que, aunque las razones que originaron esa decisión sean modificadas, da igual, nadie se plantea si la decisión debe replantearse o no.

Es importante poder recurrir siempre a los principios que originan la decisión de aplicar las prácticas. Cuando el debate gira entorno a la práctica, olvidando el principio, estamos perdidos. Es como debatir sobre una solución sin recordar el problema. Cuando esto sucede, lo habitual termina siendo un debate en el que se pierde completamente el foco y terminamos en una espiral en la que lo que debatimos es cómo solucionar el nuevo problema que ha generado la solución propuesta, en lugar de pensar si es posible buscar una solución alternativa al problema original sin que ello conlleve una reacción en cadena de problemas.

# Localiza bien el problema

Cuando llega el momento de enfrentarse a un problema, funciona muy bien dedicar unos minutos a poner por escrito cuál es exactamente el problema que queremos resolver. Una vez hecho esto, mi recomendación dedicar unos minutos más a pensar si ese es realmente es el problema a resolver o en realidad estamos condicionados por un problema generado por la solución actual. Una vez que hemos salido de la espiral de los problemas generados por las soluciones, estaremos en verdadera disposición de debatir sobre el problema.

En ocasiones tendremos que adoptar soluciones a problemas grandes que generan problemas pequeños y nos veremos obligados a trabajar sobre esas bases, pero lo más importante es no olvidar nunca los principios o perderemos la capacidad de cuestionar dichas soluciones porque la respuesta siempre será: &#8220;no se, se decidió así y así lo hemos hecho desde entonces. Mejor no tocarlo por si acaso&#8221;.

# No sólo a nivel de código

Este tipo de problemas nos los encontramos constantemente en otras facetas del desarrollo de software. Es común terminar con procesos de despliegue enrevesados, en los que mitigamos el dolor complicando el proceso con soluciones que a la larga generan más focos de dolor y que nos sumergen en la espiral de la que ya he hablado. Esto es frecuente cuando llega el momento de plantear despliegues automáticos de sistemas complejos o en el desarrollo de los propios procesos de negocio.

# Cuidado con el diagnóstico

Otro de los problemas comunes es criticar el principio después de tener una experiencia traumática aplicando la práctica. Otro es justo el contrario, estigmatizar la práctica cuando el error fue tratar de aplicarla sobre un problema para el que no era adecuada. Algunos ejemplos podrían ser:

  * Enfocar de forma errónea un entorno de integración continua y que la conclusión sea que la integración continua no funciona.
  * Invertir la <a href="http://martinfowler.com/bliki/TestPyramid.html" target="_blank">pirámide de test</a> y llegar a un punto en el que los tests son muy frágiles y caros de mantener, pero concluir que hacer tests automáticos no merece la pena.
  * Decidir utilizar una base de datos no relacional para solucionar un problema más adecuado para una relacional y descartar el uso de base de datos no relacionales para cualquier cosa.

# No nos olvidemos de entregar software

La realidad de todo proyecto es que siempre hay algo que se puede hacer mejor. Siempre hay un proceso que se puede optimizar, los tests pueden ser más completos y legibles, el código se puede refactorizar o se puede buscar otro enfoque. Pero no hay que volverse loco, no hay que olvidar que desarrollamos software con una finalidad y, si no lo entregamos, los usuarios no lo podrán utilizar. Desarrollar software y no entregarlo es lo mismo que no desarrollar software.