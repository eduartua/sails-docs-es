# Configuración

### Resumen

Si bien Sails diligentenmente incorpora la filosofía de [convención-sobre-configuración](http://en.wikipedia.org/wiki/Convention_over_configuration), es importante entender como personalizar esos prácticos valores predeterminados de vez en cuando.  Por casi cada convención en Sails, hay un conjunto de acompañamiento de opcíones de configuración que te permiten ajustar o eliminar cosas para ajustarte a tus necesidades.  Esta sección de la documentación incluye una referencia completa de las opciones de configuración disponibles en Sails.

Las aplicaciones en Sails pueden ser [programáticamente configuradas](https://github.com/mikermcneil/sails-generate-new-but-like-express/blob/master/templates/app.js#L15), especificando [variables de entorno](http://en.wikipedia.org/wiki/Environment_variable) o argumentos de línea de comandos, cambiando los archivos globales o locales [`.sailsrc` ](http://sailsjs.org/documentation/anatomy/myApp/sailsrc.html), o (más comúnmente) usando los archivos de configuración boilerplate (código reusable) convencionalmente localizados en la carpeta [`config/`](http://sailsjs.org/documentation/anatomy/myApp/config) de nuevos proyectos. La autoritativa, configuración merged-together usada en tu app está disponible en runtime en la global `sails` como `sails.config`.


### Archivos de configuración estándard (`config/*`)

Cierto número de archivos de configuración son genereados por defecto en nuevas aplicaciones Sails.  Ésos archivos boilerplate (código reusable) incluyen una cantidad de comentarios inline, el cual son diseñados para proveer una rápida referencia sobre la marcha sin tener que saltar de aquí para allá entre documentos y tu editor de texto.

En la mayoría de los casos, las llaves the top-level en el objeto `sails.config` (por ejemplo `sails.config.views`) corresponden a un archivo de configuración en particular (por ejemplo `config/views.js`) en tu aplicación; sin embargo, los ajustes de configuración pueden ser arreglados de la manera que quieras a través de los archivos en tu directorio `config/` .  La parte importante es el nombre (por ejemplo key) de los ajustes- no el archivo de donde viene.

Por ejemplo, digamos que agregas un nuevo archivo, `config/foo.js`:

```js
// config/foo.js
// El objeto abajo será fusionado en `sails.config.blueprints`:
module.exports.blueprints = {
  shortcuts: false
};
```

Para una referencia exhaustiva de oprciones de configuración individual y el archivo donde se encuentra por defecto, revisa la páginas de referencia in esta sección, e échale un vistazo a ["`config/`"](http://sailsjs.org/documentation/anatomy/myApp/config) en [La Anatomía de una Aplicación Sails](http://sailsjs.org/documentation/anatomy) para un resumen más amplio.

### Archivos de entorno específico (`config/env/*`)

Configuraciones especificadas en los archivos de configuración estándar generalmente estarán disponibles en todos los entornos (por ejemplo, desarrollo, producción, test, entre otros.).  Si quisieras tener algunos ajustes que tomen edecto sólo en ciertos ambientes, puedes usar los dirctorios y archivos especiales de ambientes específicos:

* Cualquier archivo guardado en el directoiro `/config/env/<environment-name>` será cargado *solamente* cuando Sails es lifted en el ambiente `<environment-name>`.  Por ejemplo, archivos guardados en `config/env/production` serán solamente cargados cuando Sails es ejecutado  en modo producción.
* Cualquier archivo guardado como `config/env/<environment-name>.js` será cargado *solamente* cuando Sails es ejecutado en el ambiente `<environment-name>`, y será fusionado en la parte superior (top) de cualquier otro ajuste cargado desde el sub-directorio de ambiente específico.  Por ejemplo, ajustes en `config/env/production.js` tomarán precedencia sobre aquellos archivos in el directorio  `config/env/production`.  

Por defecto, tu aplicación corren en el ambiente "development".  La manera de abordar recomendada el cambio de ambientes de tu aplicación es usando la variamble de entorno `NODE_ENV` :
```
NODE_ENV=production node app.js
```

> El entorno `production` es especial-- dependiendo de tu configuración, habilita compression, caching, minification, entre otros. 
>
> También ten en cuanta que si estás usando `config/local.js`, la configuración exportada en ese archivo toma precedencia sobre los archivos de configuración environment-specific.


### El archivo `config/local.js`

Puedes utilizar el archivo `config/local.js` para configurar una app Sails en tu entorno local (computadora portátil, por ejemplo).  La configuración de este archivo tiene precedencia sobre todos los demás archivos de configuración, excepto  [.sailsrc](http://sailsjs.org/documentation/concepts/Configuration/usingsailsrcfiles.html). Debido a que está destinado solo para su uso local, no debería ser puesto bajo el control de versiones (incluirlo en el archivo `.gitignore` por esta razón).  Utilizar `local.js` para almacenar la configuración de bases de datos locales, cambiar el puerto utilizado para levantar una aplicación en la computadora, etc.

Ver [http://sailsjs.org/documentation/concepts/Configuration/localjsfile.html](http://sailsjs.org/documentation/concepts/Configuration/localjsfile.html) para más información.


### Accediendo a `sails.config` en tu app

El objeto `config` está disponible en la instancia de aplicación de Sails (`sails`).  Por defecto, esta es expuesta en el [global scope](http://sailsjs.org/documentation/concepts/Globals) durante la ejecución, y por lo tanto disponible desde cualquier ubicación en tu app.

##### Ejemplo
```javascript
// Este ejemplo comprueba que, si estamos en modo profucción, csrf está habilitado.
// Arroja un error y bloquea la ejecución de la aplicación de cualquier manera.
if (sails.config.environment === 'production' && !sails.config.csrf) {
  throw new Error('STOP IMMEDIATELY ! CSRF should always be enabled in a production deployment!');
}
```

### Configurando valores de `sails.config` directamente usando variables de entorno

Además de usar _archivos_ de configuración, puedes agrupar valores de configuración individual en la línea de comandos cuando ejecutas Sails anteponiendo los nombres claves con `sails_`, y separando nombres claves que están anidados (nested) con doble-underscores (`__`).  Por ejemplo, podrías hacer lo siguiente para agrupar el [CORS origin](http://sailsjs.org/documentation/concepts/security/cors) (`sails.config.cors.origin`) a "http://algúndominio.com" en la línea de comandos:

```javascript
sails_cors__origin="http://somedomain.com" sails lift
```

Este valor estará en vigencia por el resto de la vida de _solamente_ esta instancia Sails en  particular, y anulará cualquier valor en los archivos de configuración.


> Hay ciertas excepciones especiales aplicadas a la regla: `NODE_ENV` y `PORT`.
> + `NODE_ENV` es una convención para cualquier app de Node.js.  Cuando se configura para `'producción'`, configura [`sails.config.environment`](http://sailsjs.org/documentation/reference/configuration/sails-config#?sailsconfigenvironment). 
> + Similarmente, `PORT` es sólo otra forma de ajustar [`sails.config.port`](http://sailsjs.org/documentation/reference/configuration/sails-config#?sailsconfigport).  Esto es estrictamente por conveniencia y compatibilidad con versiones previas.
>
> Aquí hay un ejemplo relativamente común donde puedes usar ambas variables de entorno al mismo tiempo:
>
> ```bash
> PORT=443 NODE_ENV=production sails lift
> ```


### Configuración Personalizada
Sails reconoce muchas configuraciones diferentes, cuyos espacios de nombres estén bajo claves  de nivel superior (ejemplo `sails.config.sockets` y `sails.config.blueprints`).  Sin embargo puedes también usar `sails.config` para tu propia configuración personalizada (por ejemplo `sails.config.someProprietaryAPI.secret`).

##### Ejemplo

```javascript
// config/linkedin.js
module.exports.linkedin = {
  apiKey: '...',
  apiSecret: '...'
};
```

```javascript
// In your controller/service/model/hook/whatever:
// ...
var apiKey = sails.config.linkedin.apiKey;
var apiSecret = sails.config.linkedin.apiSecret;
// ...
```




### Configurando la Interfaz de la Línea de Comandos de `sails` 

Cuando de configuración se trata, la mayoría de las veces estarás enfocado en gestionar la configuración del runtime para una particular app: el puerto, las conexión a la base de datos, automatización, entre otros.the port, database connections, and so forth.  Sin  embargo, puede ser de utilidad personalizar la línea de comandos de Sails; pata simplificar tu flujo de trabajo, reduce tareas repetitivas,  realiza buils automatizados personalizados, entre otros.  Todo esto gracias a que en Sails v0.10 se agregó una poderosa herramienta para ello.

El [archivo `.sailsrc` ](http://sailsjs.org/documentation/anatomy/myApp/sailsrc.html) es único de otros archivos fuentes de configuración en Sails, también puede ser usado para configurar la línea de comandos de Sails -- ya sea a nivel de todo el sistema, para un grupo de directorios, o solamente cuando estás ingresado a un directorio en particular con `cd`.  La pricipal razón para hacer esto es para personalizar los [generadores](http://sailsjs.org/documentation/concepts/extending-sails/Generators) que son usados cuando `sails generate` y `sails new` son ejecutados, pero también puede ser de utilidad para instalar tus propios generadores personalizados o para anular configuraciones forzosamente codificadas (hard-coded).

Y puesto que Sails buscará por el `.sailsrc` "más cercano" en los directorios superiores del actual directorio de trabajo, puedes con toda seguridad usar este archivo para ajustar configuraciones sensibles que no puedes verificar en tu repositorio alojado en la nube (_como tu **contrseña de la base de datos**_.)  Sólo incluye un archivo `.sailsrc` en tu directorio "$HOME" .  Ver [documentación en archivos `.sailsrc`](http://sailsjs.org/documentation/anatomy/myApp/sailsrc.html) para más información.




### Notas
> El significado de configuraciones que está incorporado en `sails.config` es, en algunos casos, solamente interpretado por Sails durante el proceso de "lift".  En otras palabras, cambiando algunas opciones al momento de ejecución no tendrá efecto alguno.  Para cambiar el puerto en el que tu app se está ejecutando, por ejemplo,  no puedes solamente cambiar `sails.config.port`-- necesitarás cambiar o anular la configuración en un archivo de configuración o como un argumento de la línea de comandos, entre otros. Luego reiniciar el servidor.


<docmeta name="displayName" value="Configuration">
