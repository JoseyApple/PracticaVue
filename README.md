Bienvenido a mi primer proyecto de Vue, hecho por Jose Manuel de la Cruz 2ºDAW
Aplicación de Lista de Libros
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

const librosFiltrados = computed(() => {
  return libros.value
    .filter(libro => generoSelect.value === "todas" || 
                     libro.book.genre.toLowerCase().includes(generoSelect.value.toLowerCase()))
    .filter(libro => libro.book.pages <= maximoPaginas.value)
    .filter(libro => libro.book.title.toLowerCase().includes(inputDeBusqueda.value.toLowerCase()));
});

Como es computed, esta cambia siempre que haya cambios en la lista de datos de libros.value.. de esta manera, cuando agregamos o movemos libros de lista en lista, nuestro objetivo es manipular libros.value para que
computed lo detecte y así se ejecute de nuevo la función que causará los cambios esperados por el usuario en la renderización de la lista.

Como ejemplo:

const agregarALectura = (libro) => {
  if (!listaDeLectura.value.some((b) => b.book.ISBN === libro.book.ISBN)) {
    listaDeLectura.value.push(libro);
    libros.value = libros.value.filter((b) => b.book.ISBN !== libro.book.ISBN);
    localStorage.setItem("listaDeLectura", JSON.stringify(listaDeLectura.value));  
  }
};

La condición es por medidas de seguridad, en caso de problemas se sincronización entre nuestra sesión y el localStorage. 

Pusheamos nuestro libro a la listaDeLectura y para modificar la lista de libros, hacemos un filtro inverso para devolver todos los libros menos en que acabamos de añadir a la otra lista. Ahora nuestro computed será capaz de detectar
el cambio y realizar la renderización con un bucle de v-for

















