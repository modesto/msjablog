---
title: '[Solucionado] Fluent Migrator no encuentra migraciones'
author: msanjuan
layout: post
date: 2015-01-19T01:23:05+00:00
url: /fluent-migrator-no-encuentra-migraciones/
dsq_thread_id:
  - 3433259413
categories:
  - .Net
tags:
  - FluentMigrator

---
Hace unos meses empecé a trabajar en un proyecto en el que utilizamos [Fluent Migrator][1]. Todo funciona bien el proyecto tiene bastantes migraciones funcionando sin problema. En líneas generales funciona casi igual que las migraciones de EF, con algún detalle distinto.

El caso es que hoy he empezado un proyecto nuevo y he añadido la referencia al paquete de Fluent Migrator para empezar a utilizarlo. Cuando he lanzado las migraciones parecía que todo estaba bien (no daba ningún error), pero no me encontraba ninguna de las migraciones que había definido en el ensamblado de migraciones.

Después de darle muchas vueltas buscando el problema, me he dado cuenta de que en el proyecto nuevo la versión de Fluent Migrator era la 1.4. Buscando un poco he encontrado una [incidencia][2] en la que describían lo mismo que me pasaba a mi, pero no especificaban la solución. Tirando del hilo me he dado cuenta también de la versión del &#8220;migrate.exe&#8221; que estaba ejecutando era la correspondiente a las Fluent Migrator Tools 1.3, porque tenía añadida la ruta en el PATH.

Al quitar la ruta del path y utilizar el &#8220;migrate.exe&#8221; de la versión 1.4, todo ha funcionado correctamente.

 [1]: https://github.com/schambers/fluentmigrator
 [2]: https://github.com/schambers/fluentmigrator/issues/567