---
title: La regla del boy scout
author: msanjuan
layout: post
date: 2016-11-10T10:01:15+00:00
url: /la-regla-del-boy-scout/
dsq_thread_id:
  - 5293120432
categories:
  - Sin categoría

---
Hay discipinas en las que dicen que a largo plazo es más importante la constancia que el talento. Creo que en el desarrollo de software tener constancia es una virtud.

Conforme va avanzando un proyecto, es normal que el diseño sufra cambios (preferiblemente pequeños) y a la vez el código puede sufrir altibajos de calidad.

Tener un conocimiento escaso del problema, equivocarnos al enfocar la solución y muchos otros factores pueden llevarnos a una circunstancia muy habitual: estamos enfocando una historia de usuario y pasamos por una parte de nuestro código que no se entiende bien, está mal enfocada, es complicada de extender, en general, nos está impidiendo avanzar de forma fluida hacia nuestro objetivo, entregar valor.

## Patadón palante

Un intento de solución que he visto con frecuencia es el del &#8220;patadón palante&#8221; haciendo una ñapa para sacar la historia lo antes posible y luego, cada cierto tiempo, se mete una historia técnia en el sprint para eliminar deuda técnica y listo. Ron Jeffries describe muy bien este problema en [Refactoring not on the backlog][1]

No voy a negar que hay veces que es una solución perfectamente válida en circunstancias en las que es realmente importante cumplir con un plazo. El problema lo veo cuando se convierte en práctica habitual. Si tu proyecto requiere constantemente recurrir al &#8220;patadón palante&#8221;, tal vez deberías replantearte las fechas, el alcance y/o la forma en la que estás enfocando el desarrollo de tu software.

## La regla del boy scout

Otra opción es asegurarnos de que en cada historia de usuario que entregamos y en cada incidencia que resolvemos, el código está mejor de como lo encontramos. Uncle Bob lo llama aplicar la &#8220;regla del boy scout&#8221;, Fowler lo llama [refactor oportunista][2], pero en esencia vienen a decir lo mismo.

Una de las grandes ventajas de aplicar esta regla es que se dedican más cuiadados a las zonas que más se tocan, lo que nos ayuda a no terminar teniendo una [big ball of mud][3] y terminar con un desastre inmantenible.

Alguno podrá pensar &#8220;pero entonces te estás dejando partes de tu software sin &#8216;limpiar'&#8221;. En efecto, pero son partes que no están dando problemas (no tienen incidencias) y que no estamos necesitando tocar (no hay HU), así que, por el momento, ¡no hay necesidad de dedicarles ni un minuto!

## Poco a poco

Da igual el estado en el que se encuentre nuestro software, si de manera constante terminamos nuestra jornada y está un poco mejor que cuando la empezamos, a largo plazo veremos que nuestro flujo de entrega de valor es muchísimo mejor que antes.

No es necesario tardar tres veces más en hacer una historia de usuario porque nos embarcamos en un refactor de dimensiones monstruosas, con que esté un poco mejor es suficiente.

## La excusa del tiempo

Habitualmente escucho la excusa de &#8220;es que no tenemos tiempo para eso, tenemos que entregar las historias de usuario lo antes posible&#8221;. Esa afirmación es un arma de doble filo porque si no prestamos a nuestro código la atención necesaria, el tiempo que tardaremos en terminar las historias de usuario será cada vez mayor y es muy probable que el número de incidencias se incremente.

## Mantenernos en una solución simple

Recurrir a arquitecturas y diseños demasiado complejos tiende a crear software rígido que pierde capacidad de maniobra de cara al futuro. No olvidemos de que es precisamente en el futuro cuando tendremos más conocimiento del problema y, por lo tanto, estaremos más cualificados para ofrecer mejores soluciones. Anclarnos a arquitectures encorsetadas desde el principio puede dar una sensación muy &#8220;enterprise&#8221;, pero se convierte en un problema en el futuro.

En esta línea recomiendo leer [Understanding the Four Rules of Simple Design][4].

## El coste de deshacer

A menudo tenemos en cuenta únicamente el coste de hacer, pero es muy importante tener en cuenta también el coste de deshacer.

Recurrir a código de terceros no es malo, puede ahorrarnos muchísimas horas de trabajo en aspectos que posiblemente no tengan nada que ver con nuestro negocio. Pero al hacerlo debemos escoger muy bien la forma en la que lo hacemos.

Esta es una de las razones por las que se suele hablar de la preferencia de usar librerías vs frameworks. El coste de deshacer cuando hablamos de una librería puede ser relativamente bajo si además hemos creado las abstracciones necesarias. Cuando escogemos acoplarnos a un Framework, el coste de deshacer es más alto.

Por supuesto, es una cuestión de equilibrio y hay muchas veces en las que el coste a pagar por utilizar un framework merece completamente la pena.

Aunque no habla exáctamente de este tema, me gusta mucho el vídeo [&#8220;8 Lines of Code&#8221;][5] de Greg Young, en el que habla del coste de atarnos a soluciones mágicas.

## Conclusión

Debemos intentar que nuestro código sea siempre un poquito mejor. La única forma de hacer que nuestro código sea mejor es mejorar nosotros. Si identificamos que tomamos malas decisiones con demasiada frecuencia, tenemos que deshacer mucho de lo que hacemos, no somos capaces de alcanzar un flujo de entrega adecuado, tal vez tengamos que plantearnos invertir un poco de tiempo en mejorar. Existen multitud de recursos que nos pueden ayudar a desarrollar mejor, tenemos las comunidades y los eventos de comunidad, cursos online y presenciales, blogs de un nivel bestial&#8230; lo que no tenemos es excusa.

&nbsp;

##

 [1]: http://ronjeffries.com/xprog/articles/refactoring-not-on-the-backlog/
 [2]: http://martinfowler.com/bliki/OpportunisticRefactoring.html
 [3]: https://blog.codinghorror.com/the-big-ball-of-mud-and-other-architectural-disasters/
 [4]: https://leanpub.com/4rulesofsimpledesign
 [5]: https://www.infoq.com/presentations/8-lines-code-refactoring