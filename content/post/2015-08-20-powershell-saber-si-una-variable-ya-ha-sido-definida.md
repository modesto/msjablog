---
title: '[PowerShell] Saber si una variable ya ha sido definida'
author: msanjuan
layout: post
date: 2015-08-20T22:14:23+00:00
url: /powershell-saber-si-una-variable-ya-ha-sido-definida/
dsq_thread_id:
  - 4051770317
categories:
  - PowerShell

---
En ocasiones tengo la necesidad de saber si una variable ha sido previamente definida. Esto es especialmente importante porque utilizo siempre el &#8220;Set-StrictMode -Version 2&#8221; y, si necesito recurrir a variables de otros scopes, cuando intento acceder a una variable no definida obtengo un error. Aunque generalmente intento no recurrir a otros scopes, especialmente el _$global_, en ocasiones los necesito, especialmente el _$script_.

Un ejemplo podría ser querer verificar si una variable que contiene un array ha sido inicializada o no, para poder añadirle ítems. El código quedaría de la siguiente forma:

<pre class="lang:ps decode:true">Function Add-ItemToScriptVariable($item) {
	if (-not (Test-Path variable:script:anyVariable)) {
		[System.Collections.ArrayList]$script:anyVariable = @()
	}
    $script:anyVariable += $item
}

Add-ItemToScriptVariable "item 1"
Add-ItemToScriptVariable "item 2"
Add-ItemToScriptVariable "item 3"

Write-Host $script:anyVariable</pre>

Como es costumbre en PowerShell, un mismo comando vale aparentemente para múltiples propósitos. En este caso, _**Test-Path**_ nos permite verificar si una variable existe en un ámbito determinado. Aunque choque, es importante fijarse en que el habitual símbolo del dolar no aparece por ningún lado en este caso.

Por si alguien se lo pregunta, al igual que el _**Test-Path**_ funciona, también es posible utilizar el comando _**&#8220;Get-ChildItem variable:&#8221;**_. No obstante, si el objetivo es enumerar las variables, es mucho más apropiado utilizar el comando _**Get-Variable**_.