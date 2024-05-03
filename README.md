Diseñando el conversor

Creamos un directorio para el proyecto Convertidor de monedas 

Creamos un fichero index.html.

Autocompletamos con ! + tab y abrimos en Live Server.

Cambiamos el title del HTML a convertidor de monedas.

Añadimos la estructura base al body:

index.html

Añadiendo estilos CSS
Creamos los estilos mientras añadimos los identificadores:
index.html
Y para la css dentro de un nuevo tag style antes de cerrar la cabecera head:

Luego trasladamos los estilos a un fichero styles.css separado y lo importamos en la cabecera:

<link rel="stylesheet" href="styles.css" type="text/css">
Accediendo a los inputs
Comprobaremos este enlace a la API con el valor actual de diferentes divisas respecto al euro.

Crearemos un campo dataset en cada input con el valor al cambio con el nombre que queramos:

index.html


Crearemos un script para establecer los valores iniciales de los inputs usando el método forEach que tienen los arrays en JavaScript:
scripts.js
Finalmente trasladamos el código JavaScript a un fichero scripts.js separado y lo cargamos justo antes de cerrar el body:

Muy bien tenemos los valores de la API listos para manipularlos en datos JSON. JSON es el acrónimo de JavaScript Object Notation, así que en realidad lo que tenemos son objetos con sus propiedades que podemos consultar de la forma como os enseñé en mi curso de JavaScript para principiantes.

scripts.js


.then(data => {
  console.log(data.rates);
})
En un nivel más de profundidad tenemos los datos que necesitamos:


.then(data => {
  console.log(data.rates.GBP);
  console.log(data.rates.USD);
  console.log(data.rates.JPY);
})

Este código podría servirnos para usarlo de referencia a la hora de establecer los valores de cambio, pero necesitamos añadirlo a cada input. Podemos hacerlo por ejemlo asignando un atributo id:

index.html


data-clave="USD"
data-clave="GBP"
data-clave="JPY"
En el caso del euro no lo necesitamos porque siempre se toma como base 1 euro al hacer las conversiones.

Ahora simplemente tenemos que hacer referencia a los inputs a partir de esa id y asignarles su nuevo valor de cambio:

scripts.js


.then(data => {
  document.querySelector('#USD')
    .dataset.cambio = data.rates.USD;
  document.querySelector('#GBP')
    .dataset.cambio = data.rates.GBP;  
  document.querySelector('#JPY')
    .dataset.cambio = data.rates.JPY;
})


Ese tiempo es suficiente para que nuestra web muestre los valores de cambio que tenemos establecidos manualmente en los inputs en lugar de los que se cargan de la API.



El detalle final
Para evitar el problema inherente de usar las promesas tendremos que establecer los valores de cambio en los inputs justo después de recibirlos dentro del fetch():
scripts.js


// Nos llevamos esto arriba del todo para reutiliarlo
let inputs = document.querySelectorAll(".valor");

let url = 'https://api.exchangeratesapi.io/latest?symbols=USD,GBP,JPY';
fetch(url)
  .then(r => r.json())
  .then(data => {
    document.querySelector('#USD')
      .dataset.cambio = data.rates.USD;
    document.querySelector('#GBP')
      .dataset.cambio = data.rates.GBP;  
    document.querySelector('#JPY')
      .dataset.cambio = data.rates.JPY;

    // Recorremos y establecemos los valores aquí en lugar de abajo
    inputs.forEach(input => {
      input.value = input.dataset.cambio;
    });
  })
  .catch(error => console.error(error))
Con esto podemos borrar el dataset data-cambio de todos los inputs exceputando el del euro, ya que es el único que debe tener un valor manual de 1 porque no se recibe de la API.

index.html


data-cambio="1"  <!-- dejar sólo
