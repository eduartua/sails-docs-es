![Squiddy reads the docs](http://sailsjs.org/images/squidford_swimming.png)

# Documentación de Sails.js

La documentación oficial de la última versión estable de Sails está en la [rama master](https://github.com/balderdashy/sails-docs) de este repositorio. El contenido de la mayoría de las secciones en el [sitio web oficial  de Sails](http://sailsjs.org) es compilado desde aquí.


## En Otros Idiomas

La documentación de Sails ha sido traducida a diferentes idiomas. La tabla de abajo es una referencia de los proyectos asociados a las traducciones de los cuáles estamos al tanto.

| Idioma                     | [Etiqueta de idomas IETF](https://en.wikipedia.org/wiki/IETF_language_tag)  | Colaboarador(es)        | Repo                               |
| ---------------------------- | ------- | ------------------ | ---------------------------------- |
| Japonés                     | `ja`    | [@kory-yhg](https://github.com/kory-yhg)      | [sails-docs-ja](https://github.com/balderdashy/sails-docs/tree/ja) <br/>(_en vivo en [sailsjs.jp](http://sailsjs.jp)_)
| Español                      | `es`    | [@eduartua](https://github.com/eduartua/) & [@alejandronanez](https://github.com/alejandronanez)   | [sails-docs-es](https://github.com/eduartua/sails-docs-es)
| Portugés Brasileño         | `pt-BR` | [@marceloboeira](https://github.com/marceloboeira) & [@gabrielalmir10](https://github.com/gabrielalmir10)   | [sails-docs-pt-BR](https://github.com/balderdashy/sails-docs/tree/pt-BR)
| Mandarín Taiwanés           | `zh-TW` | [@CalvertYang](https://github.com/CalvertYang)   | [sails-docs-zh-TW](https://github.com/balderdashy/sails-docs/tree/zh-TW)
| Koreano                       | `ko`    | [@sapsaldog](https://github.com/sapsaldog)   | [sails-docs-ko](https://github.com/balderdashy/sails-docs/tree/ko)

> Dado que ahora utilizamos ramas para realizar seguimiento de las diferentes versiones de la documentación de Sails, estamos dejando a un lado el enfoque original de usar ramas para diferentes idiomas. Antes de iniciar un nuevo proyecto de traducción, te pedimos revises la [información actualizada abajo](#Como-puedo-ayudar-a-traducir-la-documentacion)-- el proceso a cambiado un poco.



## Contribuyendo a la documentación de Sails

Agradecemos tu ayuda!  Por favor envía un Pull request a **master** con las correciones/adiciones y serán revisadas hasta dos veces para luego hacer la fusión tan pronto como sea posible.

Segundo, escuchamos sugerencias acerca del proceso que actualmente usamos para gestionar nuestra documentación, y para trabajar con la comunidad en general.  Por favor postea en el Google Group con tus ideas - o si estás interesado en ayudar directamente, contacta a @fancydoilies, @rudeboot, or @mikermcneil en Twitter.

#### ¿Que rama debo editar?

Eso depende del tipo de edición que estés haciendo.  A menudo, estarás haciendo una edición que es relevante a la última versión estable de Sails (por ejemplo, la versión en [NPM](npmjs.org/package/sails)) y querrás editar la rama master de _este_ repo (lo que ves en repositorio por defecto de sails-docs).

Por otro lado, si estás editando algo relacionado a una característica que aún no se ha lanzado, presente en una futura versión; comúnmente como un acompañamiento, una propuesta de alguna nueva característica, un pull request abierto a Sails o un proyecto relacionado, querrás editar la rama para la próxima versión de Sails que aún no se lanzado (algunas veces llamada "edge").


| Rama (en `sails-docs`)                    | Versión de documentación de Sails...                                   | Vista anticipada en...      |
|-------------------------------------------------------------------------------------|------------------------|:-------------------|
| [`master`](https://github.com/balderdashy/sails-docs/tree/master) | [![NPM version](https://badge.fury.io/js/sails.png)](http://badge.fury.io/js/sails) | [preview.sailsjs.org](http://preview.sailsjs.org)
| [`1.0`](https://github.com/balderdashy/sails-docs/tree/1.0) | Upcoming v1.0 release _(branch not available yet)_           | [next.sailsjs.org](http://next.sailsjs.org)
| [`0.11`](https://github.com/balderdashy/sails-docs/tree/0.11) | Sails v0.11.x           | [0.11.sailsjs.org](http://0.11.sailsjs.org)


#### ¿Cómo son esos docs compilados y enviados al sitio Web?

Usamos un módulo llamado `doc-templater` para convertir los archivos .md a html para el sitio web. Puedes aprender más al respecto acerca de como funciona en [el repo doc-templater](https://github.com/uncletammy/doc-templater).

Cada archivo .md tiene su propia página el en sitio web (por ejemplo, los archivos reference, concepts, and anatomy), además deberán incluir una etiqueta especial `<docmeta name="displayName">` con una propiedad `value` especificando el título para la página.  Esto impactará como la página de documentación aparecerá según los resultados en motores de búsqueda y también será usada como su display name en el menú de navegación en sailsjs.org.  Por ejemplo:

```markdown
<docmeta name="displayName" value="Building Custom Homemade Puddings">
```

#### ¿Cuando aparecerán mis cambios en el sitio web de Sails?

Cambios en la documentación se enviarán al sitio en vivo cuando sean fusionados con la rama especial correspondiente con la actual versión estable de Sails (por ejemplo, 0.12). No podemos fusionar pull requests enviados directamente a esta rama -- su único objetivo es reflejar el contenido actualmente hospedado en sailsjs.org, y el contenido es solamente fusionado sólo justo antes del nuevo despliegue a el sitio web de Sails.

Si quieres ver como los cambios en la documentación aparecerán en sailsjs.org, puedes visitar [preview.sailsjs.org](http://preview.sailsjs.org). La vista anticipada del sitio se actualiza por sí misma automáticamente a medida que los cambios son fusionados con la rama master de sails-docs.


#### ¿Cómo puedo ayudar a traducir la documentación?

Una grandiosa forma de ayudar al proyecto Sails, especialmente si hablas nativamente un idioma diferente a Inglés, es ofrecerse como voluntario para traducir la documentación de Sails.  Si estás interesado en colaborar con alguno de los proyectos de traducción listados en la tabla arriba, contacta al colaborador encargado del proyecto de traducción usando las instrucciones en éste README.

Si tu idioma no aparece en la tabla de arriba, y estás interesado en comenzar un nuevo proyecto de traducción, sigue los siguientes pasos:

+ Fork este repo (`balderdashy/sails-docs`) y cambia el nombre de tu fork para que sea `sails-docs-{{IETF}}` donde {{IETF}} es la [etiqueta de idioma IETF](https://en.wikipedia.org/wiki/IETF_language_tag) para tu idioma.
+ Edita el README para resumir tu actual progreso, provee alguna otra información que creas sería de ayuda para otros leyendo tu traducción, y hazle saber a los colaboradores interesados como contactarte.
+ Envía un pull request editando la tabla arriba y agrega un enlace a tu fork.
+ Cuando estés satisfehco with la primera versión completa de tu traducción, abre un issue y alguien de nuestro equipo de documentación estará feliz ed ayudarte para obtener una vista anticipada en el context del sitio web de Sails, desplegarlo en un dominio (tuyo o un subdominio de sailsjs.org, el que sea tenga más sentido), y compartirlo con el resto de la comunidad de Sails.


#### ¿De que otra forma puedo ayudar?

Para más información sobre cómo contribuir a Sails en general, échale un vistazo a la [Guía de Contribución](https://github.com/balderdashy/sails/blob/master/CONTRIBUTING.md).

#### Progreso del proyecto de traducción a español:
Archivos traducidos:

+ sails-docs-es/getting-started/getting-started.md
+ sails-docs-es/README.md

Para interesados en colaborar con este proyecto contactarme por [@eduartua](https://twitter.com/eduartua).