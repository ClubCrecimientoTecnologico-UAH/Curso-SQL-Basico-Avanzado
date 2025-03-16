
# Relaciones 

Uno de los aspectos más importantes de las bases de datos relacionales es su capacidad para **relacionar información** entre diferentes tablas. Una **relación** es una asociación lógica entre dos o más tablas, lo que permite conectar datos de manera estructurada y eficiente. Estas relaciones son fundamentales para:
    
Por ejemplo, en una base de datos de un cine tenemos:
        
  - La tabla  `Peliculas`  almacena información sobre las películas.
  - La tabla  `Salas`  almacena información sobre las salas de proyección.
  - La tabla  `Funciones`  relaciona las películas con las salas, indicando en qué sala se proyecta cada película y a qué hora.

La relación entre tablas se crea estableciendo columnas claves que definan esta relación. Estas claves se conocen como **Llave primaria** y **Llave foránea**. 

## Llave Primaria

En toda tabla de una base de datos, es fundamental definir una  **llave primaria (primary key)**, la cual actúa como el  **identificador único**  de cada registro. Este identificador garantiza que cada fila en la tabla sea única y fácilmente referenciable. Por convención, la llave primaria suele ser la  **primera columna**  de la tabla y se identifica con nombres como  **`id`**  o  **`id_<nombre_entidad>`**.

**Tabla películas:**
|id_pelicula | nombre | anio_lanzamiento |
|--|--|--| 
| 1 | Avengers  | 2012 |
| 2 | Avengers: la era de Ultron | 2015 |
| 3 | Avengers: Infinity War | 2018 |
...

En esta tabla, la columna  **`id_pelicula`**  es la  **llave primaria**. Cada película tiene un identificador único:

-   La película  **Avengers**  se identifica con el valor  **1**.
    
-   La película  **Avengers: La era de Ultron**  se identifica con el valor  **2**, y así sucesivamente.
    

Por otro lado, una llave primaria debe cumplir con las siguientes características:

1.  **Única**: No puede haber dos registros con el mismo valor en la columna de la llave primaria. Por ejemplo, no puede existir otra película con el valor  **`id_pelicula = 1`**. Esta unicidad garantiza que cada registro sea identificable de manera inequívoca.
        
2.  **No nula (NOT NULL)**: La llave primaria  **no puede tener valores nulos (NULL)**. Es decir,  **todos los registros deben tener un identificador**. ejemplo, no puede existir una película en la tabla sin un valor en la columna  **`id_pelicula`**.
        
3.  **Estable**: El valor de la llave primaria  **no debe cambiar con el tiempo**. Una vez asignado, el identificador debe permanecer constante durante toda la vida del registro. Por ejemplo, si la película  **Avengers**  tiene el identificador  **1**, este valor no debería modificarse nunca, incluso si cambia el nombre de la película o su año de lanzamiento.

## Llaves Foraneas

Las **llaves foráneas (foreign keys)** son establecen **relaciones entre tablas**. Una llave foránea es una columna (o conjunto de columnas) en una tabla que hace referencia a la **llave primaria** de otra tabla. Esta referencia permite vincular registros entre diferentes tablas evitando la redundancia de datos. 

Por convención, las columnas que actúan como llaves foráneas suelen nombrarse siguiendo el formato **`<nombre_de_la_entidad>_id`**. Por ejemplo, si una tabla hace referencia a la tabla `Peliculas`, la columna de la llave foránea podría llamarse **`pelicula_id`**

Siguiendo con la tabla películas. Tenemos tres películas, cada una con su identificador: 

**Tabla películas:**
|id_pelicula | nombre | anio_lanzamiento |
|--|--|--| 
| 1 | Avengers  | 2012 |
| 2 | Avengers: la era de Ultron | 2015 |
| 3 | Avengers: Infinity War | 2018 |
...

Y tenemos la tabla funciones: 

**Tabla Funciones:**
|id_funcion | hora_inicio | hora_fin  |
|--|--|--| 
| 1 | 11:30  | 1:30 |
| 2 | 15:30 | 17:30 |
| 2 | 14:00 | 16:30 |
...

Tenemos ahora una tabla funciones, pero no sabemos que película se vera en cada función. Se podría crear una columna nombre_película en la tabla **Funciones**, y guardar el nombre de la película, pero esto generaría redundancia de información, ademas de que nada garantiza que el nombre de la película en la tabla  `Funciones`  coincida con el nombre en la tabla  `Peliculas`.

Para evitar estos problemas, se utiliza una **llave foránea** (`pelicula_id`) en la tabla `Funciones`. En lugar de almacenar el nombre de la película, se guarda el **identificador único** (`id_pelicula`) de la película en la tabla `Peliculas`:

**Tabla Funciones:**
|id_funcion | hora_inicio | hora_fin  | película_id |
|--|--|--| -- |
| 1 | 11:30  | 1:30 | 1 |
| 2 | 15:30 | 17:30 | 1 |
| 3 | 16:00 | 18:30 | 2 |
...

Si vemos la funcion con el id 1. Esta tiene en la columna película_id, el identificador 1. Lo que significa que en esa funcion, se vera la película con el identificador 1 de la tabla películas: **Avengers**

Para las funciones con id 2 y 3, se verán las películas con id 1 y 2 respectivamente. Para la función de las 15:30 se verá **Avengers**, pero para la funcion de las 16:00 se verá **Avengers: la era de Ultron**.

Esto que acabamos de hacer, permite **Evitar redundancia**: el nombre de la película se almacena una sola vez en la tabla  `Peliculas`; y **Garantizar integridad**: La llave foránea asegura que solo se puedan registrar funciones para películas que existan en la tabla  `Peliculas`.

### **Integridad Referencial**

La  **integridad referencial**  es un concepto fundamental en las bases de datos relacionales que garantiza la  **consistencia y validez**  de las relaciones entre los registros de diferentes tablas. Se refiere a las reglas y comportamientos que aseguran que las relaciones entre tablas se mantengan correctamente, especialmente ante operaciones como inserciones, actualizaciones o eliminaciones de registros.

Esta tiene dos características principales:  

#### Restricción entre Registros

Cuando se establece una relación entre dos tablas mediante una  **llave foránea**, la base de datos se encarga de asegurar que dicha relación sea válida. Esto significa que:

-   **No se puede insertar un registro en una tabla con una llave foránea que no exista en la tabla referenciada**. Por ejemplo, en la tabla  `Funciones`, no se puede guardar un valor en la columna  `pelicula_id`  (llave foránea) si dicho valor no existe en la columna  `id_pelicula`  (llave primaria) de la tabla  `Peliculas`.
    
-   Si se intenta realizar una operación que viole esta regla, la base de datos  **arrojará una excepción**  y evitará que el registro se inserte o actualice. Por ejemplo, si intentas insertar una función con  `pelicula_id = 5701`, pero no existe una película con  `id_pelicula = 5701`  en la tabla  `Peliculas`, la base de datos rechazará la operación.
    

Este mecanismo asegura que las relaciones entre tablas siempre sean  **consistentes**  y que no haya registros huérfanos o inválidos.

#### **Acciones en Cascada**

En algunos casos, puede surgir la necesidad de  **eliminar o actualizar un registro**  que está siendo referenciado por otros registros en una tabla relacionada. Por ejemplo, si intentas eliminar una película de la tabla  `Peliculas`, pero existen funciones en la tabla  `Funciones`  que dependen de esa película, la base de datos  **bloqueará la operación**  por defecto, ya que no sabe cómo manejar la situación.

Para estos casos, de ser requerido, las  **llaves foráneas**  permiten definir  **comportamientos específicos**  conocidos como  **acciones en cascada**. Estas acciones determinan cómo debe actuar la base de datos cuando se elimina o actualiza un registro al que otros registros dependen.

En primer lugar, tenemos `ON DELETE <RESTRICT | CASCADE>`. Vamos a ilustrar el siguiente ejemplo para explicar cada uno:
```sql
CREATE TABLE productos (
    id integer PRIMARY KEY,
    nombre text,
    precio numeric
);

CREATE TABLE facturas (
    id integer PRIMARY KEY,
    monto_total numeric,
    subtotal numeric,
    producto_id int REFERENCES productos(id) ON DELETE RESTRICT/CASCADE
    ...
);
```

-   Si la  **llave foránea**  se establece con la opción  **`ON DELETE RESTRICT`**, significa que  **no se podrá eliminar un registro en la tabla "producto"**  mientras existan registros relacionados en la tabla "facturas". En otras palabras, la eliminación estará  **restringida**  si hay dependencias entre los registros. Por ejemplo, si intentas eliminar un producto que está asociado a una o más facturas, la base de datos  **bloqueará la operación**  para mantener la integridad referencial.
    
-   Si la  **llave foránea**  se establece con la opción  **`ON DELETE CASCADE`**, significa que  **al eliminar un registro en la tabla "producto"**, también se eliminarán  **automáticamente**  todos los registros relacionados en otras tablas. Por ejemplo, si hay 100 facturas asociadas al producto con  `id = 256`, al eliminar este producto, las 100 facturas también se eliminarán. Este comportamiento se conoce como  **eliminación en cascada**.
    
    Es importante destacar que este efecto en cascada se propaga a través de  **todas las tablas relacionadas**. Por ejemplo, si tienes 10 tablas relacionadas entre sí (la primera con la segunda, la segunda con la tercera, y así sucesivamente hasta la décima), y todas las llaves foráneas están configuradas con  `ON DELETE CASCADE`, al eliminar un registro en la  **tabla 10**, se eliminarán  **todos los registros dependientes**  en las tablas 9, 8, 7, y así hasta la  **tabla 1**. Este comportamiento garantiza que no queden registros huérfanos, pero debe usarse con precaución, ya que puede resultar en la eliminación masiva de datos.
 
Por otro lado, las opciones **`ON DELETE RESTRICT`** y **`ON DELETE CASCADE`**  no son los únicos comportamientos que se pueden definir en una llave foránea. También existen las opciones **`SET NULL`** y **`SET DEFAULT`**, las cuales permiten manejar la eliminación de registros de manera más flexible.

```sql
CREATE TABLE autores (
    id serial PRIMARY KEY ,
    user_id integer NOT NULL
);

CREATE TABLE blog (
    id serial REFERENCES tenants ON DELETE CASCADE,
    autor_id integer NOT NULL,
    FOREIGN KEY (autor_id) REFERENCES autores ON DELETE SET NULL
);
```
- **`ON DELETE SET NULL`**: Si se establece esta opción en una llave foránea,  al eliminar un registro en la tabla referenciada (en este caso,  `autores`), el valor de la columna correspondiente en la tabla dependiente (en este caso,  `blog`) se establecerá automáticamente como  **`NULL`**.
        
   Por ejemplo, si eliminas un autor de la tabla  `autores`, la columna  `autor_id`  en la tabla  `blog`se actualizará a  `NULL`  para todos los registros que estaban asociados a ese autor. Esto permite mantener los registros en la tabla  `blog`, pero sin una referencia válida al autor eliminado.
        
- **`ON DELETE SET DEFAULT`**:  Si se establece esta opción,  al eliminar un registro en la tabla referenciada, el valor de la columna correspondiente en la tabla dependiente se establecerá automáticamente en el  **valor por defecto**  definido para esa columna. Si no se especificó un valor por defecto al crear la columna, PostgreSQL utilizará  **`NULL`**  como valor predeterminado.
        
    Por ejemplo, si defines un valor por defecto para la columna  `autor_id`  (como  `0`), al eliminar un autor, todos los registros en  `blog`  asociados a ese autor tendrán su  `autor_id`  actualizado a  `0`. Si no hay un valor por defecto definido, se establecerá a  `NULL`.

Cuando se utilizan las opciones **`ON DELETE SET NULL`** o **`ON DELETE SET DEFAULT`** en una llave foránea, es importante entender que **PostgreSQL sigue realizando verificaciones de integridad referencial**. Esto significa que, aunque estas opciones permiten manejar la eliminación de registros de manera más flexible, **no evitan las validaciones de la base de datos**.

Por ejemplo:  Supongamos que defines una llave foránea con  **`ON DELETE SET DEFAULT`**  y estableces un valor por defecto para la columna `1`:

```sql
CREATE TABLE autores (
    id serial PRIMARY KEY,
    nombre text NOT NULL
);

CREATE TABLE blog (
    id serial PRIMARY KEY,
    titulo text NOT NULL,
    autor_id integer DEFAULT 1,
    FOREIGN KEY (autor_id) REFERENCES autores(id) ON DELETE SET DEFAULT
);
```
En este caso:  si se elimina un registro de la tabla  `autores`, PostgreSQL intentará actualizar la columna  `autor_id`en la tabla  `blog`  al valor por defecto definido (en este caso,  `1`).
 
 Sin embargo,  **si el registro con  `id = 1`  no existe en la tabla  `autores`**, PostgreSQL  **arrojará una excepción**. Esto se debe a que la base de datos verifica que el valor por defecto (`1`) sea un valor válido en la tabla referenciada (`autores`). Si no existe, se viola la integridad referencial, y PostgreSQL no permitirá la operación arrojando la siguiente excepción:
    
```
ERROR:  insert or update on table "blog" violates foreign key constraint "blog_autor_id_fkey"
DETAIL:  Key (autor_id)=(1) is not present in table "autores".
```
### Importancia de las Relaciones

Las relaciones entre tablas son un pilar fundamental en el diseño de bases de datos relacionales. No solo permiten organizar la información de manera eficiente, sino que también ayudan a  **optimizar el uso de recursos**,  **garantizar la integridad de los datos**  y  **evitar la redundancia**. 

Para entender su importancia, es útil reflexionar sobre los desafíos que enfrentaban los sistemas en el pasado, cuando la capacidad de almacenamiento y memoria era limitada.

#### **Optimización del Espacio de Almacenamiento**

En los inicios de la informática, cuando los recursos de memoria y almacenamiento eran escasos, cada byte de información era valioso. Hoy en día, aunque las computadoras tienen una capacidad mucho mayor, la optimización del espacio sigue siendo relevante, especialmente en sistemas con grandes volúmenes de datos.

Imaginemos un sistema de usuarios donde cada usuario tiene un rol, como  **"ADMIN"**  o  **"USUARIO"**. Una forma sencilla de implementar esto sería crear una columna  `rol`  en la tabla de usuarios y almacenar el rol como una cadena de texto:

```sql
CREATE TABLE Usuarios (
    id SERIAL PRIMARY KEY,
    nombre TEXT,
    rol TEXT  -- "ADMIN" o "USUARIO"
);
```
Sin embargo, este enfoque tiene un problema: el almacenamiento de cadenas de texto consume más espacio que los valores numéricos. Por ejemplo:

-   El texto  **"ADMIN"**  ocupa  **5 bytes**.
-   El texto  **"USUARIO"**  ocupa  **7 bytes**.
    

Si tenemos  **10,000 usuarios**, y solo  **10**  de ellos son administradores, el resto (9,990 usuarios) almacenará el valor  **"USUARIO"**, lo que significa que se están utilizando  **7 bytes por registro**  para almacenar el mismo valor repetidamente.

Para optimizar el espacio, podemos crear una tabla separada para los roles y relacionarla con la tabla de usuarios mediante una llave foránea:

```sql

CREATE TABLE Roles (
    id SMALLSERIAL PRIMARY KEY,  -- SMALLINT para ahorrar espacio
    nombre TEXT  -- "ADMIN" o "USUARIO"
);

CREATE TABLE Usuarios (
    id SERIAL PRIMARY KEY,
    nombre TEXT,
    rol_id SMALLINT REFERENCES Roles(id)  -- Llave foránea
);

```

-   La columna  `rol_id`  en la tabla  `Usuarios`  almacena un valor numérico (`SMALLINT`), que ocupa solo  **2 bytes**.
    
-   Los roles se definen una sola vez en la tabla  `Roles`, evitando la repetición de cadenas de texto.
    

Con este enfoque, se ahorran  **5 bytes por registro**  en comparación con el uso de cadenas de texto.

#### **Garantía de Integridad de los Datos**

Otro beneficio clave de las relaciones es que  **garantizan la integridad de los datos**. Al establecer una relación entre tablas, la base de datos asegura que solo se puedan insertar valores válidos en la columna de la llave foránea.

En el enfoque inicial, donde el rol se almacena como texto, podrían ocurrir errores como:

-   Errores tipográficos:  **"Admin "**  (con un espacio al final) en lugar de  **"ADMIN"**.
    
-   Roles no definidos:  **"Empleado"**  en lugar de  **"USUARIO"**.
    
Estos errores pueden llevar a inconsistencias en los datos y complicar las consultas y el mantenimiento del sistema.

Con el uso de una tabla de roles y una llave foránea, estos problemas se evitan: La columna  `rol_id`  en la tabla  `Usuarios`  solo puede contener valores que existan en la tabla  `Roles`  (por ejemplo,  `1`  para "ADMIN" y  `2`  para "USUARIO"), y  si se intenta insertar un valor no válido (como  `3`), la base de datos  **arrojará una excepción**, impidiendo la operación.
    
#### **Evitar la Redundancia de Información**

Por ultimo: las relaciones también ayudan a  **evitar la redundancia de datos**, lo que simplifica el mantenimiento y reduce el riesgo de inconsistencias.

Supongamos que tenemos una tabla de facturas donde cada factura está asociada a un cliente. Un enfoque incorrecto sería almacenar el nombre del cliente directamente en la tabla de facturas:

```sql

CREATE TABLE Facturas (
    id SERIAL PRIMARY KEY,
    cliente_nombre STRING,  -- Redundancia de datos
    monto DECIMAL
);
```
Este enfoque tiene varios problemas:

-   **Redundancia**: Si un cliente realiza múltiples compras, su nombre se repetirá en varias filas.
    
-   **Inconsistencias**: Si el nombre del cliente cambia, habría que actualizarlo en todas las facturas asociadas.
    
Para evitar estos problemas, podemos crear una tabla separada para los clientes y relacionarla con la tabla de facturas:

```sql

CREATE TABLE Clientes (
    id SERIAL PRIMARY KEY,
    nombre TEXT NOT NULL
);

CREATE TABLE Facturas (
    id SERIAL PRIMARY KEY,
    cliente_id INTEGER REFERENCES Clientes(id),  -- Llave foránea
    monto DECIMAL NOT NULL
);
```
Con este enfoque, el nombre del cliente se almacena una sola vez en la tabla  `Clientes`, y  la tabla  `Facturas`  solo almacena el  `id`  del cliente, evitando la redundancia y garantizando la consistencia.



## Cardinalidad 

Como se vio en la primera clase, uno de la es la capacidad de este tipo de db de relacionar la información con el fin de reducir la redundancia de la información. En este sentido   la  **cardinalidad**  define la cantidad de instancias de una entidad que pueden asociarse con instancias de otra entidad a través de una relación.

Dicho de otro modo, la cárdinalidad indica con cuales y como una entidad se relaciona con otra. Por ejemplo, si se tiene la instancia **Películas**, la cual guarda información sobre las películas disponibles de un **Cine**, podríamos relacionar esta con la entidad **Entradas**, y esta relación nos podria decir que que un entrada se compró para ver cual película. 

Las carnalidad se vera de forma mas practica en clases posteriores, pero por ahora es importante conocer los tipos de cardinalidad:

### **Tipos de cardinalidad:**
    
 ####  **Uno a uno (1:1):**
Es cuando una instancia de la entidad A se relaciona con una sola instancia de la entidad B, y viceversa.

**Ejemplo:**  siguiendo con el ejemplo del cine, si tuviéramos una tabla cajeros y cada cajero solo pudiera tener una taquilla de venta asignada, seria una relación 1 a 1. Una taquilla solo pudiera ser usada por su taquillero, y un taquillero solo pudiera tener una taquilla asignada.

**Ejemplo 2: ** El pasaporte y una persona. Un  `Pasaporte`  pertenece a una única  `Persona`, y una Persona no puede tener dos pasaportes .
            
Este tipo de relaciones en la práctica no es tan común, porque una relación 1 a 1 entre dos entidades se puede representar por general en sola tabla. Sin embargo, dependiendo de la estructura de la DB, si tuviéramos una tabla con muchas columnas, dividirla en dos podria ser una forma de organizar la información.

Por ejemplo: si la empresa necesita guardar la calle, avenida, ciudad, estado, etc de la sucursales de los cines. En lugar de tener una sola tabla sucursales, se podria tener sucursales y sucursales_ubicación.

  #### **Uno a muchos (1:N):**
        
Es el tipo de relación mas común, en donde una instancia de la entidad A se relaciona con varias instancias de la entidad B, pero una instancia de B solo se relaciona con una instancia de A.
            
Ejemplo:   `Entradas` y `funciones`. A la función de una película puede llegar multiples clientes, cada uno con su entrada. Pero una sola entrada no puede servir para ver dos funciones diferentes.

            
   #### **Muchos a muchos (N:M):**
        
Este es el tipo relacion mas complicado, y ocurre cuando una instancia de la entidad A se relaciona con varias instancias de la entidad B, y viceversa.
             
Ejemplo:  `Peliculas` y `Salas`. Una misma película puede ser vista en varias salas en diferentes horarios, y una misma sala durante el día puede reproducir multiples películas.

Ejemplo 2: Productos y Facturas. Una factura puede tener multiples productos, y un producto puede estar en varias facturas ( Facturas de diferentes clientes)

En un diagrama entidad relacional y en la base datos, no es posible  relacionar ambas entidades directamente con solo dos tablas. Porque cada registro de la entidad Películas, tendría solo una columna sala_id, y una entidad de la tabla salas, solo podria tener una columna película_id.

Para solucionar este problema, las relaciones de muchos a muchos se manejan agregando una tabla intermedia entre ambas entidades principales, y tanto  originando dos elecciones uno a muchos. A esta tablas intermedias las llamas con el nombre de las entidades principales, separadas con una x. 

En el ejemplo de Películas y salas:



Al agregar la tabla intermedia, tendríamos:



Como se puede ver, la tabla se nombre Salas_x_peliculas.

**Nota:** Muchas veces, este tipos de tablas se pueden considerar como una entidad mas. Si analizamos lo que esta haciendo esta tabla intermedia `Salas_x_peliculas`, esta define que película se ve en que sala y en que salas se ve que película. Esto se podria llamar en su lugar `funciones_peliculas` y la podemos trabajar como una entidad mas agregando mas columnas si fuera necesario, por ejemplo: `horario`, `hora_inicio` y `hora_fin`
  

Herramientas para elaborar en el d**

DbDiagram

#### Actividades

### Ejercicio 1: Diseño de un Diagrama Entidad-Relación para un Local de Botánica

#### Contexto del Problema

Un local de botánica especializado en la venta de plantas, semillas, herramientas de jardinería y productos naturales necesita una base de datos para gestionar su inventario, ventas, clientes y proveedores. Actualmente, toda la información se maneja de forma manual, lo que ha generado problemas de organización y control. El dueño del local te ha contratado para diseñar una base de datos que le permita:

1.  Registrar y gestionar el inventario de productos.
    
2.  Llevar un control de las ventas realizadas.
    
3.  Gestionar la información de los clientes y proveedores.
    
4.  Registrar las compras de productos a proveedores.
    

#### Actividad

A continuación, se proporciona una lista de  **entidades**, sus  **columnas**  y los  **tipos de datos** correspondientes. Debes usar esta información para diseñar el diagrama entidad-relación (ERD) que solicita el cliente.

----------

#### Entidades y Columnas

1.  **Productos**
    
    -   `id_producto`  (entero, clave primaria)
        
    -   `nombre`  (texto)
        
    -   `descripcion`  (texto)
        
    -   `precio`  (decimal)
        
    -   `stock`  (entero)
        
    -   `id_categoria`  (entero, clave foránea a  `Categorias`)
        
2.  **Categorias**
    
    -   `id_categoria`  (entero, clave primaria)
        
    -   `nombre`  (texto)
        
3.  **Clientes**
    
    -   `id_cliente`  (entero, clave primaria)
        
    -   `nombre`  (texto)
        
    -   `apellido`  (texto)
        
    -   `telefono`  (texto)
        
    -   `email`  (texto)
        
    -   `direccion`  (texto)
        
4.  **Ventas**
    
    -   `id_venta`  (entero, clave primaria)
        
    -   `fecha_venta`  (fecha)
        
    -   `total`  (decimal)
        
    -   `id_cliente`  (entero, clave foránea a  `Clientes`)
        
5.  **Detalle_Ventas**
    
    -   `id_detalle_venta`  (entero, clave primaria)
        
    -   `id_venta`  (entero, clave foránea a  `Ventas`)
        
    -   `id_producto`  (entero, clave foránea a  `Productos`)
        
    -   `cantidad`  (entero)
        
    -   `subtotal`  (decimal)
        
6.  **Proveedores**
    
    -   `id_proveedor`  (entero, clave primaria)
        
    -   `nombre`  (texto)
        
    -   `telefono`  (texto)
        
    -   `email`  (texto)
        
    -   `direccion`  (texto)
        
7.  **Compras**
    
    -   `id_compra`  (entero, clave primaria)
        
    -   `fecha_compra`  (fecha)
        
    -   `total`  (decimal)
        
    -   `id_proveedor`  (entero, clave foránea a  `Proveedores`)
        
8.  **Detalle_Compras**
    
    -   `id_detalle_compra`  (entero, clave primaria)
  
    -   `id_compra`  (entero, clave foránea a  `Compras`)
    
    -   `id_producto`  (entero, clave foránea a  `Productos`)
    -   `cantidad`  (entero)
        
    -   `precio_unitario`  (decimal)

### Ejercicio 2: Diseño de la DB y Diagrama Entidad-Relación para una Academia de Cursos

#### Contexto del Problema

Un cliente, dueño de una academia que ofrece cursos de diversos tipos, se acerca a ti como desarrollador para resolver los problemas de administración que enfrenta su empresa. Desde sus inicios, la academia ha manejado toda su información en hojas de Excel, pero ahora que planea expandirse y abrir más sucursales en la ciudad, necesita  **automatizar sus procesos**  para mejorar la eficiencia y el control de sus operaciones.

El principal problema radica en la gestión de  **pagos**. Los cursos tienen una duración de 3 a 5 meses y los estudiantes deben pagar una mensualidad el primer día de cada mes. Sin embargo, las formas de pago son muy variadas: incluyen transferencias bancarias, efectivo, cheques, transferencias internacionales e incluso criptomonedas. Esta diversidad ha generado un  **descontrol en el registro y seguimiento de los pagos**, lo que dificulta la administración financiera de la academia.

#### Requerimientos del Cliente

En una reunión con el cliente, este especifica que necesita una base de datos que le permita gestionar la siguiente información:

1.  **Estudiantes**: Datos de los alumnos que toman los cursos.
    
2.  **Cursos**: Información sobre los cursos ofrecidos por la academia.
    
3.  **Relación entre estudiantes y cursos**: Registrar qué curso ha comprado cada estudiante.
    
4.  **Maestros**: Datos de los profesores que imparten los cursos.
    
5.  **Inscripciones**: Información sobre las inscripciones de los estudiantes a los cursos.
    
6.  **Pagos**: Registro detallado de los pagos realizados por los estudiantes, incluyendo la forma de pago y la fecha.
    
Además, el cliente menciona que la lista proporcionada es solo una guía inicial, pero  **el diseño final de la base de datos queda a criterio del desarrollador**. Esto significa que, si consideras necesario agregar entidades adicionales para mejorar la funcionalidad del sistema, puedes hacerlo. Por ejemplo, podrías incluir una entidad llamada  `clases`  para gestionar los horarios en los que se imparten los cursos, o una entidad  `sucursales`  para manejar la información de las diferentes ubicaciones de la academia.

#### Actividad

Diseña la  **estructura de la base de datos**  y el  **diagrama entidad-relación (ERD)**  que permita solucionar los problemas descritos por el cliente. Asegúrate de que el diseño cumpla con los siguientes puntos:

1.  **Entidades principales**: Define las entidades clave (como  `Estudiantes`,  `Cursos`,  `Maestros`,  `Inscripciones`,  `Pagos`, etc.) y sus atributos.
    
2.  **Relaciones**: Establece las relaciones entre las entidades (por ejemplo, un estudiante puede inscribirse en varios cursos, y un curso puede tener varios estudiantes).
    
3.  **Atributos adicionales**: Incluye atributos relevantes para cada entidad. Por ejemplo, para la entidad  `Pagos`, podrías considerar atributos como  `monto`,  `fecha_pago`,  `forma_pago`,  `estado_pago`, etc.
