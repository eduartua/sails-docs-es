# Assets

### Resumen

La palabra Assets se refiere a [archivos estáticos](http://en.wikipedia.org/wiki/Static_web_page) (js, css, images, entre otros) almacenados en tu servidor que quieres sean accesibles al mundo exterior (internet). En Sails, esos archivos son colocados en el directorio [`assets/`](http://sailsjs.org/documentation/anatomy/myApp/assets), donde son procesados y sincronizados a un directorio temporal oculto (`.tmp/public/`) cuando inicias (lift) tu aplicación. El contenido del directorio `.tmp/public` es lo que actualmente Sails sirve - aproximadamente equivalente a la carpeta "public" en [Express](https://github.com/expressjs), o la carpeta "www" con la que debes estar familiarizado si has trabajo con otros servidores web, tal como Apache.  Este paso intermedio le permite a Sails preparar/pre-compilar assets para uso en el cliente - Elementos de LESS, CoffeeScript, SASS, spritesheets, Jade templates, entre otros.

### Static middleware

Detrás de las escenas, Sails usa [static middleware](http://www.senchalabs.org/connect/static.html) de Express para servir tus assets. Puedes configurar este middleware (por ejemplo, cache settings) en [`/config/http.js`](http://sailsjs.org/documentation/reference/sails.config/sails.config.http.html).

##### `index.html`
Como la mayoría de servidores web, Sails respeta la conveción `index.html`.  Por ejemplo, si creas `assets/foo.html` en un nuevo proyecto de Sails, será accesible a través de `http://localhost:1337/foo.html`.  Pero si creas `assets/foo/index.html`, la misma será accesibe a través de ambas `http://localhost:1337/foo/index.html` y `http://localhost:1337/foo`.

##### Precedencia
Es importante notar que el [middleware estático](http://stephensugden.com/middleware_guide/) es instalado **depués** de el router en Sails.  Entonces, si defines una [ruta personalizada](http://sailsjs.org/documentation/concepts/Routes?q=custom-routes), pero también tienes un archivo en tu directorio de assets con un path conflictivo, la ruta personalizada interceptará el request antes que alcance el middleware estático. Por ejemplo, si creas `assets/index.html`, sin ninguna ruta definida en tu archivo [`config/routes.js`](http://sailsjs.org/documentation/reference/sails.config/sails.config.routes.html) será utilizado como tu home page.  Pero si defines una ruta personalizada, `'/': 'FooController.bar'`, ésa ruta tomará precedencia.



<docmeta name="displayName" value="Assets">
