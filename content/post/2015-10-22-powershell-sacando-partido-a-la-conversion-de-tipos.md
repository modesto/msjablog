---
title: '[PowerShell] Sacando partido a la conversión de tipos'
author: msanjuan
layout: post
date: 2015-10-22T00:27:51+00:00
url: /powershell-sacando-partido-a-la-conversion-de-tipos/
dsq_thread_id:
  - 4247947647
categories:
  - PowerShell

---
PowerShell ETS (Extended Type System) forma parte de las tripas de PowerShell y es la parte que permite que convivan tipos de .Net, COM, ADSI y WMI pero que a nosotros se nos muestren como objetos de PowerShell de primer orden.

ETS tiene mucha potencia y no siempre es fácil obtener documentación sobre cómo hacer las cosas, pero en este post quiero dejarle a mi yo futuro un regalito para cuando necesite realizar conversión de tipos de .Net en PowerShell.

El ejemplo es muy sencillo y me ha surgido en un contexto real y para explicarlo lo voy a simplificar todo lo que pueda. Imaginemos que tenemos un tipo personalizado tal que este:

<pre class="lang:ps decode:true">Add-Type -TypeDefinition @”
using System;
using System.Collections;
 
namespace Configuration
{
    public class ConfigFolder {
        public ConfigFolder() {}
        
        public ConfigFolder(string path) {
            Path = path;
        }

        public string Path { get; set; }

        public override string ToString() {
            return Path;
        }
    }
}
“@
</pre>

Al sobreescribir el método **ToString()** del tipo, esto nos permite hacer cosas como esta:

<pre class="lang:ps decode:true">Function Write-AsString($folder) {
    "as string we get: {0}" -f $folder
}

$configFolder = New-Object Configuration.ConfigFolder
$configFolder.Path = "c:\anyPath\"

Write-AsString $configFolder
</pre>

Como la función **Write-AsString** no especifica el tipo para **$folder**, al llamar a la función con el parámetro de tipo **ConfigFolder**, se está llamando al método **ToString()** para convertir el valor automáticamente. El tema es que si queremos sacarle un poco más de partido y especificar un tipo que no sea string a **$folder**, lo que obtendremos será un error. Por ejemplo:

<pre class="lang:ps decode:true">Function Write-FullName([System.IO.DirectoryInfo]$folder) {
    "BaseName is: {0}" -f $folder.BaseName
}

$configFolder = New-Object Configuration.ConfigFolder
$configFolder.Path = "c:\anyPath\"

Write-FullName $configFolder
</pre>

Al ejecutar este código nos dará un error de conversión de tipos porque PowerShell no tiene ni la menor idea de cómo convertir un ConfigFolder en un System.IO.DirectoryInfo. Aquí es donde ETS viene al rescate.  Además de añadir el tipo ConfigFolder, es necesario añadir un tipo que realice la conversión automáticamente:

<pre class="lang:ps decode:true ">Add-Type -TypeDefinition @”
using System;
using System.Collections;
using System.Management.Automation;
 
namespace Configuration
{
    public class ConfigFolderConverter : PSTypeConverter {
        public override bool CanConvertFrom(Object sourceValue, Type destinationType)
        {
            string src = sourceValue as string;
            if (src != null) {
                return true;
            }
            return false;
        }
 
        public override object ConvertFrom(object sourceValue, Type destinationType, IFormatProvider provider, bool IgnoreCase) {
            if (sourceValue == null) { throw new InvalidCastException("no conversion possible"); }
            if (this.CanConvertFrom(sourceValue, destinationType)) {
                try {
                    string src = sourceValue as string;
                    return new Aida.ConfigFolder(src);
                }
                catch (Exception) { throw new InvalidCastException("no conversion possible"); }
            }
            throw new InvalidCastException("no conversion possible");
        }
        public override bool CanConvertTo(object value, Type destinationType) { 
            if((destinationType == typeof(System.IO.DirectoryInfo)) && (value.GetType() == typeof(Configuration.ConfigFolder))) {
                return true;
            }
            return false; 
        }
        public override object ConvertTo(object value, Type destinationType, IFormatProvider provider, bool IgnoreCase) {
            if (CanConvertTo(value, destinationType)) {
                return new System.IO.DirectoryInfo((value as Configuration.ConfigFolder).Path);
            }
            throw new InvalidCastException("no conversion possible");
        }
    }
}
“@</pre>

Por supuesto, este tipo es un ejemplo y podría dejarse un código mucho más limpio, pero cualquiera que sepa lo que es experimentar con el **Add-Type** de PowerShell entenderá que porqué prefiero no dedicarle mucho tiempo a limpiarlo y a irlo probando ;).

Después de añadir este tipo, es necesario decirle a PowerShell que lo tiene que usar. Para esto es necesario crear un archivo (por ejemplo: ConfigFolder.Converter.ps1xml) que sería así:

<pre class="lang:xhtml decode:true ">&lt;Types&gt;
  &lt;Type&gt;
    &lt;Name&gt;Configuration.ConfigFolder&lt;/Name&gt;
    &lt;TypeConverter&gt;
      &lt;TypeName&gt;Configuration.ConfigFolderConverter&lt;/TypeName&gt;
    &lt;/TypeConverter&gt;
  &lt;/Type&gt;
&lt;/Types&gt;</pre>

Y luego ejecutar el comando:

<pre class="lang:ps decode:true">Update-TypeData .\ConfigFolder.Converter.ps1xml</pre>

Una vez hecho esto, el ejemplo que antes fallaba ahora funcionará perfectamente, permitiendo conversiones automáticas del tipo **ConfigFolder** a **System.IO.DirectoryInfo**.

Esto es sólo un ejemplo de lo que PowerShell ETS tiene para ofrecernos. Además de custom converters, también permite extender clases existentes, añadir alias y algunas cosillas curiosas.

&nbsp;