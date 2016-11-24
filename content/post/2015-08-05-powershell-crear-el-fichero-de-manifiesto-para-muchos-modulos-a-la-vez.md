---
title: '[PowerShell] Crear el fichero de manifiesto para muchos módulos a la vez'
author: msanjuan
layout: post
date: 2015-08-05T13:50:27+00:00
url: /powershell-crear-el-fichero-de-manifiesto-para-muchos-modulos-a-la-vez/
dsq_thread_id:
  - 4006411077
categories:
  - PowerShell
format: aside

---
Hoy he tenido que crear de golpe el manifest a varios módulos. Teníamos más de 10 módulos (.psm1), cada uno en su propia carpeta y necesitaba añadirles el manifest para poder definir la empresa de cada módulo. Gracias al pipeline de PowerShell, no he tenido que esforzarme mucho para conseguirlo:

<pre class="lang:ps decode:true">ls *.psm1 -Recurse | ForEach-Object { 
                                        $manifestFileName = $_.BaseName + ".psd1"
                                        $manifestPath = Join-Path $_.Directory $manifestFileName
                                        $companyConstant = "AnyCompany"
                                        New-ModuleManifest -Path $manifestPath  -RootModule $_.Name -Author $companyConstant -CompanyName $companyConstant -Copyright $companyConstant -FileList @($_.Name)
                                        Write-Host $manifestPath
                                    }</pre>

Es un script tonto, sin mucho misterio, pero me lo dejo aquí apuntado para el futuro. La razón por la que necesitaba hacer esto era para luego poder descargar de golpe todos los módulos cargados de esta empresa. Para hacer esto también es bastante sencillo:

<pre class="lang:ps decode:true ">Get-module | Where-Object { $_.CompanyName -eq "AnyCompany" } | Remove-Module</pre>

&nbsp;

&nbsp;