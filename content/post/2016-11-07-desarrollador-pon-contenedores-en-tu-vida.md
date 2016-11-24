---
title: Desarrollador, pon contenedores en tu vida
author: msanjuan
layout: post
date: 2016-11-06T23:10:11+00:00
url: /desarrollador-pon-contenedores-en-tu-vida/
dsq_thread_id:
  - 5284090553
categories:
  - Sin categoría

---
No, en este post no te voy a vender las virtudes de desplegar su aplicación sobre docker. Mucho se ha escrito sobre el tema y mucho se seguirá escribiendo. También se ha escrito de [lo malo que es][1] docker y de lo [no tan malo que es][2].

Este post está destinado especialmente a todos aquellos desarrolladores que no desplegáis vuestras aplicaciones con Docker. Porque los que ya usáis docker en el ciclo de entrega ya sabéis lo que voy a contar y además os parece obvio.

Seguro que alguna vez has tenido que instalarte Redis, Mongo, Couchbase, MySql, ElasticSearch, RabbitMq o cualquier otro software similar para hacer una prueba puntual, por requisitos de un proyecto fugaz o para hacer un spike y decidir si finalmente introduces esa pieza o no en tu infraestructura.

Lo normal antes de tener contenedores era instalar el software en cuestión en nuestra máquina o tal vez recurrir a una máquina virtual. Lo de la máquina virtual era práctica habitual cuando sabíamos que el software a instalar iba a causar estragos en nuestra máquina, ya fuera en forma de mierda repartida por todo el sistema o en forma de losa de 500kg para el rendimiento.

Pues ahora tenemos una opción más, con las ventajas de aislamiento de las máquinas virtuales pero muchísimo más sencillo. Gracias al [hub de docker][3] tenemos al alcance de un &#8220;docker run&#8221; imágenes oficiales de todos los programas mencionados antes y muchos más. Incluso tenemos algunas no oficiales que pueden ser de mucha ayuda, como Oracle. Si, Oracle, porque quién narices quiere instalarse Oracle para hacer 4 pruebas pudiendo tirar de una imagen preconfigurada!.

Si las imágenes están curradas (suelen estarlo), podrás incluso personalizar aspectos del funcionamiento de la aplicación. Por ejemplo, la [imagen de Redis][4] está preparada para poder usar tu propio &#8220;redis.conf&#8221; o simplemente para usar redis con contraseña.

Por cierto, aunque las imágenes del [repositorio de Microsoft][5] aún no se consideran oficiales dentro del hub de Docker, son las oficiales de Microsoft, son de fiar 😉

Tal como lo he mencionado parece que esto de usar Docker durante el desarrollo es algo que sólo vale para hacer pruebas. Pues no, puedes usarlo en tu día a día y así evitar contaminar tu sistema con instalaciones que luego tienes que actualizar cada cierto tiempo, acumulando cada vez más y más &#8220;restos&#8221;.

Un último briconsejo: Si tienes la oportunidad, intenta usar las versiones tageadas como &#8220;alpine&#8221;. Evidentemente, las versiones &#8220;alpine&#8221; están basadas en [Alpine Linux][6], una distribución &#8220;security-oriented&#8221; que pesa sólo 5Mb. En la mayoría de los casos, teniendo en cuenta que lo que queremos probar es únicamente el software de la imagen que hemos bajado, será suficiente. Como referencia, la imagen &#8220;latest&#8221; de Redis ocupa 74Mb y la &#8220;alpine&#8221; ocupa sólo 8Mb.

 [1]: https://thehftguy.wordpress.com/2016/11/01/docker-in-production-an-history-of-failure/
 [2]: http://patrobinson.github.io/2016/11/05/docker-in-production/
 [3]: https://hub.docker.com/
 [4]: https://hub.docker.com/_/redis/
 [5]: https://hub.docker.com/r/microsoft/
 [6]: https://alpinelinux.org/