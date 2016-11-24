---
title: Datos de ejemplo en el ViewModel para facilitar la vista de diseño
author: msanjuan
layout: post
date: 2014-12-17T00:27:25+00:00
url: /datos-de-ejemplo-en-el-viewmodel-para-facilitar-la-vista-de-diseno/
dsq_thread_id:
  - 3461532293
categories:
  - .Net
  - WPF

---
Al trabajar con XAML, especialmente si la vista tiene un listado de items, puede resultar muy complicado ajustar la vista para que se vea correctamente. Tareas como ajustar los márgenes, la posición de los elementos o cualquier tipo de cambio en los estilos se pueden convertir en algo bastante pesado.

Este es un ejemplo de una vista de que contiene un ListView sin datos de ejemplo en vista de diseño:

[<img class="  aligncenter wp-image-32 size-full" src="http://www.modestosanjuan.com/wp-content/uploads/2015/01/ventana-con-listview-vacío.png" alt="ventana con listview vacío" width="538" height="366" srcset="http://www.modestosanjuan.com/wp-content/uploads/2015/01/ventana-con-listview-vacío.png 538w, http://www.modestosanjuan.com/wp-content/uploads/2015/01/ventana-con-listview-vacío-300x204.png 300w" sizes="(max-width: 538px) 100vw, 538px" />][1]

La imagen corresponde a la vista de diseño de un código como este:

<pre class="lang:xhtml decode:true">&lt;ListBox ItemsSource="{Binding SampleData}"&gt;
    &lt;ListBox.ItemTemplate&gt;
        &lt;DataTemplate&gt;
            &lt;TextBlock Text="{Binding Name}"&gt;&lt;/TextBlock&gt;
        &lt;/DataTemplate&gt;
    &lt;/ListBox.ItemTemplate&gt;
&lt;/ListBox&gt;</pre>

Este es un ejemplo que ilustra bastante bien el problema, no se ve nada. Se trata de un fragmento de XAML muy sencillo y no presenta grandes problemas para intuir lo que se va a pintar, pero en cuanto la cosa se complica un poco más, ir a ciegas no resulta nada agradable. Como no tenemos datos de ejemplo, no somos capaces de saber cómo van a mostrarse los items de la lista y cualquier tipo de cambio se hace a ciegas. Para cambios pequeños, se recurre al ensayo y error, pero esto es una solución muy engorrosa y poco fluida. Para evitar ir a ciegas tenemos que hacer dos cosas:

**Paso 1**

Decirle a la vista que cree una instancia del ViewModel en vista de diseño:

<pre class="lang:xhtml decode:true">xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
xmlns:wpfApplication1="clr-namespace:WpfApplication1"
mc:Ignorable="d"
d:DataContext="{d:DesignInstance Type=wpfApplication1:SampleViewModel, IsDesignTimeCreatable=True}"
</pre>

La clave está en la última línea. El tipo del ViewModel es **SampleViewmodel** y al poner a _true **el**_ **IsDesignTimeCreatablele,** se hablilita la creación de una instancia del viewmodel en vista de diseño.

**Paso 2**

Para hacer que el ViewModel tenga datos de ejemplo, lo único necesario es asignar los datos en el constructor. En la vista de ejemplo se puede llamar desde el constructor a un método que haga algo como esto:

<pre class="lang:c# decode:true">if (DesignerProperties.GetIsInDesignMode(new DependencyObject()))
{
    var sampleData = new List&lt;ClientData&gt;();
    sampleData.Add(new ClientData() { Name = "Sample Client 1", Email = "email@domain.com" });
    sampleData.Add(new ClientData() { Name = "Sample Client 2", Email = "email@domain.com" });
    sampleData.Add(new ClientData() { Name = "Sample Client 3", Email = "email@domain.com" });
    sampleData.Add(new ClientData() { Name = "Sample Client 4", Email = "email@domain.com" });
    sampleData.Add(new ClientData() { Name = "Sample Client 5", Email = "email@domain.com" });
    sampleData.Add(new ClientData() { Name = "Sample Client 6", Email = "email@domain.com" });

    SampleData = new ObservableCollection&lt;ClientData&gt;(sampleData);
}
</pre>

Es muy importante que únicamente se rellenen los datos en vista de diseño, para que no se rellenen durante la ejecución de la aplicación. Por eso el _if_ de la primera línea.

Una vez hecho esto, todos los bindings de datos con el ViewModel mostrarán los datos de ejemplo en vista de diseño, pudiendo así hacer los ajustes de estilos sin necesidad de ejecutar la aplicación. Por ejemplo:

<pre class="lang:xhtml decode:true">&lt;Window x:Class="WpfApplication1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:wpfApplication1="clr-namespace:WpfApplication1"
        mc:Ignorable="d"
        d:DataContext="{d:DesignInstance Type=wpfApplication1:SampleViewModel, IsDesignTimeCreatable=True}"
        Title="MainWindow" Height="350" Width="525"&gt;
    &lt;Grid&gt;
        &lt;ListBox ItemsSource="{Binding SampleData}"&gt;
            &lt;ListBox.ItemTemplate&gt;
                &lt;DataTemplate&gt;
                    &lt;StackPanel Orientation="Horizontal"&gt;
                        &lt;TextBlock Text="{Binding Name}"&gt;&lt;/TextBlock&gt;
                        &lt;TextBlock Text="{Binding Email}" Margin="15,0,0,0"&gt;&lt;/TextBlock&gt;
                    &lt;/StackPanel&gt;
                &lt;/DataTemplate&gt;
            &lt;/ListBox.ItemTemplate&gt;
        &lt;/ListBox&gt;
    &lt;/Grid&gt;
&lt;/Window&gt;
</pre>

Y así es como se vería en vista de diseño:

<p style="text-align: center;">
  <a href="http://www.modestosanjuan.com/wp-content/uploads/2015/01/ventana-con-listview-con-datos-de-ejemplo.png"><img class="alignnone size-full wp-image-35" src="http://www.modestosanjuan.com/wp-content/uploads/2015/01/ventana-con-listview-con-datos-de-ejemplo.png" alt="ventana con listview con datos de ejemplo" width="540" height="361" srcset="http://www.modestosanjuan.com/wp-content/uploads/2015/01/ventana-con-listview-con-datos-de-ejemplo.png 540w, http://www.modestosanjuan.com/wp-content/uploads/2015/01/ventana-con-listview-con-datos-de-ejemplo-300x201.png 300w" sizes="(max-width: 540px) 100vw, 540px" /></a>
</p>

Si no quieres meter los datos de ejemplo en tu clase de ViewModel, siempre puedes crear una clase específica para el ViewModel en tiempo de diseño. Sólo hay que cambiar el valor del _DesignInstance_ en el archivo XAML y darle el nombre del tipo para la vista de diseño. Eso si, mejor que recurras a interfaces o algo similar para asegurarte de que ambos ViewModel (el de vista de diseño y el que se ejecutará en real) compartan el mismo contrato.

&nbsp;

\*\*IMPORTANTE:\*\* Para que las vistas tomen los datos de ejemplo debemos compilar el proyecto para que se compile el ViewModel. Si hacemos cambios en el conjunto de datos de ejemplos del ViewModel, deberemos compilar de nuevo, así que lo mejor es crear primero un conjunto de datos de ejemplo dignos y así luego podemos centrarnos en el trabajo de la vista.

 [1]: http://www.modestosanjuan.com/wp-content/uploads/2015/01/ventana-con-listview-vacío.png