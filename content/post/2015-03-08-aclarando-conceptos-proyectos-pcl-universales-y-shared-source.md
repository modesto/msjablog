---
title: 'Aclarando conceptos: proyectos PCL, universales y shared source'
author: msanjuan
layout: post
date: 2015-03-08T12:22:55+00:00
url: /aclarando-conceptos-proyectos-pcl-universales-y-shared-source/
dsq_thread_id:
  - 3607149313
categories:
  - .Net

---
Lo habitual cuando quiero compartir código entre proyectos es recurrir a crear bibliotecas de clases. Dependiendo del tipo de proyectos en los que quiera reutilizar mi trabajo, es necesario recurrir a bibliotecas portables (PCL) o incluso a proyectos de código compartido (shared source). Con bibliotecas PCL había trabajado anteriormente, pero no con proyectos universales y shared source, y creo que es importante tener claro cuales son las diferencias entre cada tipo, ya que juegan papeles muy diferentes.

Lo primero a tener en cuenta es que el objetivo de todos estos tipos de proyecto es sencillo: compartir código. Todos buscan que, cuando trabajemos con proyectos que se van a ejecutar en distintos _runtimes_, podamos compartir la mayor cantidad de código posible. En este caso, todo surgió simplemente por compartir un módulo entre una aplicación Windows 8.1 y una WPF, pero es posible encontrar escenarios más complejos que incluyan proyectos de Windows Phone o Xamarin.

Hay que partir de la base de que hace tiempo que no existe un único .Net framework. Antes sólo era necesario preocuparse de la versión de framework para saber si unas características estaban disponibles o no, ya que siempre tenía el mismo entorno de ejecución. Ahora, dependiendo del tipo de proyecto con el que esté trabajando, me encuentro con que, con la misma versión del framework, me encuentro con distintas capacidades en función de su plataforma de ejecución. Esto implica que no voy a tener disponibles las mismas capacidades en todos los sitios en los que intente reutilizar mis bibliotecas de clases.

**Bibliotecas PCL**

Al crea una biblioteca PCL, hay que especificar las versiones de .Net a las que está destinado su uso. Esta elección, modificable en cualquier momento, determina las características de .Net se van a poder explotar, así como las bibliotecas de terceros que se podrán utilizar.

Por ejemplo: si para utilizar _dynamics_, hay que tener en cuenta que la biblioteca podrá utilizarse desde aplicaciones .Net de escritorio y aplicaciones Windows 8 de la tienda de Windows, pero no podrá ser utilizada por aplicaciones para Windows Phone.

Si quieres información detallada sobre bibliotecas PCL y las funcionalidades disponibles en cada plataforma, la documentación oficial es <a title="Desarrollo multiplataforma con la Biblioteca de clases portable" href="https://msdn.microsoft.com/es-ES/library/gg597391(v=vs.110).aspx" target="_blank">esta</a>. Si quieres crear bibliotecas PCL compatibles con Xamarin, también proporcionan <a title="Introduction to Portable Class Libraries" href="http://developer.xamarin.com/guides/cross-platform/application_fundamentals/pcl/introduction_to_portable_class_libraries/" target="_blank">documentación</a> al respecto.

En este, recurrimos a biblioteclas PCL para Windows 8.1, Windows Phone 8.1 y .Net 4.5 para las clases de los clientes de servicios REST, DTOs, etc. Compartimos incluso algunos ViewModels. Por ejemplo, tenemos un interfaz que define un mecanismo para persistir datos de logging y telemetría. Como las implementaciones dependen de cada plataforma, la biblioteca PCL contiene únicamente el interfaz a implementar.

**Bibliotecas universales**

Dentro de las bibliotecas PCL, están las llamadas bibliotecas universales. Aparecieron junto al concepto de <a title="Creación de aplicaciones universales para Windows para cualquier dispositivo de Windows" href="https://dev.windows.com/es-es/develop/building-universal-Windows-apps" target="_blank">aplicaciones universales</a> con Windows 8.1 y Windows Phone 8.1 y, en esencia, se trata de bibliotecas PCL destinadas únicamente a ser utilizadas en aplicaciones para Windows 8.1 modern ui y para Windows Phone 8.1. Con la aparición de Windows 10, estas bibliotecas se extienden a otras plataformas, como Xbox, HoloLens y Raspberry Pi 2.

Eso significa que no podremos utilizar estas bibliotecas en proyectos .Net de escritorio o web, por ejemplo. Personalmente, teniendo en cuenta sus limitaciones, creo que el nombre _universal_ está mal escogido _universal_ 😉

Para interfaz de persistencia mencionado en el anterior ejemplo., las dos primeras implementaciones que necesitaba se limitaban a escribir los datos en el disco para posterior explotación en otros módulos de la aplicación. Como las aplicaciones universales no tienen acceso al espacio de nombre System.IO, recurrí a dos implementaciones: una en una biblioteca universal, que hacía uso de la clase StorageFile y otra en una biblioteca tradicional, que si dispone de acceso a System.IO.

**Proyectos de código compartido**

Estos proyectos son peculiares. Ni siquiera son proyectos que puedas compilar de forma individual. Al crear una aplicación universal se crea un proyecto de código compartido, aunque hay una <a title="Shared Project Reference Manager" href="https://visualstudiogallery.msdn.microsoft.com/315c13a7-2787-4f57-bdf7-adae6ed54450" target="_blank">extensión</a> que nos permite hacerlo manualmente para usar proyectos de código compartido en soluciones y proyectos existente.

Usar un proyecto de código compartido es lo más parecido a compartir una carpeta entre dos proyectos, solo que la forma de compartirla es añadiendo el proyecto de código compartido como una referencia. Durante la compilación, se incluirá el código del proyecto compartido como parte del proceso íntegro de compilación del proyecto que le esté referenciando.

Volviendo al ejemplo anterior, para Windows 8.1 y Windows Phone 8.1 desarrollé una implementación que persistía en SQLite. El código para acceder al API de SQLite es el mismo en ambas plataformas, pero la referencia que hay que añadir es distinta en <a title="SQLite for Windows Runtime (Windows 8.1) " href="https://visualstudiogallery.msdn.microsoft.com/1d04f82f-2fe9-4727-a2f9-a2db127ddc9a" target="_blank">Windows 8.1</a> y en <a title="SQLite for Windows Phone 8.1" href="https://visualstudiogallery.msdn.microsoft.com/5d97faf6-39e3-4048-a0bc-adde2af75d1b" target="_blank">Windows Phone 8.1</a>, por lo que no puedo llamar al código desde un proyecto Universal. Usando un proyecto de código compartido puedo añadir a cada proyecto la referencia pertinente de SQLite pero reutilizar en ambos proyectos el código que utiliza SQLite.

Es importante recordar que el código de los proyectos de código compartido **no** se compila como un ensamblado a parte, sino como parte de cada proyecto que le referencia, como si fuese código del propio proyecto.

En este <a title="Easier Code Sharing Across iOS, Android, and Windows" href="http://blog.xamarin.com/share-code-across-ios-android-and-universal-windows-apps-using-shared-projects/" target="_blank">post</a> se puede ver un ejemplo de su uso con proyectos que utilizan Xamarin.

&nbsp;