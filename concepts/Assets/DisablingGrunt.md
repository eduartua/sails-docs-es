# Desabilitando Grunt

Para deshabilitar la integración de Grunt en Sails, simplemente borra tu Gruntfile (y/o la carpeta [`tasks/`](http://sailsjs.org/documentation/anatomy/myApp/tasks) ). También puedes deshabilitar el Grunt hook. Sólo configura la propiedad `grunt` a `false` en `.sailsrc` hooks como sigue:

```json
{
    "hooks": {
        "grunt": false
    }
}
```

### Puedo personalizar esto para SASS, Angular, client-side Jade templates, entre otros?

Si puedes! Sólo reemplaza la tarea relevante en tu directorio `tasks/` , o agrega uno nuevo.  Algo como [SASS](https://github.com/sails101/using-sass) por ejemplo.

Si aún quieres usar Grunt para otros propósitos, pero no quieres ninguno de los elementos asociados al front-end, sólo borra la carpeta assets de tu proyecto y elimina las tareas orientadas al front-end de las carpetas `grunt/register/` y `grunt/config/` .  También puedes ejecutar `sails new myCoolApi --no-frontend` para omitir la carpeta de assets y tareas de Grunt orientdas a front-end para futuros proyectos.  Puedes también reemplazar tu modulo `sails-generate-frontend` con community generators alternativos, o [crear el tuyo propio](https://github.com/balderdashy/sails-generate-generator).  Esto le permite a `sails new` crear el boilerplate para aplicaciones nativas en iOS, Android, Cordova, SteroidsJS, entre otros.



<docmeta name="displayName" value="Disabling Grunt">

### NOTE:

Cuando elimines el grunt hook como se explica arriba debes también especificar lo siguiente en `.sailsrc` para que tus assets sean servidos, de otra manera todos los assets devolverán un `404`.

```json
{
    "paths": {
    	"public": "assets"
    }
}
```
