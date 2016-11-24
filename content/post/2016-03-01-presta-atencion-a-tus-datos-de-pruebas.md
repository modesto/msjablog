---
title: Presta atención a tus datos de pruebas
author: msanjuan
layout: post
date: 2016-02-29T23:04:10+00:00
url: /presta-atencion-a-tus-datos-de-pruebas/
dsq_thread_id:
  - 4623070622
categories:
  - Sin categoría

---
Los datos utilizados en las pruebas son muy importantes y, aunque el tema de este post parece muy obvio, no son pocas la veces que me he encontrado con problemas debido a este tema, así que creo que merece la pena dedicarle al menos unas líneas.

Hay varios aspectos que son importantes a tener en cuenta al definir datos para nuestras pruebas automáticas. En este post me voy a centrar en tres errores que me encuentro con frecuencia:

  * Datos diferentes con los mismos valores
  * Exceso de datos innecesarios para el test
  * Utilización de datos que dan lugar a confusión

## Datos diferentes con los mismos valores

Este problema se da especialmente cuando hablamos de datos numéricos, ya que son los más susceptibles de ser repetidos en los tests. Supongamos que queremos probar un método que nos permite dividir dos números y obtener el resultado (un ejemplo muy original, eh? ;P). Un claro ejemplo de una prueba con datos incorrectos sería este:

<pre class="lang:c# decode:true ">[Test]
public void Divide() {
    var result = Divide(2, 2);
    result.Should().Be(1);
}</pre>

Teniendo en cuenta los datos de este test, es perfectamente posible confundir el dividendo y el divisor porque al invertirlos el resultado es el mismo. No sólo se dificulta la lectura del test, es que una implementación podría darnos un falso verde.

Evidentemente este ejemplo es muy básico, pero este tipo de errores se comete con bastante frecuencia cuando tenemos identificadores numéricos, valores para expresar cantidades o fechas. Es típico utilizar &#8220;1&#8221; como identificador numérico pero cuando tenemos que hacer una prueba con el usuario con id 1 de la empresa 1 es muy importante pararnos unos segundos y buscar datos que no puedan ser confundidos.

## Exceso de datos innecesarios para el test

Otro de los problemas habituales es proporcionar más datos de los estrictamente necesarios para el objetivo de nuestro test. Si estamos escribiendo un tests para verificar el funcionamiento de un API que puede recibir varios parámetros pero algunos son obligatorios y otros opcionales, lo recomendable es que creemos varios tests para contemplar desde el escenario más básico a los escenarios más complejos. Supongamos que tenemos una calculadora de descuentos para clientes que necesita recibir como parámetro un cliente que tenga un identificador, país de procedencia y actividad para poder calcular su descuento correspondiente.

<pre class="lang:default decode:true">var client = GivenAClient (Id:1, Name:"Peter", Country:"Spain", Activity:"Development", Sector:"Software");
discountCalculator.CalculateFor(client);</pre>

Leyendo este ejemplo parece que un cliente necesita todos los campos para poder calcular un descuento, pero estamos especificando el _sector_ y resulta que era irrelevante, lo que pasa es que nuestro afán de reutilización nos ha jugado una mala pasada. Si asumimos que nuestros tests pueden servir como documentación para otros desarrolladores (o incluso para nuestro yo del futuro), cuanto más concisos seamos con esta información, mejor.

## Utilización de datos que dan lugar a confusión

En otras ocasiones tenemos datos de presencia necesaria pero de valor irrelevante para la ejecución del test. Es importante que en el test quede claro que el valor nos da igual y que no influye en la ejecución del test. En este caso las consecuencias pueden ser similares al punto anterior ya que el test puede generar confusión al leerlo.

<pre class="lang:c# decode:true">var client = GivenAClient (Id:1, Name:"Peter", Country:"Spain", Activity:"Development");
discountCalculator.CalculateFor(client);</pre>

Al leer este código podríamos pensar que el país y la actividad son relevantes para el cálculo del descuento únicamente porque estamos utilizando valores específicos. Pero, ¿qué pasa si recurrimos a valores genéricos excepto cuando realmente el valor es relevante?

<pre class="lang:c# decode:true">var client = GivenAClient (Id:Any.Id, Name:Any.Name, Country:"Spain", Activity:Any.Activity);
discountCalculator.CalculateFor(client);</pre>

Con este pequeño cambio queda mucho más clara la información relevante para el test y tanto los compañeros como nuestro yo del futuro lo agradecerá mucho.

&nbsp;

&nbsp;