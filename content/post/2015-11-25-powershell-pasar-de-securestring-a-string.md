---
title: '[PowerShell] Pasar de SecureString a String'
author: msanjuan
layout: post
date: 2015-11-25T09:16:48+00:00
url: /powershell-pasar-de-securestring-a-string/
dsq_thread_id:
  - 4443938204
categories:
  - PowerShell
  - Sin categoría

---
En ocasiones necesito usar un **SecureString** pero luego me veo obligado a utilizar ese string de forma no segura. Aunque la recomendación es no hacer esto, la realidad es que hay muchos escenarios en los que es necesario. Un ejemplo sencillo es el caso en el que necesito que el usuario meta un dato sensible por consola y pero luego necesito utilizar ese dato como texto plano para pasarlo como parámetro a un ejecutable de Windows. Este es un ejemplo de cómo hacerlo:

<pre class="lang:ps decode:true">Param(
   [Parameter(Mandatory=$True)]
   [SecureString]$password
)

function Decrypt-SecureString($secureString) {
    $BSTR = [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($password)
    return [System.Runtime.InteropServices.Marshal]::PtrToStringAuto($BSTR)
}

$nonSecure = Decrypt-SecureString $password</pre>

&nbsp;