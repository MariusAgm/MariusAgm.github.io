No les quería subir el archivo de Excel y exponer las calificaciones de todos ante los demás. Pero entregarlas de manera individual es demasiado trabajo y las probabilidades de error son muchas. ¿Cómo entregar las calificaciones de manera individual por correo?

Si tu eres como yo, probablemente tienes las calificaciones de tus alumnos en un archivo de Excel que extrae las calificaciones que se generan a partir de las entregas en el sistema de gestión de clases (Moodle o Google Classroom). Si es así, este tutorial te puede servir.

Lo único que necesitas es una cuenta de developer de Google, un correo de Google (tal vez te convenga que no sea el correo personal) y tu lista de calificaciones en una hoja de cálculo de Google Sheets. Puedes subir el archivo en Excel y guardarlo en Google Drive como hoja de cálculo de Google.

### Prepara el archivo Excel

Necesitamos que en una columna del archivo esté el nombre del alumno (preferible), el correo electrónico (indispensable) y las calificaciones en una columna aparte. Recuérdale a tus alumnos que actualicen su dirección de correo y se aseguren de que es correcta.

En una columna puedes generar el mensaje que quieres que incluya el correo. Te muestro un ejemplo de cómo se ve la fórmula que yo usé para hacer el mensaje:

```
=CONCATENATE("Hola ",A2,". Espero primeramente que te encuentres muy bien. Tu calificación final es de ", ROUND(AB2,2), ", según los archivos del SUV y el modelo de calificación que acordamos al inicio de la clase. En esta consideración tu estatus es ",AD2)
```
Cambia `A2` y el resto de las celdas por las que contienen la información que necesitas, como el nombre, la calificación y otros detalles. Por ejemplo, en la celda `AD2` yo expreso si el alumno está reprobado, aprobado, exento, u otras características que dependen de la calificación únicamente. Todas estas fueron generadas con fórmula, de tal modo que no tuve que escribirlo yo directamente, ahorrandome la probabilidad de errores en el mensaje.


### Scripts en Google


Abre un nuevo script en [scripts.google.com](scripts.google.com). Si necesitas ayuda puedes ver el tutorial en la [página de desarrolladores de Google](https://developers.google.com/apps-script/overview). 

Escribe el siguiente código

```
/**
 * Sends emails with data from the current spreadsheet.
 */
function sendEmails() {
  var sheet = SpreadsheetApp.openById("id_de_la_hoja_de_calculo").getSheets()[0];
  //var sheet = SpreadsheetApp.getActiveSheet();
  var startRow = 2; // First row of data to process
  var numRows = 38; // Number of rows to process
  // Fetch the range of cells A2:B3
  var dataRange = sheet.getRange(startRow, 1, numRows, 30);
  // Fetch values for each row in the Range.
  var data = dataRange.getValues();
  for (var i in data) {
    var row = data[i];
    var emailAddress = row[2]; // Third column
    var message = row[28]; // Columna 29
    var subject = 'Calificaciones';
    MailApp.sendEmail(emailAddress, subject, message);
  }
}
```

Cambia el ID de la hoja de cálculo de Google. Esa la puedes identificar en el navegador cuando la hoja está abierta. Abre la barra de navegación e identifícala. Es lo que viene después de `/d/`. Por ejemplo

```https://docs.google.com/spreadsheets/d/id_de_la_hoja_de_calculo/edit```

Cambia también la fila de inicio `startRow` y el número de filas `numRows`. En mi caso requiero de 38 filas porque son el número de alumnos que tengo en ese grupo y estoy saltando la fila de los nombres de variable.

Sólo queda modificar el ciclo `for`, especificando la fila en la que se encuentra cada cosa. Es importante recordar que los lenguajes de programación comienzan sus iteraciones en 0, por lo que si deseas extraer la columna 3, necesitas poner `row[2]`; lo mismo aplica para el mensaje, tienes que ubicar la fila en la que se encuentra. En mi caso está en la columna 29, por lo que `row[28]` logró extraerlo. Escribe en `var subject` el título del correo.