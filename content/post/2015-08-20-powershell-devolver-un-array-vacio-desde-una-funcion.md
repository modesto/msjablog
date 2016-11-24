---
title: '[PowerShell] Devolver un array vacío desde una función'
author: msanjuan
layout: post
date: 2015-08-20T15:07:23+00:00
url: /powershell-devolver-un-array-vacio-desde-una-funcion/
dsq_thread_id:
  - 4050665931
categories:
  - PowerShell

---
Esto es un truco rápido muy chorra pero que puede volver loco a más de un desarrollador que no esté acostumbrado a las peculiaridades de PowerShell. Tomando este código como ejemplo:

<pre class="lang:ps decode:true">function Get-EmptyArray {
 [System.Collections.ArrayList]$anyArray = @()
 return $anyArray 
}

Write-Host (Get-EmptyArray).GetType()</pre>

En principio podría resultar bastante evidente que el resultado de la ejecución será ver por pantalla la cadena &#8220;System.Collections.Arraylist&#8221;. Pues no, el resultado será un error como una casa del estilo de &#8220;<span style="color: #ff0000;">You cannot call a method on a null-valued expression</span>&#8221;

La explicación de este funcionamiento tiene que ver con la forma que tiene PowerShell de gestionar qué se retorna en una función y creo que da para otro minipost. Como el objetivo en este caso es saber la solución, el truco es sencillo:

<pre class="lang:ps decode:true ">function Get-EmptyArray {
 [System.Collections.ArrayList]$anyArray = @()
 return ,$anyArray 
}

Write-Host (Get-EmptyArray).GetType()</pre>

Por si la diferencia no se aprecia, el truco es poner una coma delante de la variable _$anyArray_ cuando se hace el return. Fácil, sencillo y feo, pero funciona 😉