Bienvenido a mi primer proyecto de Vue, hecho por Jose Manuel de la Cruz 2ºDAW
Aplicación de Lista de Libros
Enlace de la página web: https://listadelibrosbyjosy.netlify.app/
```
PracticaVue/
├── src/
│   ├── assets/
│   │   ├── books.json        # Datos de los libros
│   │   ├── styles.css        # Estilos globales
│   ├── components/
│   │   ├── FuncionesBasicas.vue  # Filtros y búsqueda
│   │   ├── ListaDeLectura.vue    # Gestión de la lista de lectura
│   │   ├── Almacenamiento.vue    # Sincronización con localStorage
│   ├── App.vue                # Contenedor principal 
│   ├── main.js                # Archivo principal de Vue
├── public/
│   ├── index.html             # Plantilla base
├── package.json               # Dependencias del proyecto
├── vite.config.js             # Configuración de Vite

```


Ejemplo de estructura de un libro en books.json

```json
{
  "library": [
    {
      "book": {
        "title": "El Señor de los Anillos",
        "pages": 1200,
        "genre": "Fantasía",
        "cover": "https://...",
        "author": {
          "name": "J.R.R. Tolkien"
        }
      }
    }
  ]
}
```
La aplicación renderiza en un contenedor principal una lista de libros junto con su portadas, donde, con un evento de clic, podemos moverlas a otro contenedor de lista de lectura;
Esta tiene un evento similar donde pueden volver a su estado original en la lista de libros de contenedores sin conflicto de género o páginas.

Otras funcionalidades de la aplicación es la búsqueda por nombre, filtro de páginas, filtro de género, y un contador que muestra el total de libros disponibles según la lista de libros que se enseña al usuario.

Para explicar bien el código, tenemos la función:
```
const librosFiltrados = computed(() => {
  return libros.value
    .filter(libro => generoSelect.value === "todas" || 
                     libro.book.genre.toLowerCase().includes(generoSelect.value.toLowerCase()))
    .filter(libro => libro.book.pages <= maximoPaginas.value)
    .filter(libro => libro.book.title.toLowerCase().includes(inputDeBusqueda.value.toLowerCase()));
});
```
Como es computed, esta cambia siempre que haya cambios en la lista de datos de libros.value.. de esta manera, cuando agregamos o movemos libros de lista en lista, nuestro objetivo es manipular libros.value para que
computed lo detecte y así se ejecute de nuevo la función que causará los cambios esperados por el usuario en la renderización de la lista.

Como ejemplo:
```
const agregarALectura = (libro) => {
  if (!listaDeLectura.value.some((b) => b.book.ISBN === libro.book.ISBN)) {
    listaDeLectura.value.push(libro);
    libros.value = libros.value.filter((b) => b.book.ISBN !== libro.book.ISBN);
    localStorage.setItem("listaDeLectura", JSON.stringify(listaDeLectura.value));  
  }
};
```
La condición es por medidas de seguridad, en caso de problemas se sincronización entre nuestra sesión y el localStorage. 

Pusheamos nuestro libro a la listaDeLectura y para modificar la lista de libros, hacemos un filtro inverso para devolver todos los libros menos en que acabamos de añadir a la otra lista. Ahora nuestro computed será capaz de detectar
el cambio y realizar la renderización con un bucle de v-for
```
<div id="libros">
          <div v-for="libro in librosFiltrados" :key="libro.book.ISBN" class="book-card">
            <img :src="libro.book.cover" :alt="'Portada del libro ' + libro.book.title" class="book-cover" @click="agregarALectura(libro)" />
            <h3 class="book-title">{{ libro.book.title }}</h3>
            <p class="book-author">{{ libro.book.author.name }}</p>
          </div>
        </div>
```

Usaremos la misma maniobra para renderizar los libros de lectura de esta manera con unos ligeros cambios:
```
 <div id="lectura">
      <!-- Si la lista de lectura esta vacía, no se muestran libros. Y indica en el contenedor que no hay libros en la lista de lectura -->
      <div v-if="listaDeLectura.length === 0" class="no-books">
        No hay libros en la lista de lectura.
      </div>
      <!-- Hacemos un bucle for para mostrar los libros en la lista de lectura de la misma manera que hicimos en App.vue -->
      <div v-for="libro in listaDeLectura" :key="libro.book.ISBN" class="book-card">
        <img :src="libro.book.cover" :alt="'Portada del libro ' + libro.book.title" class="book-cover" @click="quitarDeLectura(libro)" />
        <h3 class="book-title">{{ libro.book.title }}</h3>
        <p class="book-author">{{ libro.book.author.name }}</p>
      </div>
    </div>
```

Con una condición de que si la lista esta vacía, aparezca el texto de que no hay libros.. en caso contrario, este desaparece ya que no cumple la condición si hay libros, y renderizamos los libros
a partir del evento de clic de agregarALectura. Ahora.. en este caso, tenemos que usar una función diferente que tenga un comportamiento similar.

```
const quitarDeLectura = (libro) => {
  //En primer lugar quitamos el libro de la lista de lectura
  listaDeLectura.value = listaDeLectura.value.filter((b) => b.book.ISBN !== libro.book.ISBN);
  //Volvemos anadirlo a la lista de libros
  libros.value.push(libro);
  //Funcion para actualizar el localStorage
  localStorage.setItem("listaDeLectura", JSON.stringify(listaDeLectura.value));
};
```

Tal y como se describe en los comentarios, siendo el renderizado de libros computed, cada cambio que hagamos en la lista de libros, se detectará automáticamente.

Y por último hablaremos de FuncionesBasicas.vue, aquí se ubican:

  Número de libros disponibles
  Filtro por género
  Filtro por input de búsqueda
  Filtro por páginas

Para el número de libros disponibles, en el elemento padre tenemos:
```
<FuncionesBasicas :books="libros" :librosDisponibles="librosFiltrados.length" @actualizarFiltros="actualizarFiltros" />
```

Necesito definir los props en FuncionesBasicas.vue
```
defineProps({
  books: Array,
  librosDisponibles: Number, // Recibimos librosDisponibles como número
});
```

Y por último, llamando simplemente a la variable..
```
<h1>{{ librosDisponibles }} Libros disponibles</h1>
```

Se renderiza exitósamente los libros disponibles con cada cambio que se haga en la lista de libros por los filtros o demás acciones..

Ahora, los tres filtros, definidos de esta manera:
```
const genero = ref("todas");
const maximoPaginas = ref(1000);
const inputBusqueda = ref("");
```

Usaremos: 
```
const emit = defineEmits(["actualizarFiltros"]);

watch(
  () => [genero.value, maximoPaginas.value, inputBusqueda.value],
  () => {
    emit("actualizarFiltros", { //Actualiza los datos actualizados a otro componente..
      genero: genero.value,
      maximoPaginas: maximoPaginas.value,
      inputBusqueda: inputBusqueda.value
    });
  }
);
```

Para que podamos actualizar los datos en el otro componente, es decir, en App.vue ya que como hemos indicado antes, el filtro de libros en general comprueba estas variables constantemente.

Watch se encarga de vigilar los valores de genero, maximoPaginas y inputBusqueda, y si resibe algun cambio, se ejecuta emit, y manda los datos al elemento padre App.vue

Y con esto ya hemos explicado cómo funciona nuestra aplicación, funciones sencillas donde movemos libros de una lista a otra. 

-----------------SUGERENCIAS------------------

El código es mejorable, en optimización, además de que no tiene mucho sentido que las variables del filtro aparezcan en otro componente pero por necesidades de LocalStorage y mantener la sincronización he sacrificado un poco la limpieza y buenas prácticas con tal que funcionase correctamente. En un futuro se podría mejorar la página web pero todavía estoy aprendiendo Vue, queda mucho por aprender de este framework.

Gracias por su atención, Jose Manuel 2º de DAW.














