Lo escuchamos todos los días desde que inició su presidencia, pero ¿cómo podríamos obtener una estadística completa sobre lo que se está diciendo en las famosas conferencias mañaneras?

Tenía tiempo que quería hacer algo de web-scrapping y tenía tiempo que quería descargar las mañaneras de AMLO. A lo lejos imaginaba que sería una labor complicada, pero sólo me tomó un script en Python y una tarde lograr descargar 485 archivos en formato txt con las mañaneras del presidente Andrés Manuel López Obrador.

El primer paso fue ir a [lopezobrador.org.mx](lopezobrador.org.mx), donde están alojadas todas las versiones estenográficas de las conferencias matutinas. El segundo paso fue revisar los permisos que tiene la página en robots.txt, el cual me muestra que todo el scrapping en esta página está permitido, hasta el día de hoy.

El tercer paso fue analizar la página en la consola y usar xpath para identificar las rutas de las mañaneras. También es necesario identificar la ruta de los títulos y el texto una vez entrando en la página.

Una vez identificadas estas rutas, sólo fue cuestión de generar un script en Python para que navegara en estas rutas y extrajera la información. Un paso importante del script fue filtrar únicamente las conferencias matutinas, pues la página alberga muchas otras noticias sobre el presidente, e incluso desde antes de que fuera presidente. El script recorre la página principal y las páginas previas que se generan cuando se aumenta el número de posts. En total, el programa tuvo que recorrer 242 páginas para lograr extraer desde las primeras conferencias mañaneras.

Luego, usando requests y lxml, se recorrieron las páginas, se hizo el _parsing_ y se crearon los archivos txt para cada una de las conferencias, las cuales están alojadas en mi disco duro en este momento. Tengo pensado hacer algo de análisis de sentimiento y un poco de estadística descriptiva.

¿Qué tipo de análisis te gustaría que hiciera?