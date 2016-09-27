# El archivo `config/local.js`

El archivo `config/localjs` es útil para configurar la aplicación de Sails en tu entorno local(tu laptop por ejemplo). Las configuraciones en este archivo preceden todas las configuraciones, a excepción de las presentes en [.sailsrc](http://sailsjs.org/documentation/concepts/Configuration/usingsailsrcfiles.html). Debido a que se pretende que sea usado en un entorno local, no debería ser agregado al control de versiones(debido a esta razón, el archivo `config/localjs` está agregado por defecto al `.gitignore`). Usa `local.js` para almacenar las configuraciones de la base de datos local, cambiar el puerto al iniciar la aplicación en tu computadora, etc.

Mientras desarrollas tu aplicación, el archivo de configuración debería incluir cualquier opción específica para tu computadora de desarrollo o servidor(contraseñas de la base de datos, etc.) Si estás usando git, ten en cuenta que `config/local.js` está incluido en el `.gitignore` para nuevas aplicaciones de Sails por defecto, por lo tanto no van a ser agregadas a tu repositorio cuando hagas un commit.

Cuando estés listo para desplegar tu aplicación en producción, también puedes usar este archivo para las opciones de configuración en el servidor que se desplegará la aplicación. Sin embargo, para el despliegue en servidores, las variables de entorno suelen ser preferidas. También puedes usar los argumentos en la linea de comandos y el archivo `.sailsrc` como alternativas a `config/local.js` para tus configuraciones de desarrollo local. [Mira el resumen](http://sailsjs.org/documentation/concepts/Configuration) para información general sobre la configuración de Sails.

> **Nota:** Este archivo es incluido en tu .gitignore, por lo que si usas git como tu solución de control de versiones para su aplicación de Sails, ¡ten en cuenta que este archivo no va a ser agregado a tu repositorio coando realices un commit!
>La ventaja de esto es que no compartiras información personal de tu máquina de desarrollo(como contraseñas de la base de datos) en el repositorio. Además, esto previene que otros miembros de tu equipo sobreescriban tu configuración local con la de ellos.

<docmeta name="displayName" value="The local.js file">
