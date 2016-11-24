---
title: La importancia del rojo en TDD
author: msanjuan
layout: post
date: 2015-12-22T14:14:43+00:00
url: /la-importancia-del-rojo-en-tdd/
dsq_thread_id:
  - 4425305354
categories:
  - .Net

---
A un desarrollador que practique TDD, el flujo **rojo, verde, refactor** no le resulta extraño. En este post quiero romper una lanza a favor del **rojo**, ese gran incomprendido. Para ilustrarlo voy a recurrir a un ejemplo muy sencillo:

<pre class="lang:c# decode:true">[TestMethod]
public void sample_test()
{
    var messagingService = GivenAMessagingServiceStub();
    var unreadMessages = messagingService.UnreadMessages();
    unreadMessages.Count.Should().Be(1)
}</pre>

Este ejemplo esta simplificado, pero me permite mostrar un test con una estructura de tipo **given/when/then o AAA** (según gustos). Con el objetivo de obtener ese primer rojo, me encuentro a veces con esto:

<pre class="lang:default decode:true">public IEnumerable&lt;Message&gt; UnreadMessages()
{
    throw new NotImplementedException();
}</pre>

Creo que hacer esto desvirtúa el objetivo de buscar el rojo. Uno de los objetivos de buscar el primer rojo es verificar el test, pero es imposible verificar el test si no se llega a ejecutar completo. Es cierto que al lanzar el test va a dar un rojo, pero ese rojo es equivalente a haber lanzado la excepción en lugar de la llamada al código de producción, así que de poco vale. Otras veces se llega un poco más en la búsqueda del rojo con un código similar a este:

<pre class="lang:default decode:true">public IEnumerable&lt;Message&gt; UnreadMessages()
{
    return null;
}</pre>

En este caso es cierto que el test falla en la línea del **then**, pero personalmente sigue sin convencerme porque en realidad el rojo no se produce a causa del assert. La razón es sencilla y se ilustra bastante bien diseccionando el test de esta forma:

<pre class="lang:default decode:true">[TestMethod]
public void sample_test()
{
    var messagingService = GivenAMessagingServiceStub();
    var unreadMessages = messagingService.UnreadMessages();
    var count = unreadMessages.Count;
    count.Should().Be(1)
}</pre>

Con este ejemplo se puede ver claramente que el rojo no da debido al assert, da debido a una **NullPointerException** en la asignación de la variable **count**, lo que sigue alejado del objetivo de probar primero el test. En este caso, no cuesta mucho buscar un rojo que además permita probar el test completo:

<pre class="lang:default decode:true ">public IEnumerable&lt;Message&gt; UnreadMessages()
{
    return new List&lt;Message&gt;();
}</pre>

Por supuesto, es importante no olvidar que el primer paso nunca es el rojo, es pensar.