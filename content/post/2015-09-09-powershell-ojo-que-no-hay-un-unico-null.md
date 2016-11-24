---
title: '[PowerShell] Ojo que no hay un único $null'
author: msanjuan
layout: post
date: 2015-09-09T20:49:56+00:00
url: /powershell-ojo-que-no-hay-un-unico-null/
dsq_thread_id:
  - 4114132658
categories:
  - PowerShell

---
Este es un ejemplo curioso de esos que me ha vuelto loco hasta que me he dado cuenta. Si parto de este código:

<pre class="lang:ps decode:true">$item1 = Get-ChildItem | Where-Object {$_ -eq "no encuentras esto ni de coña"}
"item1 es nulo? {0}" -f ($null -eq $item1)
$item2 = $null
"item2 es nulo? {0}" -f ($null -eq $item2)</pre>

Al ejecutarlo el resultado es &#8220;True&#8221; en ambos casos. Vamos, que tanto _item1_ como _item2_ son _$null_. Hasta aquí todo bien. Ahora voy a ejecutar este código después del anterior:

<pre class="lang:default decode:true">$lista1 = @($item1)
"La lista 1 tiene una longitud de {0}" -f $lista1.Length
$lista2 = @($item2)
"La lista 2 tiene una longitud de {0}" -f $lista2.Length</pre>

En principio el resultado debería ser el mismo en ambos casos&#8230; o no. Al ejecutar este código el resultado será 0 para _lista1_ y 1 para _lista2_. Toma ya!

En caso de que sea necesario distinguir de qué tipo de _$null_ se trata en cada caso, este código puede servir para salir de dudas:

<pre class="lang:ps decode:true">$item1.PSObject -eq $null
$item2.PSObject -eq $null</pre>

En este caso, para _item1_ obtendremos un _false_ y para _item2_ un _true_.

En fin, aquí queda otro [WAT][1] de PowerShell de esos que hacen que la vida sea más divertida 😉

 [1]: https://www.destroyallsoftware.com/talks/wat