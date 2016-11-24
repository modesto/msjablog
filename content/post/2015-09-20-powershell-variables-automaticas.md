---
title: '[PowerShell] Variables automáticas'
author: msanjuan
layout: post
date: 2015-09-20T22:10:01+00:00
url: /powershell-variables-automaticas/
dsq_thread_id:
  - 4148945148
categories:
  - PowerShell

---
PowerShell dispone de una serie de variables gestionadas de forma automática que permiten obtener información relacionada con la ejecución de código actual. Su propósito y utilidad es variado, siendo algunas de uso muy frecuente y otras de uso marginal. En cualquier caso, no está mal conocer unas cuantas y que suene dentro de la cabeza que hay otras que tal vez sea útiles en el futuro.

Tanto la <a href="https://technet.microsoft.com/en-us/library/hh847768.aspx" target="_blank">documentación oficial</a> como el comando _&#8220;<span style="color: #3366ff;">get-help</span> <span style="color: #cc99ff;">about_automatic_variables</span>&#8220;_ contienen el listado completo, pero me gustaría enumerar por aquí algunas que me resultan interesantes o que tienen truco.

Algunas variables son de sobra conocidas para cualquiera que conozca medianamente PowerShell. Variables como **$_, $true, $false, $Args, $NULL** son de uso común incluso antes de saber siquiera qué es eso de las variables automáticas.

Relacionadas con la gestión de errores, las variables **$?** y **$Error** podrían ser útiles, aunque hay que tratarlas con cuidado. En el primer caso, **$?** devuelve TRUE si la última operación se realizó correctamente o FALSE en caso contrario. La variable **$Error** en principio se trata de un array que contiene los errores más recientes. El problema que le veo a este array es que únicamente contiene los errores de PowerShell. Es decir, tomando este código como ejemplo:

<pre class="lang:ps decode:true">ping error_forzado
$?
$Error</pre>

En un caso como este, la variable **$?** será _False_ pero **$Error** no contendrá el error que ha dado el comando _ping_. La razón es que no se trata de un error de PowerShell. Cuando hacemos _ping_ estamos invocando un ejecutable, no una función o cmdlet de PowerShell. Por eso hay que tener cuidado y normalmente es más conveniente capturar los errores mediante otros mecanismos.

Relacionado también la captura de errores está la variable **$LastExitCode**, que almacena el código de salida del último programa de windows ejecutado. en el ejemplo anterior, **$LastExitCode** tendría un valor de 1.

Otra variable muy interesante es **$MyInvocation**. Esta variable proporciona información sobre el comando que se está ejecutando actualmente, incluyendo el nombre del script, la función, la ruta, los parámetros, etc. Por ejemplo, las propiedades _BoundParameters_ y _UnboundArguments_ permiten acceder al listado de argumentos pasados a la función. _UnboundArguments_ es algo así como **$Args**, aunque el tipo de **$Args** es Object[] y _UnboundArguments_  es de tipo List<Object>.

Otra variable interesante, relacionada con la anterior es **$PSBoundParameters**. En este caso la explicación es sencilla, llamar a esta variable es lo mismo que llamar a **$MyInvocation.BoundParameters**. Al menos así es en mi máquina, en la que _$PSBoundParameters.Equals($MyInvocation.BoundParameters)_ devuelve _True_. Y recurro al clásico &#8220;en mi máquina funciona&#8221; porque me he encontrado por <a href="https://gist.github.com/anderssonjohan/4738733" target="_blank">ahí</a> quien dice lo contrario en función de la versión de PowerShell que se ejecute.

Y por último, dos que dan mucho juego: **$PSCommmandPath** y **$PSScriptRoot**. Estas dos variables proporcionan respectivamente la ruta completa y el directorio del script desde el que se está ejecutando el código. Estas variables pueden resultar muy útiles para hacer referencias a rutas relativas a un script en concreto, independientemente del cual sea el valor de _Get-Location_. Eso si, es importante tener en cuenta que son de tipo string, si queremos manipularlas lo más conveniente suele ser crear variables de tipo _DirectoryInfo_ o _FileInfo_ a partir de sus valores.