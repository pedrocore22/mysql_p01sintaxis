# MySQL

## Introducción

MySQL es un sistema gestor de bases de datos que usa SQL (lenguaje estructurado de consultas).

En la instalación nos provee:

    - Servidor de base de datos MySQL (en desarrollo)
    - Herramienta gráfica MySQL Workbenck

## Bases de datos

El concepto de base de datos es el mayor nivel de agrupamiento
en los servidores de bases de datos y normalmente están asociados
a una aplicación o si la aplicación es muy grande a una parte.

## Tabla

La tabla se incluye dentro de las bases de datos y corresponde con
una entidad de la aplicación (usuario, pedido, artículo, ...).

## Registro o fila

Los registros son el conjunto de datos concretos para una entidad (Juan, Gómez, 44, ...).

## Empleo de SQL en MySQL

El uso del lenguaje SQL se puede realizar de múltiples formas 
y en MySQL Workbench disponemos de "ventanas" para escribirlo
y ejecutarlo en la base de datos.

### Administrar bases de datos

Crear una base de datos:

CREATE DATABASE app; // app es el nombre que le queramos dar a 
                     // la base de datos

Eliminar una base de datos:

DROP DATABASE app; 

### Manejo de tablas en una base de datos

En MySQL Workbench hay que setear como default la base
de datos sobre la que queramos crear/modificar/eliminar
sus tablas (seleccionar base de datos y con botón
derecho marcar "set as default schema").

CREATE TABLE articulos (
	id int AUTO_INCREMENT PRIMARY KEY,
    sku varchar(4) NOT NULL,
    marca varchar(255) NOT NULL,
    modelo varchar(255) NOT NULL,
    descripcion varchar(255),
    fecha_alta DATE NOT NULL
);

Los campos siempre los creamos con la sintaxis

    identificador tipo-datos constraints

Para eliminar tablas:

DROP TABLE articulos;

Para modificar tablas, por ejemplo:

ALTER TABLE articulos
ADD UNIQUE (sku);

## OPERACIONES CRUD (Create, Read, Update, Delete)

### Crear registros o filas

INSERT INTO articulos (sku, marca, modelo, descripcion, fecha_alta)
VALUES ('F412','Adidas','Pace','Lorem...', CURDATE());

### Leer registros

La forma menos restrictiva es leer todos los registros
de la base de datos

SELECT * FROM articulos; // Consulta

Si necesitamos solamente algunos campos (proyección) se pasan
separados por comas

SELECT marca, modelo FROM articulos;

*Más adelante vemos más consultas

### Actualizar registros

La actualización aunque se puede hacer en masa normalmente se hace
para un solo registro.

UPDATE articulos
SET modelo = 'Pace gold'
WHERE id = 3;

### Eliminar registros

Para eliminar (si fuera posible) se suele hacer también
para un solo registro.

DELETE FROM articulos
WHERE id = 3;

## CONSULTAS VARIADAS

Consulta de los valores distintos de un campo

SELECT DISTINCT marca
FROM articulos;

--- Añadimos una nueva tabla para tener nuevos datos ---

CREATE TABLE proveedores (
	id int AUTO_INCREMENT PRIMARY KEY,
    cif varchar(9) UNIQUE NOT NULL,
    nombre varchar(255) NOT NULL,
    direccion varchar(255) NOT NULL,
    localidad varchar(255) NOT NULL,
    fecha_alta DATE NOT NULL
);

INSERT INTO proveedores (cif, nombre, direccion, localidad, fecha_alta)
VALUES ('A12345678', 
        'Logística Int S.A.',
        'Av París 20',
        'Cáceres',
        CURDATE());
INSERT INTO proveedores (cif, nombre, direccion, localidad, fecha_alta)
VALUES ('A87654321', 
        'Distribuciones Cáceres S.A.',
        'Gil Cordero 30',
        'Cáceres',
        CURDATE());
INSERT INTO proveedores (cif, nombre, direccion, localidad, fecha_alta)
VALUES ('A11223344', 
        'Madrid Sports S.A.',
        'Príncipe de Vergara, 48',
        'Madrid',
        CURDATE());    

-------------------------------

Consultas con valores de igualdad en un campo

SELECT *
FROM proveedores
WHERE localidad = 'Cáceres';

Consultas con valores de igualdad en un campo 
con patrones de búsqueda

SELECT *
FROM articulos
WHERE modelo LIKE 'R%';

--- Modificar tabla artículo para añadir un campo
de género con un ENUM de valores ----

ALTER TABLE articulos
ADD genero ENUM('Hombre','Mujer','Niña','Niño','Todos');

Añadimos un registro con el nuevo campo:

INSERT INTO articulos (sku, marca, modelo, genero, fecha_alta)
VALUES ('G77A',
        'New Balance', 
        'Classic', 
        'Niña',
        CURDATE());

-------------------------------------

Consultas con valores de igualdad en varios campos
en los que se han de cumplir las dos condiciones (AND lógico)

SELECT *
FROM articulos
WHERE marca = 'New Balance' AND genero = 'Niña';

Consultas con valores de igualdad en varios campos
en los que se han de cumplir alguna de las condiciones (OR lógico)

SELECT *
FROM articulos
WHERE marca = 'Adidas' OR genero = 'Niña';

Consultas con valores de desigualdad (NOT lógico)

SELECT *
FROM proveedores
WHERE NOT localidad = 'Cáceres';

Consultas ordenadas

Por ejemplo por varios campos con diferente sentido

SELECT *
FROM articulos
ORDER BY marca ASC, modelo DESC;

Se pueden combinar con el filtrado por campos

SELECT *
FROM articulos
WHERE marca = 'Nike'
ORDER BY modelo ASC;

## TABLAS CON CLAVES FORÁNEAS (RELACIONES)

CREATE TABLE ofertas (
    id int AUTO_INCREMENT PRIMARY KEY,
    articuloID int,
    proveedorID int,
    precio double NOT NULL,
    dias_entrega int NOT NULL,
    fecha_oferta DATE NOT NULL,
    FOREIGN KEY (articuloID) REFERENCES articulos(id),
    FOREIGN KEY (proveedorID) REFERENCES proveedores(id)
)

--------- Introducimos varias ofertas ---------

INSERT INTO ofertas(articuloID, 
                    proveedorID, 
                    precio, 
                    dias_entrega, 
                    fecha_oferta)
VALUES (<valor>, <valor>, 38.2, 3, CURDATE());

--------------------------------------------------

## CONSULTAS EN TABLAS RELACIONADAS (JOIN)

Los JOIN más usados y básicos son del tipo INNER

SELECT ofertas.id, 
       proveedores.nombre, 
       articulos.marca, 
       articulos.modelo,
       ofertas.precio,
       ofertas.dias_entrega
FROM ofertas
INNER JOIN articulos ON ofertas.articuloID = articulos.id
INNER JOIN proveedores ON ofertas.proveedorID = proveedores.id;