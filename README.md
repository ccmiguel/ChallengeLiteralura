# Proyecto de Consumo de API y Gestión de Datos Literarios

Este proyecto está orientado a la recuperación de información literaria de la API de Gutenberg (Gutendex), permitiendo acceder a detalles de libros y autores mediante consultas específicas.

## Descripción del Proyecto

La aplicación obtiene datos de libros, autores y otros detalles literarios a través de solicitudes a la API de Gutenberg. El objetivo es procesar y visualizar información relacionada con autores como Miguel de Cervantes, usando un modelo de objetos en Java y Spring Boot.

## Tecnologías Utilizadas

- **Java 21**
- **Spring Boot**
- **Jackson (para la manipulación de JSON)**
- **PostgreSQL (base de datos)**
- **JDBC** para la conexión con la base de datos

## Funcionalidad

1. **Consumo de API de Gutenberg:** Se realiza una solicitud HTTP a la API de Gutendex, que devuelve un conjunto de libros y autores.
2. **Deserialización de JSON:** El JSON obtenido de la API se convierte en objetos Java utilizando Jackson.
3. **Filtrado de Datos de Autores:** La información de los autores es extraída y mostrada separadamente de los libros.
4. **Gestión de Base de Datos:** Se gestionan las tablas y relaciones en una base de datos PostgreSQL para almacenar información sobre autores y libros.

## Estructura del Proyecto

- **`ConsumoAPI`**: Clase encargada de realizar solicitudes HTTP a la API de Gutenberg.
- **`ConvierteDatos`**: Clase para convertir el JSON obtenido de la API en objetos Java utilizando Jackson.
- **`ChallengeLiteraluraApplication`**: Clase principal que corre la aplicación Spring Boot.
- **Modelos**:
  - **`Libro`**: Representa un libro con su título, autores, formatos disponibles, etc.
  - **`Autor`**: Contiene la información sobre los autores, como el nombre, año de nacimiento y muerte.

## Pasos para Ejecutar el Proyecto

### Requisitos previos:

- Tener instalado Java 17 y Spring Boot en tu sistema.
- PostgreSQL configurado para la base de datos.

### Instrucciones

1. Clona el repositorio del proyecto.
2. Configura la base de datos PostgreSQL y asegúrate de tener las tablas necesarias.
3. Ejecuta la aplicación Spring Boot con el comando `mvn spring-boot:run` desde la terminal.
4. La aplicación hará solicitudes a la API de Gutenberg, deserializará la información y la mostrará por consola.

## Gestión de Base de Datos

En este proyecto, utilizamos PostgreSQL para almacenar la información de los autores y libros. La relación entre ambas tablas se establece mediante una clave foránea, asegurando que cada libro tenga un autor asociado.

### Solución de Problemas en la Base de Datos

**Problema:**
Al intentar agregar una clave foránea entre las tablas `libros` y `autores`, podrías encontrar un error si la columna `id` en la tabla `autores` no está definida como clave primaria o no tiene una restricción `UNIQUE`.

**Solución:**
A continuación, te mostramos los pasos para asegurarte de que la columna `id` de la tabla `autores` sea una clave primaria o tenga una restricción `UNIQUE`.

### Paso 1: Verifica si `id` es clave primaria o tiene restricción `UNIQUE`

Ejecuta este comando para revisar la definición de la tabla `autores`:

```sql
\d autores
```

Busca si la columna `id` está definida como clave primaria o si tiene una restricción `UNIQUE`. Si no es así, sigue al paso 2.

### Paso 2: Agrega una clave primaria o restricción `UNIQUE` a `id`

Si `id` no tiene una restricción `UNIQUE` ni es clave primaria, ejecuta uno de los siguientes comandos:

#### a) Si la columna `id` ya existe pero no es clave primaria:

Agrega la restricción `UNIQUE` o `PRIMARY KEY` a la columna `id`:

```sql
ALTER TABLE autores
ADD CONSTRAINT autores_pk PRIMARY KEY (id);
```

#### b) Si no tienes una columna `id`:

Si la tabla `autores` no tiene una columna `id`, agrégala y conviértela en clave primaria. Por ejemplo:

```sql
ALTER TABLE autores
ADD COLUMN id SERIAL PRIMARY KEY;
```

Esto creará automáticamente una columna `id` con valores autoincrementales y establecerá la clave primaria.

### Paso 3: Reintenta agregar la clave foránea

Después de asegurarte de que `autores.id` sea una clave primaria o tenga una restricción `UNIQUE`, ejecuta nuevamente el comando para agregar la clave foránea:

```sql
ALTER TABLE libros 
ADD CONSTRAINT fk_autor 
FOREIGN KEY (autor_id) 
REFERENCES autores (id);
```

### Confirmación:

Para validar que la clave foránea se ha creado correctamente, ejecuta:

```sql
SELECT conname AS constraint_name, conrelid::regclass AS table_name
FROM pg_constraint
WHERE conname = 'fk_autor';
```

Esto debería mostrarte que la clave foránea `fk_autor` ahora está asociada correctamente con las tablas `libros` y `autores`.

## Contribuciones

Si deseas contribuir al proyecto, sigue estos pasos:

1. Haz un fork del repositorio.
2. Crea una rama para tus cambios (`git checkout -b feature/nueva-caracteristica`).
3. Realiza los cambios y haz commit (`git commit -am 'Agrega nueva funcionalidad'`).
4. Empuja tus cambios a tu repositorio (`git push origin feature/nueva-caracteristica`).
5. Abre un pull request en este repositorio.

## Licencia

Este proyecto está bajo la licencia MIT. Para más detalles, consulta el archivo LICENSE.

