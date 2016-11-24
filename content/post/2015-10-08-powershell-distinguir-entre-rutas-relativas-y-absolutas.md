---
title: '[PowerShell] Distinguir entre rutas relativas y absolutas'
author: msanjuan
layout: post
date: 2015-10-08T22:41:43+00:00
url: /powershell-distinguir-entre-rutas-relativas-y-absolutas/
dsq_thread_id:
  - 4207381150
categories:
  - PowerShell

---
Esta vez toca un truco muy simple pero muy útil cuando tengo un script que recibe una ruta como parámetro. Es muy típico no saber si la ruta es absoluta o relativa y muy frecuente querer hacer un **Join-Path** con la ruta actual (o cualquier otra) en caso de que la ruta sea relativa.

En ocasiones me olvido de que PowerShell tiene a su disposición toda la potencia de .Net y en este caso es precisamente útil. Utilizando el método **IsPathRooted** de **System.IO.Path** puedo saber si la ruta es absoluta o relativa y actuar en consecuencia. Este sería un ejemplo típico de estos casos.

<pre class="lang:ps decode:true">param([string]$Path) 

$targetPath = pwd

if (-not [string]::IsNullOrWhiteSpace($Path)) { 
	if ([System.IO.Path]::IsPathRooted($Path)) {
		$targetPath = $Path
	} else{
		$targetPath = Join-Path (pwd) $Path
	}
}</pre>

De regalo este script trae otro ejemplo en el que recurrir a .Net es muy útil, el método **IsNullOrWhiteSpace****()**