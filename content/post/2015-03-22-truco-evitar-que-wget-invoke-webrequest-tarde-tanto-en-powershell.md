---
title: '[PowerShell] Evitar que wget (Invoke-WebRequest) tarde tanto'
author: msanjuan
layout: post
date: 2015-03-22T19:18:44+00:00
url: /truco-evitar-que-wget-invoke-webrequest-tarde-tanto-en-powershell/
dsq_thread_id:
  - 3617615535
categories:
  - PowerShell
  - Trucos

---
Mientras estaba preparando unos scripts para automatizar el proceso de sideloading de una aplicación Windows 8.1, me encontré con un problema bastante tonto. Estaba descargando por HTTP el paquete de la aplicación desde una red local pero el comando wget de PowerShell tardaba demasiado. Tenía claro que no era problema de la red porque el archivo bajaba a buena velocidad por otros medios, pero siempre que usaba wget el comando tardaba demasiado tiempo. Una descarga de menos de 2 segundos se convertía en una descarga de más de 10.

Después de dedicar un rato descubrí que el problema estaba en el indicador de progreso de descarga del paquete. Lo único que tenía que hacer era desactivarla y el rendimiento pasaba a ser el esperado. Para hacer esto únicamente es necesario añadir esta línea antes de la llamada al comando wget:

<pre class="lang:ps decode:true">$ProgressPreference='SilentlyContinue'</pre>

Si quisiera volver a mostrar el indicador de progreso para los siguientes comandos, únicamente tendría que volver a establecer su valor por defecto:

<pre class="lang:ps decode:true ">$progressPreference = 'Continue'</pre>

En fin, es una tontería que seguro que cualquiera con algo de callo con PowerShell conoce, pero mejor me lo apunto por aquí por si lo vuelvo a necesitar 😉