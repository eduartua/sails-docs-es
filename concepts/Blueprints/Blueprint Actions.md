# Acciones Blueprint

Las acciones Blueprint (no confundir con blueprint action *routes*) son acciones genericas diseñadas para trabajar con cualquiera de tus controladores los cuales tienen un modelo con el mismo nombre (por ejemplo `ParrotController` necesitaría un modelo llamado `Parrot` ).  Piensa en ellos como el comportamiento por defecto de tu aplicación.  Por ejemplo, si tienes un modelo llamado `User.js` y un controlador vacío `UserController.js` , `find`, `create`, `update`, `destroy`, `populate`, `add` y `remove` las acciones existen implicitamente, sin que tengas que escribirlas.

Por defecto, el blueprint RESTful routes y shortcut routes estan asociados a sus correspondientes acciones blueprint. Sin embargo, cualquier acción blueprint puede ser anulada por un controlador particular creando una acción personalizada en ése archivo de controlador (por ejemplo `ParrotController.find`).  Alternativamente, puedes anular la acción blueprint  _en cualquier lugar de tu app_ creando tu propia acción personalizada. (por ejemplo `api/blueprints/create.js`).

La actual versión de Sails tiene incluida las siguiente acciones blueprint:

+ [find](http://sailsjs.org/documentation/reference/blueprint-api/Find)
+ [findOne](http://sailsjs.org/documentation/reference/blueprint-api/FindOne)
+ [create](http://sailsjs.org/documentation/reference/blueprint-api/create)
+ [update](http://sailsjs.org/documentation/reference/blueprint-api/Update)
+ [destroy](http://sailsjs.org/documentation/reference/blueprint-api/Destroy)
+ [populate](http://sailsjs.org/documentation/reference/blueprint-api/Populate)
+ [add](http://sailsjs.org/documentation/reference/blueprint-api/Add)
+ [remove](http://sailsjs.org/documentation/reference/blueprint-api/Remove)

Para más información acerca de blueprints, incluyendo como desabilitarlas y anularlas, ver la referencia[referencia de Blueprint API](http://sailsjs.org/documentation/reference/blueprint-api)

<docmeta name="displayName" value="Blueprint Actions">
