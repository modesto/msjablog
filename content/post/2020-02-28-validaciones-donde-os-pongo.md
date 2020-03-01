+++
author = ""
title = "Validaciones, ¿dónde os pongo?"
draft = false
date = "2020-02-28T12:00:00+01:00"
slug = "validaciones-donde-os-pongo"
tags = []
image = "images/gandalf-balrog.jpg"
comments = true     # set false to hide Disqus comments
share = true        # set false to share buttons
menu = ""           # set "main" to add this content to the main menu
+++

## No, no estás soñando

Pues si, me ha dado por ponerme a escribir, así, ¡a lo loco! Y la "culpa" de esto la tienen dos tweets. 

Vicenç lanzó esta pregunta por twitter:
{{< tweet 1232616521983283201 >}}

Y, cuando la respuesta se estaba convirtiendo en una ristra de tweets, me acordé de este otro:
{{< tweet 1229164559761629189 >}}

Dejando a un lado que Jorge es un troll profesional, a veces tiene razón. Y este creo que es uno de esos casos en los que me voy a sentir más cómodo plasmando mi respuesta por escrito en un medio que me permita liarme un poco más la manta a la cabeza.

## Vale pero, y de las validaciones, ¿qué?

Lo primero de todo es la respuesta de rigor: DEPENDE

Y depende sobre todo de cómo estés enfocando la arquitectura de tu aplicación, de qué necesites y de hasta donde te quieras "complicar" la vida.

Así que, para convertir ese "depende" en algo un poco más concreto, voy a intentar explicarlo basándome en un modelo de arquitectura de referencia, de rabiosa actualidad y ya cada cual que extrapole a su escenario y saque sus conclusiones.

## Ejemplo con Clean Architecture

Cuando hablamos de Clean Architecture, lo habitual es mostrar este diagrama o alguna de sus variantes:
![Diagrama típico de Clean Architecture](/images/CleanArchitecture.jpg)

En Clean Architecture se establece una delimitación de responsabilidades que fija expectativas respecto a qué debe suceder en cada corte del diagrama. Por ejemplo:

* Cuando estamos hablando de negocio, se presupone que estamos en un entorno seguro en el que no tienen cabida aspectos relacionados con la autenticación o autorización.
* Ningún objeto de negocio debería poderse construir incumpliendo las invariantes del negocio.
* La capa de aplicación no entiende del mecanismo de entrega. Un caso de uso bien podría invocarse desde un controllador o desde una aplicación de línea de comandos.
* La autenticación generalmente será responsabilidad del framework web, pero la autorización lo será del caso de uso.

Y así podríamos seguir enumerando características de este tipo de patrones arquitectónicos, pero no es el objetivo de este post, sólo quería dar un poco de contexto.

Si además estamos aplicando DDD, tendremos que añadir más reglas, limitaciones y heurísticas a la lista, pero lo importane es que seamos conscientes de cuáles son, para que podamos evaluar en cada caso si las estamos cumpliendo. Incluso podríamos llegar a verificarlas de forma automática mediante tests de arquitectura.

Ojo, para el caso, me da igual Clean Architecture, Ports&Adapters, Onion Architecture o la que sea. También me da más o menos igual qué combinación de lenguaje y framework estemos utilizando. Al final lo importante es que estamos hablando, en esencia, de enfoques en los que se separa negocio de mecanismos de entrega y de los casos de uso.

## ¿Y eso de Mediatr?

Mediatr es una implementación del patron mediator para .Net que nos va a permitir ejecutar nuestros casos de uso beneficiándonos del contenedor de dependencias de .Net y además con las posibilidad de introducir comportamientos previos y posteriores a la ejecución del propio caso de uso. 

Si tomamos como referencia el anterior diagrama, la pregunta de Vicenç y nos movemos en un nivel de abstracción relativamente alto, podríamos suponer que una llamada típica a nuestra API sería algo así como esto:

> Llamada al API -> Asp.Net -> Controlador -> Mediatr -> Caso de uso -> Entidad

A veces puede resultar un poco raro porque lo que estamos haciendo es recurrir a un pipeline dentro de otro pipeline (el propio de Asp.Net). Pero tengamos en cuenta que podemos perfectamente recurrir a mediator en otros ámbitos, como parte de un procesador de mensajes de una cola. Esto tendría esta pinta:

> Queue Subscriptor -> Message processor -> Mediatr -> Caso de uso -> Entidad

Personalmente, me resulta muy útil imaginar dos escenarios distintos desde la perspectiva del mecanismo de entrega, así me ayuda a evaluar si estoy poniendo las cosas en el lugar que les corresponde.

## Mira que te haces de rogar... ¿pero dónde pongo las validaciones?

Después de todo este peñazo intentando aportar un poco de contexto, la respuesta sigue siendo DEPENDE. Recordemos las preguntas, partiendo de la base de que usamos validaciones con Mediatr.

### ¿Pasamos de poner validaciones en el controlador?

En el controlador pongo las validaciones relativas al mecanismo de entrega. Por ejemplo, la autenticación es un tipo de validación y a Mediatr esa comprobación ya debe llegarle realizada.

Aunque ese es un ejemplo, normalmente pondriámos pocas validaciones en el controlador, ya que la mayoría de las validaciones se suelen dar en el ámbito de la aplicación, es decir, en el de los casos de uso.

Otra cosa es que me digas que estás utilizando el framework de turno, que te pone muy fácil validar según que datos llegún te llegan en el controlador y que te interesa poner ciertas validaciones ahí. Pues bueno, ahí ya entramos en los trade-offs de siempre. Lo importante es siempre que tengas en cuenta que tu caso de uso no tiene porqué ser llamado siempre a través de un controlador, recuerda el ejemplo que ponía más arriba recurriendo a una cola de mensajes.

Personalmente, esto último es algo que prefiero no hacer. Estoy seguro de que, puestos a comprar magia, puedes comprarla en la capa correcta de tu arquitectura y al menos estás respetando las fronteras.

### ¿Las duplicamos?

No, a no ser que sea por motivos de UX y de forma muy excepcional (ver explicación más abajo)

### ¿Validamos cosas diferentes?

Esa es la clave. Lo normal es que validemos cosas diferentes y por eso las pongamos en una parte u otra.

Siguiendo con el ejemplo anterior, la autorización es algo que normalmente haremos en capa de aplicación. Si es relevante, a nuestro caso de uso nos llegará la información del usuario que está autenticado y ahí podremos realizar las verificaciones de seguridad que consideremos adecuadas.

Otro ejemplo típico se da cuando tenemos que recurrir a validaciones que requieren interactuar con repositorios o similar. Este tipo de validaciones deben ir claramente en la capa de aplicación, por lo que entrarían dentro de las validaciones que realizamos en el ámbito de Mediatr.

### ¿Qué hacemos en las entidades?¿Las volvemos a poner?

Al final tenemos que volver a aplicar las mismas heurísticas. En las entidades debemos poner únicamente las validaciones que sean propias del negocio.

Por ejemplo, si el negocio no permite que se creen facturas sin líneas de factura, esta validación debería ir en la entidad correspondiente y no deberíamos redundarla en controladores o capa de aplicación.

Una vez más, la UX puede venir a destruir nuestros sueños perfectos.

## ¿Y qué pasa con la UX?

No debemos perder de vista que desarrollamos software para que sea utilizado por personas. A nuestros usuarios les importa un carajo si estamos recurriendo a una arquitectura hexagonal de libro o tenemos controladores del tamaño de un bombardero ruso.

Y por eso hay ocasiones en las que puede que tengamos que tirar todo lo descrito anteriormente a la basura sólo con el objetivo de proporcionar una mejor experiencia de usuario.

Aún así, cuando duplicamos validaciones en distintas capas de nuestra aplicación, estamos entrando en el maravilloso mundo del connascence de algoritmo. Cualquiera que lleve un tiempo en esto se ha enfrentado a las problemáticas que esto conlleva y, la verdad, tiende a ser peor que un dolor de muelas.

### Una opción respetuosa con la UX y la arquitectura

Si en nuestra aplicación vemos que tenemos que estar realizando muchas concesiones respecto a las validaciones por cuestiones de UX, deberíamos plantearnos si las validaciones son en si mismas casos de uso.

Dependiendo de cómo sea nuestro diseño, esto no sería muy difícil de conseguir y nos permitiría mantenernos respetuosos con la UX a la vez que respetamos nuestra aproximación de arquitectura. Sería algo así como lo que nos da el parámetro "-WhatIf" de PowerShell, pero aplicado a llamadas de API.

Esto, combinado con Ajax, Sockets o lo que sea que queramos/podamos utilizar, nos proporcionaría la UX deseada sin comprometer nuestra arquitectura.

## Conclusión

Piensa bien en las responsabilidades del área en el que estás metiendo validaciones para ver si estas encajan o estás meando fuera del tiesto.

Te has comido un post más largo que un día sin pan para decirte cuatro chorradas. Si no te ha aportado mucho, lo siento, seguramente esté oxidado en esto de escribir ;)

## Algunos enlaces

Imagen y post original sobre Clean Architecture: https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html

Mediatr, la librería que menciona Vicenç:  https://github.com/jbogard/MediatR

Un poco de magia de validaciones para .Net: https://github.com/FluentValidation/FluentValidation/

Foto de la cabecera: https://www.reddit.com/r/lotr/comments/alyty3/you_shall_not_pass_gandalf_confronting_the_balrog/
