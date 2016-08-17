# Automatización de Tareas

### Resumen
El directorio [`tasks/`](http://sailsjs.org/documentation/anatomy/tasks) contiene una serie de [tareas Grunt](http://gruntjs.com/creating-tasks) y sus [configuraciones](http://gruntjs.com/configuring-tasks).

Las tareas son principalmente útiles para la construcción de assets del front-end, (tales como stylesheets, scripts, y plantillas de markup del lado del client) pero también pueden ser usadas para automatizar cualquier tipo de tareas rutinarias en el proceso de desarrollo, desde compilación de [browserify](https://github.com/jmreidy/grunt-browserify) hasta [migraciones de bases de datos](https://www.npmjs.org/package/grunt-db-migrate).

Sails empaqueta algunas [tareas por defecto](http://sailsjs.org/documentation/grunt/default-tasks) por conveniencia, pero con [literalmente con cientos de plugins](http://gruntjs.com/plugins) para escoger, puedes usar tareas para automatizar cualquier cosa con mínimo esfuerzo.  Si alguien aún no ha desarrollado lo que necesitas, puedes siempre [autor](http://gruntjs.com/creating-tasks) y [publicar tu propio plugin de Grunt](http://gruntjs.com/creating-plugins) hacia [npm](http://npmjs.org)!

> Si nunca antes habías usado [Grunt](http://gruntjs.com/), asegúrarte de revisar la guía [Getting Started](http://gruntjs.com/getting-started) , ya que explica cómo crear un [Gruntfile](http://gruntjs.com/sample-gruntfile) así como instalar y utilizar Grunt plugins.


### Asset pipeline

El asset pipeline es el lugar donde arganizarás tus assets, los cuáles serán inyectados a tus vistas y puede ser encontrado en el archivo `tasks/pipeline.js` . Configurar esos assets es simple y se usa [el archivo de configuración de tareas grunt](http://gruntjs.com/configuring-tasks#files) y [patrones wildcard/glob/splat](http://gruntjs.com/configuring-tasks#globbing-patterns). Son desglosados en tres secciones.

##### Archivos CSS a inyectar
Este es un arreglo de archivos css a ser inyectados en tu html como etiquetas `<link>` .  Ésas etiquetas serán inyectadas entre los comentarios `<!--STYLES--><!--STYLES END-->` en cualquier vista que aparezcan.

##### Archivos Javascript a Inyectar
Este es un arreglo de archivos Javascript que son insertados en tu html como una etiqueta `<script>`.  Éstas etiquetas será n inyectadas entre los comentarios `<!--SCRIPTS--><!--SCRIPTS END-->` en cualquier vista en la cuál aparecen. Los archivos son insertados en el orden en el cuál están en el arreglo (Por ejemplo, deberías de colocar el path de las dependencias antes del archivo que depende de ellas.)

##### Archivos de Plantilla a Inyectar
Este es un arreglo de archivos html que serán compilados en una función jst y serán colocados en un archivo jst.js. Este archivo luego es insertado como una etiqueta `<script>` entre los comentarios `<!--TEMPLATES--><!--TEMPLATES END-->` de tu html.

> Los mismos patrones grunt wildcard/glob/splat y configuración del archivo de tareas son usados en algunos de los archivos de configuración  de tareas js, si te gustaría cambiar esos también.

### Configuración de Tareas

Las tareas configuradas son un conjunto de reglas que el Gruntfile sigue cuando es ejecutado. Son completamente personalizables y están localizadas en el directorio [`tasks/config/`](http://sailsjs.org/documentation/anatomy/my-app/tasks/config) . Puedes modificar, omitir, o sustituir esas tareas Grunt para que se adapten a tus requerimientos. También puedes agregar tus propias tareas de Grunt - sólo agraga un archivo `someTask.js` en este directorio para configurar la nueva tarea, luego regístrala con la tarea(s) padre apropiada (ver archivos en `tasks/register/*.js`). Recuerda, Sails viene con un conjunto de tareas que son útiles y que son diseñadas para ponerse en funcionamiento sin requerir de configuración alguna.

##### Configurando una tarea personalizada

Configurar una tarea personalizada dentro de tu proyecto es bastante simple y se usan las APIs de Grunt [config](http://gruntjs.com/api/grunt.config) y [task](http://gruntjs.com/api/grunt.task) para permitirte hacer las tareas de manera modular. Veamos un rápido ejemplo de cómo crear una nueva tarea que reemplaza una tarea existente. Digamos que queremos usar el motor de plantillas [Handlebars](http://handlebarsjs.com/) en vez del motor de plantillas underscore que viene configurado por defecto:

* El primer paso es instalar el plugin de grunt handlebars usando el siguiente comando en tu terminal:

```bash
npm install grunt-contrib-handlebars --save-dev
```

* Crea un archivo de configuración en `tasks/config/handlebars.js`. Aquí es donde pondremos nuesta configuración de handlebars.

```javascript
// tasks/config/handlebars.js
// --------------------------------
// handlebar task configuration.

module.exports = function(grunt) {

  // Usamos el método de la api grunt.config para configurar un
  // objeto para definir un string. En este caso la tarea 
  // 'handlebars' será configurada basada en el objeto abajo.
  grunt.config.set('handlebars', {
    dev: {
      // Definiremos cuáles archivos de plantilla inyectar
      // en tasks/pipeline.js
      files: {
        '.tmp/public/templates.js': require('../pipeline').templateFilesToInject
      }
    }
  });

  // cargar el módulo npm para handlebars.
  grunt.loadNpmTasks('grunt-contrib-handlebars');
};
```

* Reemplazar el path hacia los archivos fuentes en asset pipeline. El único cambio aquí será que handelbars buscará archivos con la extensión .hbs mientras plantillas underscore pueden estar en archivos html simples.

```javascript
// tasks/pipeline.js
// --------------------------------
// asset pipeline

var cssFilesToInject = [
  'styles/**/*.css'
];

var jsFilesToInject = [
  'js/socket.io.js',
  'js/sails.io.js',
  'js/connection.example.js',
  'js/**/*.js'
];

// Cambiamos este patrón global para incluir todos los archivos en
// el directorio templates/ que termina con la extesión .hbs
var templateFilesToInject = [
  'templates/**/*.hbs'
];

module.exports = {
  cssFilesToInject: cssFilesToInject.map(function(path) {
    return '.tmp/public/' + path;
  }),
  jsFilesToInject: jsFilesToInject.map(function(path) {
    return '.tmp/public/' + path;
  }),
  templateFilesToInject: templateFilesToInject.map(function(path) {
    return 'assets/' + path;
  })
};
```

* Incluir la tarea handlebars en las tareas registradas compileAssets and syncAssets. Aquí es donde la tarea jst estuvo siendo usada y vamos a reemplazarla con la nueva tarea configurada handlebars.

```javascript
// tasks/register/compileAssets.js
// --------------------------------
// compile assets registered grunt task

module.exports = function (grunt) {
  grunt.registerTask('compileAssets', [
    'clean:dev',
    'handlebars:dev',       // cambio de tarea jst task a tarea handlebars
    'less:dev',
    'copy:dev',
    'coffee:dev'
  ]);
};

// tasks/register/syncAssets.js
// --------------------------------
// synce assets registered grunt task

module.exports = function (grunt) {
  grunt.registerTask('syncAssets', [
    'handlebars:dev',      // changed jst task to handlebars task
    'less:dev',
    'sync:dev',
    'coffee:dev'
  ]);
};
```

* Elimina el archivo de configuración jst. No será utilizado más, de manera que puedes eliminar `tasks/config/jst.js`. Simplemente bórralo de tu proyecto.

> Idealmente deberías borrarlo de tu proyecto y del nodo de dependencias del proyecto. Esto puede lograrse ejecutando el siguiente comando en la terminal.
```bash
npm uninstall grunt-contrib-jst --save-dev
```

### Disparadores de Tareas 

En [development mode](http://sailsjs.org/documentation/reference/sails.config/sails.config.local.html?q=environment), Sails ejecuta la tarea `default` ([`tasks/register/default.js`](http://sailsjs.org/documentation/anatomy/myApp/tasks/register/default.js.html)).  Esto compila LESS, CoffeeScript, y plantillas client-side JST, luego las enlaza automáticamente desde las vistas dinámicas de tu aplicación y páginas HTML státicas.

En producción, Sails ejecuta la tarea `prod` ([`tasks/register/prod.js`](http://sailsjs.org/documentation/anatomy/myApp/tasks/register/prod.js.html)) el cual comparte los mismos deberes que `default`, pero también mignifica los scripts y hojas de estilos de tu aplicación.  Esto reduce el tiempo de carga de la aplicación y uso de ancho de banda.

Dichos disparadores son [tareas "básicas" de Grunt](http://gruntjs.com/creating-tasks#basic-tasks) localizadas in la carpeta [`tasks/register/`](http://sailsjs.org/documentation/anatomy/myApp/tasks/register).  Abajo, encontrarás la referencia completa e todos los disparadores en Sails, y el comando el cual los inicializa:

##### `sails lift`

Ejecuta la tarea **default** (`tasks/register/default.js`).

##### `sails lift --prod`

Ejecuta la tarea **prod** (`tasks/register/prod.js`).

##### `sails www`

Ejecuta la tarea **build** (`tasks/register/build.js`) que compila todos los assets en la subcarpeta `www` en vez de `.tmp/public` usando los paths relativos en referencias. Esto permite servir contenido estático con Apache o Nginx en lugar de pasarlos por ['www middleware'](http://sailsjs.org/documentation/concepts/Middleware).

##### `sails www --prod` (production)

Ejecuta la tarea **buildProd** (`tasks/register/buildProd.js`) que hace la misma tarea que  **build** pero también optimiza assets.

Puedes ejecutar otras tareas especificando NODE_ENV y creando una lista de tareas en tasks/register/ con el mismo nombre.  Por ejemplo, si NODE_ENV es QA, sails ejecutará tasks/register/QA.js si existe.


<docmeta name="displayName" value="Task Automation">
