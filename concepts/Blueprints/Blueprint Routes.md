# Rutas Blueprint

Cuando corres el comando sails lift con blueprints habilitadas el framework inspecciona todos tus controladores, modelos y configuración con la finalidad de [enlazar ciertas rutas](http://sailsjs.org/documentation/concepts/Routes). Estas rutas de blueprints implícitas (algunas veces llamadas "shadow routes" o incluso sólo "shadow") le permiten a tu aplicación responder a ciertos requests sin que tengas que enlazar esas rutas manualmente en tu archivo config/routes.js. Por defecto, las rutas de blueprints apuntan a sus correspondientes *acciones* blueprints (observa " Blueprints Actions" abajo) cualquiera de los cuales puede ser eliminada con código.


Hay tres tipos de rutas de blueprints en Sails:

+ **Rutas RESTful**, donde el path es siempre `/:modelIdentity` o `/:modelIdentity/:id`.  Estas rutas usan el verbo HTTP para determinar la acción a tomar; por ejemplo una petición POST a /user creará un nuevo usuario y una petición DELETE a /user/123 borrará el usuario cuya primary key es 123. En un entorno de producción las rutas RESTful generalmente deberían ser protegidas por [políticas](http://sailsjs.org/documentation/concepts/Policies) que restrinjan el acceso no autorizado.
+ **Rutas Shortcut**, Donde la acción a tomar está codificada en el path.  Por ejemplo, el atajo `/user/create?name=joe` crea un nuevo usuario, mientras que `/user/update/1?name=mike` actualiza el usuario #1. Estas rutas solamente responden a requests tipo`GET`.  Las rutas Shortcut sun muy práticas para desarrollo, pero generalmente seberían ser deshabilitadas en un ambient de producción.
+ **Rutas Action**, el cual automáticamente crean rutas para las acciones de tus controladores personalizados.  Por ejemplo, si tienes un archivo `FooController.js` con un método `bar`, entonces una ruta `/foo/bar` será automáticamente creada para para ti mientras las rutas de acción blueprint esten habilitadas.  Diferente a las rutas RESTful y shortcut, las rutas action *no* requieren que un controlador tenga un correspondiente archivo de su modelo.


Ver la [subsección de blueprints en la referencia de configuración](http://sailsjs.org/documentation/reference/sails.config/sails.config.blueprints.html) para opciones de configuración blueprint, incluyendo cómo habilitar / desabilitar diferentes tipos de rutas blueprint.

<docmeta name="displayName" value="Blueprint Routes">
