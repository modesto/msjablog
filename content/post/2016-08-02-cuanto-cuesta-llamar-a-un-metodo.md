---
title: ¿Cuánto cuesta llamar a un método?
author: msanjuan
layout: post
date: 2016-08-02T00:03:45+00:00
url: /cuanto-cuesta-llamar-a-un-metodo/
dsq_thread_id:
  - 5032574048
categories:
  - Sin categoría

---
Últimamente Twitter está siendo toda una fuente de inspiración. En esta ocasión el hilo culpable es [este][1]. [Javier Cantón][2] hace un comentario respecto a la diferencia de rendimiento existente entre llamar a un método virtual versus llamar al mismo método a través de un interface.

En este post no pienso hablar de lo malvadas que son las optimizaciones prematuras, asumo que el comentario de Javier tenía su contexto, así que me voy a quedar con la parte que me ha resultado curiosa. [Luis Ruiz Pavón nos comentó][3] que hiciéramos este tipo de mediciones utilizando [BenchmarkDotNet][4] y, como me habían surgido un par de dudas, me puse manos a la obra.

**Aviso importante:** las cifras de las mediciones como tal no tienen importancia, cada ordenador tardará una cantidad distinta de nanosegundos en efectuar cada operación, lo que es importante en este caso es observar el diferencial de tiempo que es necesario para ejecutar el método en cada circunstancia.

## Llamada directa a método no virtual

Javier comentaba que en su contexto le interesaban las llamadas a métodos virtuales vs interfaces, descartando las llamadas directas a un método no virtual. No obstante, por dar un poco de contexto respecto al rendimiento,  me parecía intersante medirlo. Una llamada directa a un método no virtual tarda tan poco que hay veces que no es capaz de medirlo y me dice que tarda 0.0000 nanosegundos. En algunas de las pruebas la mediana era de **0.0025 ns**. En realidad este resultado se debe a que el Jit está haciendo un inline del método. Si decoramos el método con el atributo **[MethodImpl(MethodImplOptions.NoInlining)]** para que **no haga inline** (gracias Juanma), la mediana es de **1.96 ns**.

## Directa virtual vs Interface

En mi máquina, una llamada directa a un **método virtual** tarda unos **2.91 nanosegundos**. SI realizamos la misma llamada a través de un **interface**, necesita **3.82 nanosegundos** para cada operación.

Como he dicho antes, no voy a entrar a valorar si ahorrar 1 nanosegundo por cada llamada a un método es algo importante o no, es una cuestión de contexto y estoy seguro de que hay contextos en los que puede ser importante.

Si asumimos que estamos en un contexto en el que realmente nos importa no sufrir esta penalización, podríamos descartar el uso de interfaces, pero esto puede condicionarnos el diseño en muchos aspectos.

## Clases abstractas

Pensando en cómo podría salvar ese problema y obtener un rendimiento como el de una llamada directa pero con las ventajas que da tener un interface, se me ocurrió la posibilidad de utilizar una clase abstracta. Es cierto que no es exactamente lo mismo utilizar una clase abstracta que un interface, pero aún así creo que demuestra mucho más la intencionalidad que recurrir a un método virtual.

La duda era cuál sería el rendimiento de la llamada a través de una clase abstracta. ¿Sería equivalente al de un interface?

Pues resultó que no. Según mis mediciones, cada llamada tarda unos **2.88 nanosegundos**. Es más, tanto si llamaba a través de la clase abstracta como si llamaba a través de la implementación de la clase abstracta, los tiempos eran los mismos.

Ojo, aunque en principio parece que es incluso más rápido que la llamada directa a un método virtual, creo que la diferencia es despreciable y que si repitiese las mediciones varias veces, se turnarían por ser la más rápida 😉

## Reflection

Ya que estaba, por curiosidad medí el tiempo de hacer las llamadas utilizando reflexión, asumiendo que tenía cacheado el MethodInfo y midiendo únicamente el Invoke del método. Cada llamada tardaba **272.87 nanosegundos**, un abismo comparado con los otros mecanismos.

## Un poco más de detalle

[Juanma][5] me ha comentado, con toda la razón del mundo, que para este tipo de mediciones tan pequeñas resulta útil recurrir a histogramas. BenchmarkDotNet no tiene actualmente esta funcionalidad, pero lo que si proporciona es la desviación típica, que puede resultar útil para interpretar lo datos. Aquí están los resultados de la última medición efectuada:

<div class="table-responsive">
  <table  style="width:100%; "  class="easy-table easy-table-default " border="0">
    <tr>
      <th >
        Método
      </th>
      
      <th >
        Mediana
      </th>
      
      <th >
        Desviación
      </th>
    </tr>
    
    <tr>
      <td >
        Directo no virtual
      </td>
      
      <td >
        0.0025 ns
      </td>
      
      <td >
        0.0853 ns
      </td>
    </tr>
    
    <tr>
      <td >
        Direct no virtual no inline
      </td>
      
      <td >
        1.9673 ns
      </td>
      
      <td >
        0.1040 ns
      </td>
    </tr>
    
    <tr>
      <td >
        Directo virtual
      </td>
      
      <td >
        2.9151 ns
      </td>
      
      <td >
        0.0917 ns
      </td>
    </tr>
    
    <tr>
      <td >
        Interface
      </td>
      
      <td >
        3.8213 ns
      </td>
      
      <td >
        0.1464 ns
      </td>
    </tr>
    
    <tr>
      <td >
        Abstract
      </td>
      
      <td >
        2.8829 ns
      </td>
      
      <td >
        0.1113 ns
      </td>
    </tr>
    
    <tr>
      <td >
        Directo con reflexión
      </td>
      
      <td >
        272.8738 ns
      </td>
      
      <td >
        17.7846 ns
      </td>
    </tr>
  </table>
</div>

# Conclusión

Si alguna vez nos preocupa tantísimo el rendimiento, podemos utilizar una clase abstracta en lugar de un interface (ya, ya he dicho antes que no es lo mismo).

Cualquier excusa es buena para calentarse un poco la cabeza y sacar unos cuantos datos peculiares.

Este tipo de posts no valen prácticamente para nada, pero el datap0rn tiene su gracia y me ha resultado muy entretenido escribirlo.

BenchmarkDotNet mola, aunque tarda cojón y medio en hacer cada medición 😀

&nbsp;

 [1]: https://twitter.com/jcant0n/status/759081339098464257
 [2]: https://twitter.com/jcant0n
 [3]: https://twitter.com/luisruizpavon/status/759366097497886720
 [4]: https://github.com/PerfDotNet/BenchmarkDotNet
 [5]: https://twitter.com/gulnor?lang=en