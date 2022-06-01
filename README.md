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







