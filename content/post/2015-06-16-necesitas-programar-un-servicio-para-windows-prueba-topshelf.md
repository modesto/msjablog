---
title: ¿Necesitas programar un servicio para Windows? Prueba Topshelf
author: msanjuan
layout: post
date: 2015-06-15T23:44:42+00:00
url: /necesitas-programar-un-servicio-para-windows-prueba-topshelf/
dsq_thread_id:
  - 3851565380
categories:
  - Sin categoría

---
Programar un servicio para Windows suele ser una tarea engorrosa y <a href="http://topshelf-project.com/" target="_blank">Topshelf</a> proporciona una alternativa bastante interesante y muchísimo menos engorrosa que la plantilla por defecto que incorpora Visual Studio.

Aunque podría enumerar bastantes aspectos interesantes de Topshelf, me quedo con uno que para mi es fundamental, permite que una aplicación de consola sea un servicio de Windows. Esto significa que es posible recurrir a la aplicación de consola durante la fase de desarrollo o para su ejecución de forma independiente, pero que además es posible instalar esa aplicación de consola como un servicio Windows simplemente pasando un parámetro en su ejecución.

Utilizar Topshelf para crear un servicio de Windows es muy sencillo. Voy a describir los pasos básicos necesarios para dar un servicio, aunque luego podemos complicarlo bastante:

  1. Creamos una aplicación de consola e instalamos el paquete Topshelf
  2. Añadimos una clase que contenga la lógica del servicio que queremos crear. El esqueleto podría ser como este: <pre class="lang:c# decode:true" title="Código de ejemplo del servicio">public class ServiceRunner {
        public void Start() {
            // Código para ejecutar al iniciar el servicio
        }
        public void Stop() {
            // Código para ejecutar al parar el servicio
        }
}</pre>

  3. Modificamos la clase Main para configurar el servicio: <pre class="lang:c# decode:true">static void Main(string[] args) {
     HostFactory.Run(x =&gt;
     {
          x.Service&lt;ServiceRunner&gt;(sc =&gt;
          {
               sc.ConstructUsing(name =&gt; new ServiceRunner());
               sc.WhenStarted(rn =&gt; rn.Start());
               sc.WhenStopped(rn =&gt; rn.Stop());
          });
          x.RunAsLocalSystem();
          x.SetDescription("Descripción del servicio");
          x.SetDisplayName("DisplayName");
          x.SetServiceName("Nombre del servicio");
     });
}</pre>

&nbsp;

En la <a href="http://docs.topshelf-project.com/en/latest/" target="_blank">documentación</a> es posible encontrar muchos más detalles. Por ejemplo, además de poder configurar el código a ejecutar en el inicio (WhenStarted) o en la parada (WhenStopped), podemos configurar el comportamiento para la pausa (WhenPaused), continuación (WhenContinued) y el apagado (WhenShutdown).

Una vez hecho esto tenemos una aplicación de consola que podemos ejecutar de forma individual, depurar desde Visual Studio o instalarla como servicio y Topshelf se encargará de ejecutar el código correspondiente al inicio del servicio. Para instalar el servicio sólo es necesario ejecutar la aplicación de consola con el parámetro &#8220;install&#8221; y para desinstalarla con el parámetro &#8220;uninstall&#8221;.

Muy importante a tener en cuenta es el usuario con el que se va a ejecutar el servicio. Por ejemplo, si en el servicio vamos a utilizar Owin para levantar una Web o un API, este código de ejemplo no funcionará porque si el servicio arranca como LocalSystem no tendrá los permisos adecuados.

Es posible pasar otros parámetros para configurar el comportamiento deseado para el servicio en lo que a modo de inicio, usuario de ejecución, ejecución de comandos después de su instalación, etc. Lo mejor es ver la <a href="http://docs.topshelf-project.com/en/latest/" target="_blank">documentación</a> para estar al tanto de todas las opciones.