---
title: 'Eliminar regiones (#region) de forma masiva desde Visual Studio'
author: msanjuan
layout: post
date: 2015-06-16T10:58:45+00:00
url: /eliminar-regiones-de-forma-masiva-desde-visual-studio/
dsq_thread_id:
  - 3852710305
categories:
  - Sin categoría

---
No voy a entrar en el debate de si el uso de #regions es una buena práctica o no, pero es algo que no utilizo y que suelo interpretar como un mal olor cuando me lo encuentro en el código. Me he animado a escribir este pequeño apune porque recientemente me he encontrado con la necesidad de eliminar de un proyecto casi 25.000 regiones (si, veinticinco mil) y quiero tener esto a mano para el futuro.

El truco es muy sencillo, necesitamos recurrir a la funcionalidad de buscar y reemplazar de Visual Studio y marcar el check para que utilice expresiones regulares en la búsqueda.

[<img class="alignnone size-full wp-image-104" src="http://www.modestosanjuan.com/wp-content/uploads/2015/06/search-replace-vs.png" alt="Buscar y reemplazar" width="385" height="504" srcset="http://www.modestosanjuan.com/wp-content/uploads/2015/06/search-replace-vs.png 385w, http://www.modestosanjuan.com/wp-content/uploads/2015/06/search-replace-vs-229x300.png 229w" sizes="(max-width: 385px) 100vw, 385px" />][1]

&nbsp;

Una vez hecho esto, debemos realizar dos pasos, el primero para los #region y el segundo para los #endregion. Estas son las dos expresiones regulares que debemos utilizar si el código está escrito el C#:

&nbsp;

<pre class="lang:default decode:true">\#region .*\n

Si es C#
\#endregion .*\n

Si es VB.Net
\#end region .*\n


</pre>

Cuando lo hago especifico además que únicamente aplique este cambio para ficheros &#8220;.vb&#8221; o &#8220;.cs&#8221; según corresponda.

 [1]: http://www.modestosanjuan.com/wp-content/uploads/2015/06/search-replace-vs.png