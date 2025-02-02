<template>
  <div id="app-container">
    
    <Almacenamiento @actualizarLista="actualizarLista" />
    <!-- Se encarga de leer y escribir en localStorage y sincronizar la lista de lectura -->

    <FuncionesBasicas :books="libros" :librosDisponibles="librosFiltrados.length" @actualizarFiltros="actualizarFiltros" />

    <main id="main-container">
      <section id="libros-container">
        <div id="libros">
          <div v-for="libro in librosFiltrados" :key="libro.book.ISBN" class="book-card">
            <img :src="libro.book.cover" :alt="'Portada del libro ' + libro.book.title" class="book-cover" @click="agregarALectura(libro)" />
            <h3 class="book-title">{{ libro.book.title }}</h3>
            <p class="book-author">{{ libro.book.author.name }}</p>
          </div>
        </div>
      </section>

      <ListaDeLectura :listaDeLectura="listaDeLectura" :quitarDeLectura="quitarDeLectura" />
    </main>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";
import libraryData from "./assets/books.json";
import "@/assets/styles.css";
import FuncionesBasicas from "@/components/FuncionesBasicas.vue";
import ListaDeLectura from "@/components/ListaDeLectura.vue";  
import Almacenamiento from "@/components/Almacenamiento.vue";

const libros = ref(libraryData.library);
const listaDeLectura = ref([]);  


const actualizarLista = (nuevaLista) => {
  listaDeLectura.value = nuevaLista;
  filtrarLibros(); // 
};

//Esto se encarga de sincronizar la lista de lectura entre pestañas
onMounted(() => {
  const storedLista = localStorage.getItem("listaDeLectura");
  if (storedLista) {
    listaDeLectura.value = JSON.parse(storedLista);
  }
  filtrarLibros();
});

//Usamos la misma lógica que "quitarDeLectura" pero invertida, ya que queremos que aparezcan los libros que no estan en la lista de lectura
const filtrarLibros = () => {
  libros.value = libraryData.library.filter(
    (libro) => !listaDeLectura.value.some((b) => b.book.ISBN === libro.book.ISBN)
  );
};

// Creamos las variables para los filtros de forma reactiva
const generoSelect = ref("todas"); //Por defecto, dejamos "todas" por defecto que es similar al valor del filtro que corresponse de a todas
const inputDeBusqueda = ref("");
const maximoPaginas = ref(1000);


const actualizarFiltros = ({ genero, maximoPaginas: paginas, inputBusqueda }) => {
  generoSelect.value = genero;
  maximoPaginas.value = paginas;
  inputDeBusqueda.value = inputBusqueda;
};

// Usamos computed en vez de watch para que los filtros sean reactivos y se actualicen en tiempo real cuando cambian
const librosFiltrados = computed(() => {
  return libros.value
    .filter(libro => generoSelect.value === "todas" || 
                     libro.book.genre.toLowerCase().includes(generoSelect.value.toLowerCase()))
    .filter(libro => libro.book.pages <= maximoPaginas.value)
    .filter(libro => libro.book.title.toLowerCase().includes(inputDeBusqueda.value.toLowerCase()));
});



const agregarALectura = (libro) => {
  //Si hay un error de sincronización en la lista de lectura, como medida de seguridad, uso some para evitar duplicados, aunque no es la mejor solución
    //En teoría podemos hacer un push directamente a la lista de lectura pero he optado por este mecanismo para evitar posibles errores de sincronización
  if (!listaDeLectura.value.some((b) => b.book.ISBN === libro.book.ISBN)) {
    //Si el libro no se encuentra en la lista de lectura, devuelve true, de lo contrario devuelve false, por ello si no está, se ejecuta el push
    listaDeLectura.value.push(libro);
    //Como la varible de libros es reactiva, he usado un filtro para quitar el libro que hemos agregado a la lista de lectura a través de su código ISBN
    libros.value = libros.value.filter((b) => b.book.ISBN !== libro.book.ISBN);
    localStorage.setItem("listaDeLectura", JSON.stringify(listaDeLectura.value));  
  }
};

const quitarDeLectura = (libro) => {
  //En primer lugar quitamos el libro de la lista de lectura
  listaDeLectura.value = listaDeLectura.value.filter((b) => b.book.ISBN !== libro.book.ISBN);
  //Volvemos anadirlo a la lista de libros
  libros.value.push(libro);
  //Funcion para actualizar el localStorage
  localStorage.setItem("listaDeLectura", JSON.stringify(listaDeLectura.value));
};
</script>

