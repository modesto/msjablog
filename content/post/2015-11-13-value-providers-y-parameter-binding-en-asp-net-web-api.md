---
title: Value Providers y Parameter Binding en ASP.NET Web API
author: msanjuan
layout: post
date: 2015-11-13T10:02:22+00:00
url: /value-providers-y-parameter-binding-en-asp-net-web-api/
dsq_thread_id:
  - 4314953908
categories:
  - .Net
  - Web API

---
Cuando necesito acceder a información de la **Request** desde una acción de un controlador en ASP.NET Web API, la primera tentación es acceder directamente al objeto request y buscar lo que necesito. El principal problema de hacer esto es que convierte en un infierno hacer cualquier tipo de prueba automatizada para esta acción. Por ejemplo:

<pre class="lang:c# decode:true">[HttpGet] 
[Route("doAnything")] 
public IHttpActionResult Get() 
{ 
    var referrer = request.Headers.Referrer
    var resul = .....
    // DO ANY STUFF WITH THE REFERRER    
    return result; 
}</pre>

El mecanismo más sencillo que se me ocurre para hacer que esta acción sea fácil de probar es hacer que el dato que necesito de la request, le llegue como parámetro a la acción:

<pre class="lang:c# decode:true">public IHttpActionResult Get(string referrer) 
</pre>

Con esta modificación puedo programar sin problemas un test para esta acción y me quedo tan ancho. Pero claro, ¿qué pasa cuando ejecuto el código en Web API? Evidentemente falla. En este punto es donde Web API me echa un cable y me permite hacer algo como esto:

<pre class="lang:c# decode:true">public IHttpActionResult Get([ValueProvider(typeof(ReferrerValueProviderFactory))]string referrer) 
</pre>

Gracias a los **Value Providers** Web aPI puede inyectar en el parámetro el valor que necesito. El código sería este:

<pre class="lang:c# decode:true">public class RefererValueProviderFactory : ValueProviderFactory {
    public override IValueProvider GetValueProvider(HttpActionContext actionContext) {
        return new RefererValueProvider(actionContext.Request.Headers);
    }
}

public class RefererValueProvider : IValueProvider {
    private readonly HttpRequestHeaders headers;

    public RefererValueProvider(HttpRequestHeaders headers) {
        this.headers = headers;
    }

    public bool ContainsPrefix(string prefix) {
        return true;
    }

    public ValueProviderResult GetValue(string key) {
        return new ValueProviderResult(headers.Referrer, headers.Referrer.ToString(), CultureInfo.InvariantCulture);
    }
}</pre>

El código está simplificado para el ejemplo, pero es necesario aplicar control de errores. Por ejemplo, es posible que la cabecera sea nula, lo que lanzaría una excepción al intentar hacer el **ToString()**.

Para que lo que llevo de ejemplo funcione faltaría únicamente registrar el nuevo value provider  en el **Register** de **WebApiConfig**:

<pre class="lang:default decode:true">public static void Register(HttpConfiguration config)
{
    config.Services.Add(typeof(ValueProviderFactory), new ReferrerValueProviderFactory());

    ...</pre>

El problema que le veo hasta este punto es que el uso de el atributo [ValueProvide] dificulta la lectura y tener que registrarlo añade un poco de complejidad al setup y algo de lo que me tendría que acordar en el próximo proyecto. Teniendo en cuenta cómo se hacen las cosas en Web API, preferiría tener algo como esto:

<pre class="lang:c# decode:true">public IHttpActionResult Get([FromReferrerHeader]string referrer)</pre>

Algo como esto resulta mucho más legible y alineado con lo que ya estoy acostumbrado cuando recurro a atributos como **[FromBody]** o **[FromUri]**. Aquí es donde Web API me invita a utilizar **Parameter Binding**. Básicamente lo que voy a hacer es crear un atributo que herede de **HttpParameterBinding** y a utilizar el ValueProvider que he creado justo antes para obtener este dato:

<pre class="lang:c# decode:true ">public class FromReferrerHeaderAttribute : ParameterBindingAttribute {
    public override HttpParameterBinding GetBinding(HttpParameterDescriptor parameter) {
        var httpConfig = parameter.Configuration;
        var binder = new ModelBinderAttribute()
            .GetModelBinder(httpConfig, parameter.ParameterType);
        return new ModelBinderParameterBinding(
            parameter, binder,
            new ValueProviderFactory[] { new ReferrerValueProviderFactory() });
    }
} 
</pre>

&nbsp;

Además, al utilizar parameter binding, ya no es necesario registrar el value provider, lo que simplifica su uso.