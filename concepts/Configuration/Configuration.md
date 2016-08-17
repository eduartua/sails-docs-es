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

You may use the `config/local.js` file to configure a Sails app for your local environment (your laptop, for example).  The settings in this file take precedence over all other config files except [.sailsrc](http://sailsjs.org/documentation/concepts/Configuration/usingsailsrcfiles.html).  Since they're intended only for local use, they should not be put under version control (and are included in the default `.gitignore` file for that reason).  Use `local.js` to store local database settings, change the port used when lifting an app on your computer, etc.

See [http://sailsjs.org/documentation/concepts/Configuration/localjsfile.html](http://sailsjs.org/documentation/concepts/Configuration/localjsfile.html) for more information.


### Accessing `sails.config` in your app

The `config` object is available on the Sails app instance (`sails`).  By default, this is exposed on the [global scope](http://sailsjs.org/documentation/concepts/Globals) during lift, and therefore available from anywhere in your app.

##### Example
```javascript
// This example checks that, if we are in production mode, csrf is enabled.
// It throws an error and crashes the app otherwise.
if (sails.config.environment === 'production' && !sails.config.csrf) {
  throw new Error('STOP IMMEDIATELY ! CSRF should always be enabled in a production deployment!');
}
```

### Setting `sails.config` values directly using environment variables

In addition to using configuration _files_, you can set individual configuration values on the command line when you lift Sails by prefixing the config key names with `sails_`, and separating nested key names with double-underscores (`__`).  For example, you could do the following to set the [CORS origin](http://sailsjs.org/documentation/concepts/security/cors) (`sails.config.cors.origin`) to "http://somedomain.com" on the command line:

```javascript
sails_cors__origin="http://somedomain.com" sails lift
```

This value will be in effect _only_ for the lifetime of this particular Sails instance, and will override any values in the configuration files.


> There are a couple of special exceptions to the rule: `NODE_ENV` and `PORT`.
> + `NODE_ENV` is a convention for any Node.js app.  When set to `'production'`, it sets [`sails.config.environment`](http://sailsjs.org/documentation/reference/configuration/sails-config#?sailsconfigenvironment). 
> + Similarly, `PORT` is just another way to set [`sails.config.port`](http://sailsjs.org/documentation/reference/configuration/sails-config#?sailsconfigport).  This is strictly for convenience and backwards compatibility.
>
> Here's a relatively common example where you might use both of these environment variables at the same time:
>
> ```bash
> PORT=443 NODE_ENV=production sails lift
> ```


### Custom Configuration
Sails recognizes many different settings, namespaced under different top level keys (e.g. `sails.config.sockets` and `sails.config.blueprints`).  However you can also use `sails.config` for your own custom configuration (e.g. `sails.config.someProprietaryAPI.secret`).

##### Example

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




### Configuring the `sails` Command-Line Interface

When it comes to configuration, most of the time you'll be focused on managing the runtime settings for a particular app: the port, database connections, and so forth.  However it can also be useful to customize the Sails CLI itself; to simplify your workflow, reduce repetitive tasks, perform custom build automation, etc.  Thankfully, Sails v0.10 added a powerful new tool to do just that.

The [`.sailsrc` file](http://sailsjs.org/documentation/anatomy/myApp/sailsrc.html) is unique from other configuration sources in Sails in that it may also be used to configure the Sails CLI-- either system-wide, for a group of directories, or only when you are `cd`'ed into a particular folder.  The main reason to do this is to customize the [generators](http://sailsjs.org/documentation/concepts/extending-sails/Generators) that are used when `sails generate` and `sails new` are run, but it can also be useful to install your own custom generators or apply hard-coded config overrides.

And since Sails will look for the "nearest" `.sailsrc` in the ancestor directories of the current working directory, you can safely use this file to configure sensitive settings you can't check in to your cloud-hosted code repository (_like your **database password**_.)  Just include a `.sailsrc` file in your "$HOME" directory.  See [the docs on `.sailsrc`](http://sailsjs.org/documentation/anatomy/myApp/sailsrc.html) files for more information.




### Notes
> The built-in meaning of the settings in `sails.config` are, in some cases, only interpreted by Sails during the "lift" process.  In other words, changing some options at runtime will have no effect.  To change the port your app is running on, for instance, you can't just change `sails.config.port`-- you'll need to change or override the setting in a configuration file or as a command-line argument, etc., then restart the server.



<docmeta name="displayName" value="Configuration">
