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


Ejemplo de estructura de un libro en books.json por si al usuario le interesa añadir más libros. Sigue la estructura presentada para hacerlo exitósamente.

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


![image](https://github.com/user-attachments/assets/7a10898a-afa3-419a-bcb9-94c8231db6a2)
En primer lugar, al entrar a la página web por primera vez podemos observar que hay 17 libros disponibles; 
Al mover el filtro de páginas a la derecha..
![image](https://github.com/user-attachments/assets/7c14a3b0-2825-4bb7-ada0-7ea38a2cabd9)
Podemos ver que hay más libros disponibles si queremos consultar los que tienen más páginas.
![image](https://github.com/user-attachments/assets/ebdcca58-1ba0-4030-af28-efbe601c284f)
También tenemos a nuestra disposición un filtro de géneros, y a la derecha, donde se encuentra el emoticono de una lupa, podemos hacer consultar personalizadas.
![image](https://github.com/user-attachments/assets/86be7bb6-e5dc-4c32-8d28-6b665967e926)
![image](https://github.com/user-attachments/assets/7885c72c-4853-4830-b7de-ae1f366b95af)
Como puede observar, la aplicación adapta el buscador para ignorar mayúsculas, y tampoco tiene que ser precisa la búsqueda, ya que busca por nombres similares.

Ahora, vamos a añadir libros a nuestra lista de lectura, para ello, hacemos clic sobre la cobertura del libro que queremos añadir y se mueve a la derecha, donde se encuentra
la otra lista; Al mismo tiempo, si nos hemos equivocado, podemos quitar el libro de la lista de lectura haciendo clic sobre él. Como demostración.. usaremos el filtro de género
para buscar los libros de fantasía.
![image](https://github.com/user-attachments/assets/46ef00e3-59c4-4e7a-85f5-8fb0289d1e6b)
Añadimos Harry Potter y la piedra filosofal y Juego de tronos.
![image](https://github.com/user-attachments/assets/41213b42-ca5a-41a3-8661-fef1815c75b9)
Si hacemos clic sobre ellos en la lista de lectura, estos volverán a la lista de disponibles siempre y cuando no hayamos cambiado de filtro;
![image](https://github.com/user-attachments/assets/4ad99836-75d7-44d8-a567-b308c71949d0)
Como puede observar en la imagen, hemos cambiado el filtro para Ciencia ficción, aparecen libros acorde con ese género, aún así si retiramos los libros de fantasía de la lista de lectura..
![image](https://github.com/user-attachments/assets/6a4bc3ac-112e-4868-84b3-3cf2ff5e3816)
Estos vuelven a sus categorías sin haber conflictos con los libros disponibles según las necesidades del usuario.

Por último, la aplicación, por si alguna razón cerrastes la pestaña del navegador, u hay otra sesiones del mismo equipo.. tiene la funcionalidad de guardar tu lista de lectura

![image](https://github.com/user-attachments/assets/08f717b1-a339-4aa5-81a2-340e6ca66a72)

Como ejemplo, hemos añadido Juego de tronos a nuestra lista de lectura. Si reiniciamos la página..

![image](https://github.com/user-attachments/assets/c032a87c-88e8-45c7-b883-afd92ab48f0a)

Los filtros se limpian pero la lista de lectura se mantiene.

Gracias por su atención, Jose Manuel 2º de DAW.














