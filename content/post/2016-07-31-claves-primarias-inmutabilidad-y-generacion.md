---
title: 'Claves primarias: inmutabilidad y generación'
author: msanjuan
layout: post
date: 2016-07-30T23:39:11+00:00
url: /claves-primarias-inmutabilidad-y-generacion/
dsq_thread_id:
  - 5027626506
categories:
  - Sin categoría

---
Hace unos días [Pablo Iglesias][1] [tuiteó][1] una pregunta de Stack Exchange en la que hablaban sobre la inmutabilidad de las claves primarias. Se generó un debate bastante interesante en Twitter que se extendió más allá de la cuestión de la inmutabilidad. Este es un intento de recopilar y explicar algunos de los conceptos que se mencionaron durante ese debate.

# Claves primarias mutables o inmutables

Para el que ande un poco perdido con el concepto, básicamente se trata de determinar si la clave primaria de una entidad debería poder cambiar o no una vez establecida. Por ejemplo: si me registro en un servicio online y utilizan mi email como PK, si el servicio me da la opción de cambiar mi cuenta de correo, la PK es mutable. Esto significa que en todos los sitios en los que el servicio se esté persistiendo mi email como PK va a tener que actualizar el dato para constatar el cambio. Independientemente de que usemos o no foreign keys, esto es un foco de dolor, especialmente si utilizamos distintos mecanismos de persistencia de datos. Si además nuestro software cuenta con funcionalidades como con auditar los cambios o cualquier tipo de integración con sistemas externos como herramientas de CRM y similar, tener PKs mutables se puede convertir en algo imposible.

Parece que hubo un consenso generalizado respecto a que las PK deberían ser inmutables, así que no me voy a extender más en este punto.

# Claves subrogadas o naturales

Cuando usamos una clave natural, el valor de la clave está relacionado con los datos que identifica. Por ejemplo, si estamos identificando libros, el ISBN podría ser un candidato a PK. El número de bastidor para un coche, número de serie de un ordenador, la MAC de una tarjeta de red, el número de una factura, etc. En resumen, utilizamos un dato del negocio para identificar nuestras entidades al persistirlas.

Si, por otro lado, elegimos usar una clave subrogada, lo que estamos haciendo es generar un identificador único que no tiene nada que ver con los datos que identifica, pero se garantiza a nivel infraestructura que el dato es único.

Aunque siempre pongo un gran &#8220;depende&#8221; y no soy muy amigo de las afirmaciones absolutas, la realidad es que llevo años utilizando claves subrogadas porque la experiencia me ha demostrado que suele ser una mala idea utilizar datos del negocio. La razón es muy sencilla: el negocio cambia.

Ejemplos hay miles. Por tomar uno, [Sergio León][2] mencionaba los ISBN de los libros. Desarrollando un software para gestionar liberías podríamos pensar que el ISBN debería valernos, pero los libros muy viejos no tienen ISBN. Así que si de repente al librero le diese por dedicarse a vender libros antiguos de colección, tendríamos un problema importante. Ese tipo de situaciones se dan con mucha frecuencia y siempre cuando el desarrollo ya está bastante avanzado, que es cuando más daño hacen.

Como el ejemplo del ISBN hay muchos y debemos tener en cuenta el coste del cambio. Hay quién podría utilizar el argumento de que es un error pensar en el futuro y que usar una clave subrogada es una decisión prematura basada en anticiparse a un posible cambio de negocio que no sabemos si alguna vez va a suceder. En esas circunstancias suele ser un buen ejercicio pensar en cuál sería el coste del cambio en el futuro frente al coste de usar una clave subrogada en el presente. Si lo pensamos en términos económicos, utilizar una clave natural podría implicar estar firmando una hipoteca muy cara.

Además, como indicaba [Pedro J. Molina][3], &#8220;Si vas por clave natural acaba uno fácilmente con claves compuestas. Las de 4 campos ya no hacen gracia&#8221;. Pero es que encima eso termina tocando mucho las narices cuando llega el momento de &#8220;enchufarte&#8221; a otras cosas. Es típico que el servicio que te permite asociar comentarios, likes, etc. te pida que le des un identificador único de tu entidad y nos podríamos volver locos buscando ejemplos que nos llevan a desear trabajar siempre con una clave subrogada.

## Generación de las claves subrogadas

Venga, vamos a suponer que a estas alturas ya estamos convencidos de que vamos a identificar a nuestras entidades con claves inmutables y subrogadas al menos algunas veces. ¿Cómo las generamos? Este punto también generó debate y en este caso no había unanimidad ni de lejos.

Para generar una clave subrogada podemos optar por dejar que la base de datos la genere por nosotros automáticamente cuando insertamos el registro o podemos ser nosotros desde el código los que proporcionemos la clave a la base de datos.

La opción clásica es que la base de datos genere el identificador por ti y listo. Podemos usar un identity, secuencia o lo que nos proporcione la base de datos que utilicemos y no nos complicamos mucho la vida. Pero hay escenarios en los que nos podría interesar recurrir a otro tipo de mecanismos.

## Mecanismos para la generación de claves subrogadas

Aunque podríamos ponernos muy creativos de cara a la generación de nuestras claves, hay ciertos mecanismos predominantes. No voy a posicionarme respecto a ninguno porque creo que su utilización depende mucho de los requisitos del negocio y de la infraestructura de la aplicación.

### Guids

Generar Guids es algo sencillo, se puede hacer desde cualquier sitio y las posibilidades de colisión son absolutamente despreciables. En principio es un mecanismo interesante ya que nos permite tener el control desde el negocio y no depender de la base de datos para su generación. Además, entre sus virtudes podemos considerar que al usar un Guid estamos utilizando un identificador no sólo a la tabla, también entre todas las tablas e incluso entre varias bases de datos. Esto habilita con mucha más facilidad escenarios de &#8220;merge&#8221; en los que queremos juntar datos que provienen de varias fuentes.

Por supuesto, también tiene sus contras, entre ellos que un Guid ocupa muchísimo más que una clave numérica y además [afecta negativamente][4] al rendimiento de los índices clustered.

Hay mucha literatura sobre este tema en internet, incluyendo algunos intentos de análisis sobre la diferencia desde el punto de vista de rendimiento. Además debemos tener en cuenta que la implementación de base de datos que utilicemos influye muchísimo. Como ejemplo, no es lo mismo usar [MySQL][5] que [SQL Server][6].

### Numérico autoincrementado

Este mecanismo es uno de los más frecuentes y creo que no necesita mucha explicación. El standard SQL:2003 define los tipos IDENTITY y SEQUENCE pero dependemos de la base de datos que estemos utilizando. Por ejemplo, en MySQL está el AUTO_INCREMENT y en PostgreSQL también está el SERIAL (creo que es &#8220;azucar sintáctico&#8221; para definir un SEQUENCE).

### HiLo

A grandes rasos, con HiLo la aplicación reserva con antelación en la base de datos un rango de identificadores que serán utilizados necesite insertar nuevos registros en la base de datos. Cuando la aplicación ha usado todos los identificadores, va a la base de datos y reserva más. NHibernate usa esta técnica y también es posible usarla en Entity Framework. Entre sus ventajas está que nos permite utilizar identificadores de tipo numérico (para los que demandan rendimiento) y entre las desventajas que genera &#8220;lagunas&#8221; entre los identificadores generados. Si una aplicación reserva los identificadores del 1 al 10 y luego usa únicamente 5, los identificadores de 6 al 10 quedarán sin usar.

### Dependiendo de un tercero

Consiste en delegar en un tercero la estrategia de generación de nuestra clave. Desde nuestra aplicación, cuando queramos identificar una entidad, llamaremos a este servicio externo y nos proporcionará una identidad que será la que luego persistiremos.

Cuando estamos haciendo HiLo, en parte estamos utilizando este mecanismo. Aunque sea nuestra base de datos la que genera los identificadores, en realidad es un tercero (la clase que gestiona el algoritmo) el que nos los está proporcionando.

Un ejemplo (raro pero real) es utilizarlo cuando la persistencia se realiza sobre mecanismos que no tienen soporte de autonuméricos, delegando la generación del autonumérico sobre un tercero para luego persistir el dato. Otro escenario de uso sería si necesitamos asegurarnos de que nuestros identificadores son únicos entre varios sistemas y además queremos aplicar reglas especiales para su generación.

Siendo realistas, es un mecanismo que añade complejidad y no es necesario la mayoría de las veces, pero no deja de ser interesante mencionarlo.

## ¿Cuál uso?

Según las respuestas a la conversación que originó el debate, una gran mayoría respondería &#8220;secuencias numéricas autoincrementadas y no me complico&#8221;. No les falta razón, pero hay veces que complicarnos es la opción menos complicada. Complicado, eh? 😉

Existen muchas razones que podrían llevarnos a querer complicarnos la vida y no pretendo hacer un estudio detallado, pero al menos quiero poner algunos ejempos comunes.

### CQS

Si queremos usar [CQS][7] (Command-Query Separation) partimos del principio de que nuestros comandos no devuelven nada. Si tenemos un comando que inserta una entidad, deberíamos saber el id de la entidad que estamos insertando antes de llamar al propio comando, ya que el comando no va a devolvernos nada.

Podríamos pensar que tampoco pasa nada porque un comando de inserción devuelva un valor y ya está. No voy a entrar a valorar esto, pero en cuanto queramos que el modelo de ejecución de nuestros comandos sea asíncrono (a través de una cola o similar), volvemos al problema inicial.

### Modo offline

Si nuestra aplicación tiene como requisito de negocio proporcionar la posibilidad trabajar offline, recurrir al servidor de base de datos para obtener el id de una entidad no es una opción.

Imaginemos una aplicación como Evernote. Cuando creemos una nueva nota y no tengamos conexión, ¿qué identificador tendrá esa nota entonces? Si generamos ese identificador desde el código (mediante, por ejemplo, un GUID) podremos realizar la sincronización fácilmente una vez tengamos conexión.

Aunque en el caso de CQS, HiLo podría ser una opción perfectamente válida, en este caso sería la opción menos recomendada ya que tendríamos la necesidad de conectarnos al servidor de base de datos si creamos tantas entidades que agotamos nuestro &#8220;cupo&#8221; de identificadores.

### Si no usamos SQL

Este caso es ignorado por muchos desarrolladores cuyo contexto profesional se limita a bases de datos SQL para persistir información. Cuando salimos del mundo SQL, muchísimos mecanismos de persistencia piden que seamos nosotros desde el código los que proporcionemos nuestros identificadores. Como ejemplo &#8220;primitivo&#8221; a la par que útil, si persistimos en disco tendremos que proporcionar nosotros el identificador de nuestra entidad (nombre del archivo).

# Conclusiones

Tenemos multitud de mecanismos para identificar nuestras entidades. Debemos conocerlos bien y hacer un uso inteligente de ellos en función de nuestros requisitos de negocio, pero nunca complicarnos la vida innecesariamente.Y muy importante, que el rendimiento únicamente sea un argumento cuando tenemos requisitos no funcionales que nos hablan de rendimiento. Hablar de rendimiento cuando no tienes requisitos relacionados con el rendimiento suele ser foco de optimizaciones prematuras.

Y con esto creo que termino el &#8220;resumen&#8221; de las cosas que se comentaron en el hilo de Twitter y algunas de las que pasaron por mi cabeza. Seguro que me dejo en el tintero muchas observaciones y muchos matices, me conformo si a alguien le vale para entender que hay vida más allá de los identificadores mutables naturales 😀

 [1]: https://twitter.com/Pablyte/status/754056720666730500
 [2]: https://twitter.com/panicoenlaxbox
 [3]: https://twitter.com/pmolinam
 [4]: http://sqlmag.com/database-performance-tuning/clustered-indexes-based-upon-guids
 [5]: http://krow.livejournal.com/497839.html
 [6]: http://www.informit.com/articles/printerfriendly/25862
 [7]: http://martinfowler.com/bliki/CommandQuerySeparation.html