# 🗃️ Contact DB Free

Este repositorio explica cómo usar formularios de Google como base de datos para guardar los mensajes de la sección de contacto de tu página web.

![Screenshot de una sección de contacto de ejemplo.](/assets/contact.png)

> [!IMPORTANT]
> La información la saque de un directo de Twitch de [goncypozzo](https://github.com/goncy) → [Canal de Twitch](https://www.twitch.tv/goncypozzo).

## 🛠️ Requerimientos

📌 Cuenta de **Google**.

## 📝 Paso a Paso

1. Crear un **"Formulario en blanco"** de Google (https://docs.google.com/forms/), tambien puede usar la plantilla de **"Datos de contacto"**.

2. Colocar los campos deseados de contacto. Ejemplo:
    > [!NOTE]
    > La cantidad de campos debe coincidir con los de la sección de contacto de tu página.

    ![Screenshot de un form google de ejemplo.](/assets/formExample.png)

3. En la sección **Respuestas**, selecciona la opción **"Vincular con Hojas de cálculo"**.
  ![Screenshot de un form google en la sección de respuestas.](/assets/formRespuestas.png)
  
    En esta **Hoja de cálculo** se guardarán todos los mensajes enviados desde la sección de contacto de tu página.
    > [!NOTE]
    > Cada columna representa un campo del formulario, además de una columna con la marca temporal del mensaje.

    ![Screenshot de un excel de google.](/assets/excel.png)

4. Luego en la sección **Configuración**, dentro de **Respuestas**, desactiva la opción **"Limitar a 1 respuesta"**.

    ![Screenshot de un form google en la sección de configuración.](/assets/formConfig.png)

5. Luego, en las opciones junto al botón **Enviar** (arriba a la derecha), selecciona la opción **"Obtener enlace previamente rellenado"**.

    ![Screenshot de un form google en la sección de configuración 2.](/assets/formConfig2.png)

6. Después del paso anterior, se abrirá una nueva pestaña para completar los campos del formulario y generar un enlace. Ingresa cualquier valor en todos los campos (diferenciando cada uno) y selecciona **"Obtener enlace"**, luego elige la opción **"COPIAR ENLACE"**.

    ![Screenshot de un form google en la sección de configuración en el apratado de Link](/assets/formLink.png)

7. Pega el enlace en un bloc de notas o en Visual Studio Code para editarlo:

    El enlace será como este:
    ```
    https://docs.google.com/forms/d/e/********/viewform?usp=pp_url&entry.2005620554=nombreEjemplo&entry.1045781291=emailEjemplo@gmail.com&entry.839337160=comentariosEjemplo
    ```
    Debes reemplazar la palabra *'viewform'* por **'formResponse'** y, después del *'?'*, agregar **'submit=Submit&'**, quedando así:
    > [!NOTE]
    > Con esta modificación, al realizar un fetch con este enlace, se enviará la información.
    ```
    https://docs.google.com/forms/d/e/********/formResponse?submit=Submit&usp=pp_url&entry.2005620554=nombreEjemplo&entry.1045781291=emailEjemplo@gmail.com&entry.839337160=comentariosEjemplo
    ```
    Ten en cuenta que cada campo del formulario está representado por un 'entry' seguido de un número correspondiente (identificador del campo) y del valor del campo ingresado en el paso anterior.
    
    Por ejemplo, el parámetro *'entry.2005620554=nombreEjemplo'* corresponde al campo **'Nombre'** de nuestro formulario. Al modificar *'nombreEjemplo'* por el nombre de contacto que los usuarios ingresan en nuestra página, podremos registrarlo en la hoja de cálculo creada anteriormente.

8. Por último, creamos una función que modifique el **enlace** generado en el punto anterior con los datos que el usuario ingrese en nuestra página. Por ejemplo:

    > [!IMPORTANT]
    > Desafortunadamente, el fetch siempre devuelve un error de CORS, sin importar el resultado de la petición. Por eso, utilizo un catch para saber cuándo finaliza el fetch y mostrar un mensaje en la consola.
    ```
    function guardarContacto (nombre, email, comentario) {
      fetch(`https://docs.google.com/forms/d/e/********/formResponse?submit=Submit&usp=pp_url&entry.2005620554=${nombre}&entry.1045781291=${email}&entry.839337160=${comentario}`)
        .catch(() => {
          console.log('Contacto Guardado!')
        })
    }
    ```
    A esta función le pasamos el **nombre**, el **email** y el **comentario** que el usuario ingresó en nuestra página web.

9. ¡Listo! Ahora tenemos una **base de datos** que almacena los datos de contacto. Cuando un usuario de nuestra página envía la información a través de la **función** creada en el punto anterior, se guarda en la **Hoja de cálculo** que creamos en el paso 3. Por ejemplo:

    ![Screenshot de un form google en la sección de configuración en el apratado de Link](/assets/excel2.png)

## 👨‍💻 Ejemplos
📌 [index.html](/example/index.html)