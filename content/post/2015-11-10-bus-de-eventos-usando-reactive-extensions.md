---
title: Bus de eventos usando Reactive Extensions
author: msanjuan
layout: post
date: 2015-11-10T09:32:59+00:00
url: /bus-de-eventos-usando-reactive-extensions/
dsq_thread_id:
  - 4305678045
categories:
  - .Net
  - WPF

---
Utilizar un bus de eventos dentro de una aplicación es algo bastante común. Un ejemplo muy sencillo podría ser desacoplar la lógica de envío de notificaciones al usuario en WPF desde los ViewModels sin necesidad de recurrir a la inyección de dependencias en el ViewModel. Aunque hasta ahora había recurrido siempre a programar mi propio bus de eventos, utilizar <a href="https://github.com/Reactive-Extensions/Rx.NET" target="_blank">Reactive Extensions</a> me ha resultado muy interesante, útil y sencillo. Este sería un ejemplo de bus de eventos utilizando RX:

<pre class="lang:c# decode:true">public static class ReactiveEventBus {
    private static readonly Subject&lt;object&gt; messageSubject = new Subject&lt;object&gt;();

    public static void Send&lt;T&gt;(T message) {
        messageSubject.OnNext(message);
    }

    public static IObservable&lt;T&gt; AsObservable&lt;T&gt;() { return messageSubject.OfType&lt;T&gt;(); }
}</pre>

Este código lo encontré <a href="http://rogeralsing.com/2010/01/23/rx-framework-building-a-message-bus/" target="_blank">aquí</a>.

Lo habitual en un bus de eventos es tener un mecanismo para enviar eventos y otro para suscribirnos a ellos. En este caso, en lugar de habilitar la suscripción, lo que me proporciona es la posibilidad de obtener un **IObservable<T>**, siendo **T** el tipo del mensaje que quiero observar.

Aplicado al ejemplo de las notificaciones, en la MainWindow (o donde sea) podría añadir este código:

<pre class="lang:c# decode:true">var subscription = ReactiveEventBus.AsObservable&lt;GlobalMessage&gt;().Subscribe(message =&gt; MessageBox.Show(message.Message));
Closed += (sender, args) =&gt; subscription.Dispose();
</pre>

Y eso me permitiría poder enviar mensajes desde cualquier ViewModel (o cualquier otro sitio) con este código:

<pre class="lang:c# decode:true">ReactiveEventBus.Send(new GlobalMessage("Hola!"));
</pre>

Es muy importante tener en cuenta la llamada al **Dispose()** de la suscripción para evitar problemas de liberación de recursos.

Lo interesante de que el bus de eventos me de un **IObservable<T>** es que, combinado con RX puedo hacer cosas más complejas de una forma muy sencilla, como:

<pre class="lang:c# decode:true ">ReactiveEventBus.AsObservable&lt;ScopedEvent&gt;()
    .Where(evt =&gt; evt.Scope == "Scope1")
    .Subscribe(evt =&gt; PUT YOUR CODE FOR Scope1 HERE);

ReactiveEventBus.AsObservable&lt;ScopedEvent&gt;()
    .Where(evt =&gt; evt.Scope == "Scope2")
    .Subscribe(evt =&gt; PUT YOUR CODE FOR Scope2 HERE);
</pre>

Respecto a si tiene sentido el uso de una propiedad para determinar el scope o sería más adecuado utilizar un objeto de distinto tipo, la respuesta es la de siempre: **depende** 😉

Algunos casos en los que me parece interesante son el envío un evento destinado únicamente a los UserControl que están mostrando una entidad concreta en un interfaz MDI o hacer que el scope de un evento sea una ventana concreta.