# Blueprints

### Resumen

Como cualquier buen framework, Sails tiene por objetivo reducir la cantidad de código que escribes y el tiempo que toma colocar una aplicación funcional corriendo.  Las _Blueprints_ son la forma en que Sails&rsquo;  genera rápidamente API [routes](http://sailsjs.org/documentation/concepts/routes) y [actions](http://sailsjs.org/documentation/concepts/controllers#?actions) basadas en el diseño de tu aplicación.

Juntas, [blueprint routes](http://sailsjs.org/documentation/concepts/blueprints/blueprint-routes) y [blueprint actions](http://sailsjs.org/documentation/concepts/blueprints/blueprint-actions) constituyen la **blueprint API**, la lógica	interna que compone el potencial de la [RESTful JSON API](http://en.wikipedia.org/wiki/Representational_state_transfer) el cual obtienes cada vez que creas un modelo y u controlador.

Por ejemplo, si creas un arhivo modelo `User.js` y un arhivo controlador `UserController.js` en tu proyecto, a continuación con las blueprints habilitadas serás capaz de visitar inmediatamente `/user/create?name=joe` para crear un usuario, y visitar `/user` para ver un arreglo de los usuarios de tu app.  Todo sin escribir tan siquiera una sola línea de código!

Blueprints son estupendas para hacer prototipos, pero son también una herramienta poderosa en producción, debido a sus capacidades de ser eliminadas, protegidas, extendidas o deshabilitadas completamente.

<docmeta name="displayName" value="Blueprints">
