# 📐 Diagramas de Modelo Entidad-Relación (MER), Modelo Relacional y Modelo SQL
 
En esta sección se presentan los modelos conceptuales, relacionales y físicos (SQL) trabajados durante la **Unidad 1**, detallando los requerimientos de cada problema, la evidencia gráfica, su esquema relacional y su implementación en SQL.
 
> 📝 **Notas:**
> - En el modelo relacional, las llaves (primarias y foráneas) están marcadas con `acentos graves`.
> - Los scripts SQL están hechos para **MySQL** (probados en **DBeaver**). Cada ejemplo crea su propia base de datos.
 
---
 
## 📑 Ejemplos Desarrollados
 
### 📖 Ejemplo 1: Sistema de Biblioteca
Una biblioteca registra libros y autores. Un libro puede tener varios autores y un autor varios libros. Los socios toman libros prestados; de cada préstamo interesan la fecha de retiro y la de devolución.
 
> 📷 **Evidencia Diagrama ER:**
>
> <img width="2432" height="1768" alt="image" src="https://github.com/user-attachments/assets/053f9970-48cf-46e3-9eee-75e3ccaffd75" />
 
#### 🗃️ Modelo Relacional
* **Socio** (`id_socio`)
* **Libro** (`id_libro`, fecha_retiro, fecha_devolucion, `id_socio`)
* **Autor** (`id_autor`, `id_libro`)
#### 🛢️ Modelo SQL
 
**Diagrama:**
 
> <img width="885" height="255" alt="image" src="https://github.com/user-attachments/assets/22fe90da-65f0-4422-ba84-76f761283508" />

 
**Tablas:**
 
> 📷 *(pendiente: captura de las tablas en DBeaver)*
 
**Código:**
 
```sql
DROP DATABASE IF EXISTS biblioteca;
CREATE DATABASE biblioteca;
USE biblioteca;
 
CREATE TABLE socio (
    id_socio INT PRIMARY KEY
);
 
CREATE TABLE libro (
    id_libro         INT PRIMARY KEY,
    fecha_retiro     DATE,
    fecha_devolucion DATE,
    id_socio         INT,
    FOREIGN KEY (id_socio) REFERENCES socio(id_socio)
);
 
CREATE TABLE autor (
    id_autor INT PRIMARY KEY,
    id_libro INT,
    FOREIGN KEY (id_libro) REFERENCES libro(id_libro)
);
```
 
---
 
### 🍽️ Ejemplo 2: Pedidos de Restaurante
Los clientes realizan pedidos que contienen varios platos con distinta cantidad. Los platos se agrupan en categorías. Cada pedido lo atiende un único mesero. El precio del plato debe registrarse tal como estaba al momento del pedido, aunque luego cambie.
 
> 📷 **Evidencia Diagrama ER:**
>
> <img width="4368" height="2168" alt="image" src="https://github.com/user-attachments/assets/112c6b85-bb8a-460b-9297-17a6bd5831fa" />
 
#### 🗃️ Modelo Relacional
* **Cliente** (`id_cliente`)
* **Mesero** (`id_mesero`)
* **Pedido** (`id_pedido`, precio, cantidad, `id_cliente`, `id_mesero`)
* **Plato** (`id_plato`, precio_actual, `id_pedido`)
* **Categoría** (`id_categoria`, `id_plato`)
#### 🛢️ Modelo SQL
 
**Diagrama:**
 
> <img width="856" height="256" alt="image" src="https://github.com/user-attachments/assets/f9d38f43-eb28-4636-b25a-562b2c611eb7" />

 
**Tablas:**
 
> <img width="915" height="242" alt="image" src="https://github.com/user-attachments/assets/3d2444b8-e23b-4647-a394-5dca0a5a01fa" />

 
**Código:**
 
```sql
DROP DATABASE IF EXISTS ej2_restaurante;
CREATE DATABASE ej2_restaurante;
USE ej2_restaurante;
 
CREATE TABLE cliente (
    id_cliente INT PRIMARY KEY
);
 
CREATE TABLE mesero (
    id_mesero INT PRIMARY KEY
);
 
CREATE TABLE pedido (
    id_pedido  INT PRIMARY KEY,
    precio     FLOAT,
    cantidad   INT,
    id_cliente INT,
    id_mesero  INT,
    FOREIGN KEY (id_cliente) REFERENCES cliente(id_cliente),
    FOREIGN KEY (id_mesero)  REFERENCES mesero(id_mesero)
);
 
CREATE TABLE plato (
    id_plato      INT PRIMARY KEY,
    precio_actual FLOAT,
    id_pedido     INT,
    FOREIGN KEY (id_pedido) REFERENCES pedido(id_pedido)
);
 
CREATE TABLE categoria (
    id_categoria INT PRIMARY KEY,
    id_plato     INT,
    FOREIGN KEY (id_plato) REFERENCES plato(id_plato)
);
```
 
---
 
### 🩺 Ejemplo 3: Clínica Médica
Los pacientes solicitan citas; cada cita la atiende un médico a un paciente en fecha, hora y consultorio. De algunas citas se genera una receta que incluye medicamentos con dosis y duración. Cada médico pertenece a una especialidad.
 
> 📷 **Evidencia Diagrama ER:**
>
> <img width="5128" height="2688" alt="image" src="https://github.com/user-attachments/assets/bc8475bf-6baa-4e3f-80a5-4e4e050db610" />
 
#### 🗃️ Modelo Relacional
* **Paciente** (`id_paciente`)
* **Especialidad** (`id_especialidad`)
* **Médico** (`id_medico`, `id_especialidad`)
* **Cita** (`id_cita`, fecha, hora, consultorio, `id_paciente`, `id_medico`)
* **Receta** (`id_receta`, dosis, duración, `id_cita`)
* **Medicamento** (`id_medicamento`, `id_receta`)
#### 🛢️ Modelo SQL
 
**Diagrama:**
 
> <img width="856" height="216" alt="image" src="https://github.com/user-attachments/assets/9717b261-1775-47ee-9278-eb01e49a65ea" />

 
**Tablas:**
 
> <img width="851" height="250" alt="image" src="https://github.com/user-attachments/assets/6e1dab91-f634-4456-99c6-c0c7df41ea7d" />

 
**Código:**
 
```sql
DROP DATABASE IF EXISTS ej3_clinica;
CREATE DATABASE ej3_clinica;
USE ej3_clinica;
 
CREATE TABLE paciente (
    id_paciente INT PRIMARY KEY
);
 
CREATE TABLE especialidad (
    id_especialidad INT PRIMARY KEY
);
 
CREATE TABLE medico (
    id_medico       INT PRIMARY KEY,
    id_especialidad INT NOT NULL,
    FOREIGN KEY (id_especialidad) REFERENCES especialidad(id_especialidad)
);
 
CREATE TABLE cita (
    id_cita     INT PRIMARY KEY,
    fecha       DATE NOT NULL,
    hora        VARCHAR(10) NOT NULL,
    consultorio VARCHAR(20) NOT NULL,
    id_paciente INT NOT NULL,
    id_medico   INT NOT NULL,
    FOREIGN KEY (id_paciente) REFERENCES paciente(id_paciente),
    FOREIGN KEY (id_medico)   REFERENCES medico(id_medico)
);
 
CREATE TABLE receta (
    id_receta INT PRIMARY KEY,
    dosis     VARCHAR(50) NOT NULL,
    duracion  VARCHAR(50) NOT NULL,
    id_cita   INT NOT NULL UNIQUE,
    FOREIGN KEY (id_cita) REFERENCES cita(id_cita)
);
 
CREATE TABLE medicamento (
    id_medicamento INT PRIMARY KEY,
    id_receta      INT NOT NULL,
    FOREIGN KEY (id_receta) REFERENCES receta(id_receta)
);
```
 
---
 
### ✈️ Ejemplo 4: Sistema de Aerolínea
Una aerolínea opera vuelos (cada uno con número, fecha, origen y destino), cada vuelo lo realiza un avión. Los pasajeros realizan reservas para un vuelo específico, indicando asiento y tarifa pagada en el momento de la reserva.
 
> 📷 **Evidencia Diagrama ER:**
>
> <img width="3728" height="1888" alt="image" src="https://github.com/user-attachments/assets/a2fccd7a-d4c3-4c92-b1b5-d04c365c669f" />
 
#### 🗃️ Modelo Relacional
* **Avión** (`id_avion`)
* **Pasajeros** (`id_pasajero`)
* **Vuelo** (`numero`, fecha, origen, destino, `id_avion`)
* **Reservan** (`numero_vuelo`, `id_pasajero`, tarifa, asiento)
#### 🛢️ Modelo SQL
 
**Diagrama:**
 
> <img width="684" height="335" alt="image" src="https://github.com/user-attachments/assets/14caf886-e1e7-4f07-87b6-c170a6b19f17" />

 
**Tablas:**
 
> <img width="941" height="276" alt="image" src="https://github.com/user-attachments/assets/e9cd2404-f998-4411-b429-3cca5eaa64f5" />

 
**Código:**
 
```sql
DROP DATABASE IF EXISTS ej4_aerolinea;
CREATE DATABASE ej4_aerolinea;
USE ej4_aerolinea;
 
CREATE TABLE avion (
    id_avion INT PRIMARY KEY
);
 
CREATE TABLE pasajeros (
    id_pasajero INT PRIMARY KEY
);
 
CREATE TABLE vuelo (
    numero   INT PRIMARY KEY,
    fecha    DATE NOT NULL,
    origen   VARCHAR(60) NOT NULL,
    destino  VARCHAR(60) NOT NULL,
    id_avion INT NOT NULL,
    FOREIGN KEY (id_avion) REFERENCES avion(id_avion)
);
 
CREATE TABLE reservan (
    numero_vuelo INT NOT NULL,
    id_pasajero  INT NOT NULL,
    tarifa       FLOAT NOT NULL,
    asiento      VARCHAR(5) NOT NULL,
    PRIMARY KEY (numero_vuelo, id_pasajero),
    FOREIGN KEY (numero_vuelo) REFERENCES vuelo(numero),
    FOREIGN KEY (id_pasajero)  REFERENCES pasajeros(id_pasajero)
);
```
 
---
 
## 🛠️ Ejercicios Propuestos
 
### 🎵 Ejercicio 5: Empresa Discográfica
Una empresa discográfica necesita modelar los datos sobre sus diferentes recursos según las siguientes características:
- **Manager:** De cada mánager se almacena un identificador de manager y su nombre. Un manager representa a una serie de artistas.
- **Artista:** Un artista es representado por un manager. De los artistas se almacena su nombre completo (usando un único atributo) y su NIF.
- **Evento de promoción:** Los artistas participan en eventos de promoción para dar a conocer sus trabajos. En un evento de promoción pueden participar varios artistas. De un evento se almacena un identificador único, la fecha de celebración y el número de asistentes.
> 📷 **Evidencia Diagrama ER:**
>
> <img width="4168" height="2084" alt="image" src="https://github.com/user-attachments/assets/1cda67b6-c12a-44ba-92c0-e624223b8c6b" />
 
#### 🗃️ Modelo Relacional
* **Manager** (`alias`, nombre)
* **Artistas** (`NIF`, nombre, `alias`)
* **Evento** (`codigo_de_evento`, fecha_celebracion, numero_de_asistentes)
* **Participa** (`NIF`, `codigo_de_evento`)
#### 🛢️ Modelo SQL
 
**Diagrama:**
 
> <img width="941" height="450" alt="image" src="https://github.com/user-attachments/assets/a1eb2552-7091-44da-b2b1-ed9e013c17ec" />

 
**Tablas:**
 
> <img width="941" height="256" alt="image" src="https://github.com/user-attachments/assets/d6589d76-9a56-49d9-a636-48a08d0f2920" />

 
**Código:**
 
```sql
DROP DATABASE IF EXISTS ej5_discografica;
CREATE DATABASE ej5_discografica;
USE ej5_discografica;
 
CREATE TABLE manager (
    alias  VARCHAR(50) PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL
);
 
CREATE TABLE artistas (
    nif    VARCHAR(15) PRIMARY KEY,
    nombre VARCHAR(120) NOT NULL,
    alias  VARCHAR(50) NOT NULL,
    FOREIGN KEY (alias) REFERENCES manager(alias)
);
 
CREATE TABLE evento (
    codigo_de_evento     INT PRIMARY KEY,
    fecha_celebracion    DATE NOT NULL,
    numero_de_asistentes INT NOT NULL
);
 
CREATE TABLE participa (
    nif              VARCHAR(15) NOT NULL,
    codigo_de_evento INT NOT NULL,
    PRIMARY KEY (nif, codigo_de_evento),
    FOREIGN KEY (nif)              REFERENCES artistas(nif),
    FOREIGN KEY (codigo_de_evento) REFERENCES evento(codigo_de_evento)
);
```
 
---
 
### 💻 Ejercicio 6: Tienda Informática
Se desea crear una aplicación para la gestión de una tienda informática. La tienda dispone de una serie de productos que se venden a los clientes. De cada producto informático se desea guardar el código, descripción, precio y número de existencias. De cada cliente se desea guardar el código, nombre, apellidos, dirección y número telefónico. Un cliente puede comprar varios productos en la tienda y un mismo producto puede ser comprado por varios clientes. Cada vez que se compre un artículo quedará registrada la compra en la base de datos junto con la fecha en la que se compró el artículo. La tienda tiene contactos con varios proveedores que son los que suministran los productos. Un mismo producto puede ser suministrado por varios proveedores. De cada proveedor se desea guardar el código, nombres, apellidos, dirección, provincia y número de teléfono.
 
> 📷 **Evidencia Diagrama ER:**
>
> <img width="6268" height="2548" alt="image" src="https://github.com/user-attachments/assets/4db007ab-c1dc-4861-a3e9-b17521e95317" />
 
#### 🗃️ Modelo Relacional
* **Cliente** (`id`, nombre, apellidos, dirección)
* **Cliente_Telefono** (`id`, `teléfono`)
* **Producto** (`código`, descripción, precio, número_existencias)
* **Compra** (`id`, `código`, `fecha`)
* **Proveedor** (`identificador`, nombres, apellidos, dirección, provincia)
* **Proveedor_Telefono** (`identificador`, `num_teléfono`)
* **Suministra** (`código`, `identificador`)
#### 🛢️ Modelo SQL
 
**Diagrama:**
 
> <img width="664" height="921" alt="image" src="https://github.com/user-attachments/assets/af737062-99b2-448b-9f17-2a575a93cad4" />

 
**Tablas:**
 
> <img width="941" height="296" alt="image" src="https://github.com/user-attachments/assets/c73cb5de-375a-493f-bfd2-fcf740e22ac5" />

 
**Código:**
 
```sql
DROP DATABASE IF EXISTS ej6_tienda_informatica;
CREATE DATABASE ej6_tienda_informatica;
USE ej6_tienda_informatica;
 
CREATE TABLE cliente (
    id        INT PRIMARY KEY,
    nombre    VARCHAR(60) NOT NULL,
    apellidos VARCHAR(80) NOT NULL,
    direccion VARCHAR(150)
);
 
CREATE TABLE cliente_telefono (
    id       INT NOT NULL,
    telefono VARCHAR(15) NOT NULL,
    PRIMARY KEY (id, telefono),
    FOREIGN KEY (id) REFERENCES cliente(id)
);
 
CREATE TABLE producto (
    codigo             INT PRIMARY KEY,
    descripcion        VARCHAR(150) NOT NULL,
    precio             FLOAT NOT NULL,
    numero_existencias INT NOT NULL
);
 
CREATE TABLE compra (
    id     INT NOT NULL,
    codigo INT NOT NULL,
    fecha  DATE NOT NULL,
    PRIMARY KEY (id, codigo, fecha),
    FOREIGN KEY (id)     REFERENCES cliente(id),
    FOREIGN KEY (codigo) REFERENCES producto(codigo)
);
 
CREATE TABLE proveedor (
    identificador INT PRIMARY KEY,
    nombres       VARCHAR(60) NOT NULL,
    apellidos     VARCHAR(80) NOT NULL,
    direccion     VARCHAR(150),
    provincia     VARCHAR(60)
);
 
CREATE TABLE proveedor_telefono (
    identificador INT NOT NULL,
    num_telefono  VARCHAR(15) NOT NULL,
    PRIMARY KEY (identificador, num_telefono),
    FOREIGN KEY (identificador) REFERENCES proveedor(identificador)
);
 
CREATE TABLE suministra (
    codigo        INT NOT NULL,
    identificador INT NOT NULL,
    PRIMARY KEY (codigo, identificador),
    FOREIGN KEY (codigo)        REFERENCES producto(codigo),
    FOREIGN KEY (identificador) REFERENCES proveedor(identificador)
);
```
 
---
 
### 📀 Ejercicio 7: Discos Musicales
- La tienda vende discos de diferentes géneros musicales y cantantes.
- Cada género musical tiene un identificador.
- Cada disco tiene título, género musical, precio y cantante.
- De un cantante se registra el nombre y su país. Todo disco pertenece a un cantante y un cantante puede tener muchos discos. Además, un disco tiene un conjunto de canciones (identificador y título).
- Un disco tiene muchas canciones y una canción puede estar en varios discos. Interesa conocer la posición de una canción en un determinado disco.
> 📷 **Evidencia Diagrama ER:**
>
> <img width="6208" height="2244" alt="image" src="https://github.com/user-attachments/assets/3ca40e03-ab5a-4491-8353-b04510292226" />
 
#### 🗃️ Modelo Relacional
* **Género_Musical** (`id`)
* **Cantante** (`identificador`, nombre, país)
* **Canción** (`identificador`, título)
* **Disco** (`id`, título, precio, `id_genero`, `id_cantante`)
* **Tiene** (`id`, `identificador`, posición)
#### 🛢️ Modelo SQL
 
**Diagrama:**
 
> <img width="920" height="639" alt="image" src="https://github.com/user-attachments/assets/8934eae8-5842-45d5-835c-9f19c593a801" />

 
**Tablas:**
 
> <img width="941" height="278" alt="image" src="https://github.com/user-attachments/assets/bc2ee2d1-a130-4666-8858-72f2cbfcca43" />

 
**Código:**
 
```sql
DROP DATABASE IF EXISTS ej7_discos;
CREATE DATABASE ej7_discos;
USE ej7_discos;
 
CREATE TABLE genero_musical (
    id INT PRIMARY KEY
);
 
CREATE TABLE cantante (
    identificador INT PRIMARY KEY,
    nombre        VARCHAR(100) NOT NULL,
    pais          VARCHAR(60)
);
 
CREATE TABLE cancion (
    identificador INT PRIMARY KEY,
    titulo        VARCHAR(120) NOT NULL
);
 
CREATE TABLE disco (
    id          INT PRIMARY KEY,
    titulo      VARCHAR(120) NOT NULL,
    precio      FLOAT NOT NULL,
    id_genero   INT NOT NULL,
    id_cantante INT NOT NULL,
    FOREIGN KEY (id_genero)   REFERENCES genero_musical(id),
    FOREIGN KEY (id_cantante) REFERENCES cantante(identificador)
);
 
CREATE TABLE tiene (
    id            INT NOT NULL,
    identificador INT NOT NULL,
    posicion      INT NOT NULL,
    PRIMARY KEY (id, identificador),
    FOREIGN KEY (id)            REFERENCES disco(id),
    FOREIGN KEY (identificador) REFERENCES cancion(identificador)
);
```
 
---
 
### 🚚 Ejercicio 8: Empresa de Camiones
Se desea informatizar la gestión de una empresa de transporte que reparte paquetes por toda España. Los encargados de llevar los paquetes son los camioneros, de los que se quiere guardar la cédula, nombre, teléfono, dirección, salario y población en que vive. De los paquetes transportados interesa conocer el código del paquete, descripción, destinatario y dirección del destinatario.
 
Un camionero distribuye muchos paquetes, y un paquete sólo puede ser distribuido por un camionero. De las provincias a las que llegan los paquetes interesa guardar el código de provincia y el nombre. Un paquete sólo puede llegar a una provincia. Sin embargo, a una provincia pueden llegar varios paquetes.
 
De los camiones que llevan los camioneros, interesa conocer la matrícula, modelo, tipo y potencia. Un camionero puede conducir diferentes camiones en fechas diferentes, y un camión puede ser conducido por varios camioneros.
 
> 📷 **Evidencia Diagrama ER:**
>
> <img width="4888" height="3368" alt="image" src="https://github.com/user-attachments/assets/1c46a3cc-41e9-43e5-addd-a9cdee813242" />
 
#### 🗃️ Modelo Relacional
* **Provincia** (`código`, nombre)
* **Destinatario** (`id_destinatario`, nombre, dirección)
* **Camionero** (`cédula`, nombre, dirección)
* **Camionero_Telefono** (`cédula`, `teléfono`)
* **Paquete** (`código`, descripción, dirección, `código_provincia`, `id_destinatario`, `cédula`)
* **Camión** (`matrícula`, modelo, potencia, tipo, `cédula`)
#### 🛢️ Modelo SQL
 
**Diagrama:**
 
> <img width="775" height="887" alt="image" src="https://github.com/user-attachments/assets/85395fb3-0fbf-4104-82f5-b8d7d4fb1274" />

 
**Tablas:**
 
> <img width="941" height="258" alt="image" src="https://github.com/user-attachments/assets/4b57744c-9c0e-42cf-8052-e73b2965d911" />

 
**Código:**
 
```sql
DROP DATABASE IF EXISTS ej8_camiones;
CREATE DATABASE ej8_camiones;
USE ej8_camiones;
 
CREATE TABLE provincia (
    codigo INT PRIMARY KEY,
    nombre VARCHAR(80) NOT NULL
);
 
CREATE TABLE destinatario (
    id_destinatario INT PRIMARY KEY,
    nombre          VARCHAR(100) NOT NULL,
    direccion       VARCHAR(150)
);
 
CREATE TABLE camionero (
    cedula    VARCHAR(10) PRIMARY KEY,
    nombre    VARCHAR(100) NOT NULL,
    direccion VARCHAR(150)
);
 
CREATE TABLE camionero_telefono (
    cedula   VARCHAR(10) NOT NULL,
    telefono VARCHAR(15) NOT NULL,
    PRIMARY KEY (cedula, telefono),
    FOREIGN KEY (cedula) REFERENCES camionero(cedula)
);
 
CREATE TABLE paquete (
    codigo           INT PRIMARY KEY,
    descripcion      VARCHAR(150),
    direccion        VARCHAR(150),
    codigo_provincia INT NOT NULL,
    id_destinatario  INT NOT NULL,
    cedula           VARCHAR(10) NOT NULL,
    FOREIGN KEY (codigo_provincia) REFERENCES provincia(codigo),
    FOREIGN KEY (id_destinatario)  REFERENCES destinatario(id_destinatario),
    FOREIGN KEY (cedula)           REFERENCES camionero(cedula)
);
 
CREATE TABLE camion (
    matricula VARCHAR(10) PRIMARY KEY,
    modelo    VARCHAR(50),
    potencia  INT,
    tipo      VARCHAR(50),
    cedula    VARCHAR(10) NOT NULL,
    FOREIGN KEY (cedula) REFERENCES camionero(cedula)
);
```
 
---
 
### 🐾 Ejercicio 9: Veterinaria
- **Propietario:** Cédula, apellidos, nombres, dirección y teléfonos.
- **Mascota:** Identificador, nombre, fecha de nacimiento y tipo.
  * Un propietario puede llevar una o varias mascotas.
  * Una mascota la lleva uno y solo un propietario.
- **Personal de la clínica:** Código, cédula y nombre.
  * Se distingue a los **veterinarios** y **auxiliares** (pudiendo haber otros tipos).
  * **Veterinario:** Fecha de alta y especialidad.
  * **Auxiliares:** Base de cotización.
- **Consultas:** Las mascotas pasan consulta con los veterinarios.
  * Un veterinario puede pasar consulta a ninguna o a varias mascotas.
  * Una misma mascota ha pasado consulta una o más veces en fechas diferentes con varios veterinarios.
  * Al pasar consulta se realiza un diagnóstico.
- **Familiar de contacto:** Cada propietario tiene un familiar de contacto para casos de emergencia.
  * Se registra la cédula, nombre y teléfono.
  * Si el propietario se da de alta en la clínica veterinaria, el familiar ya no interesa.
  * El familiar es contacto de un solo propietario.
> 📷 **Evidencia Diagrama ER:**
>
> <img width="6328" height="2688" alt="image" src="https://github.com/user-attachments/assets/53bae9e4-6ddf-443f-bf72-2757f4780a1f" />
 
#### 🗃️ Modelo Relacional
* **Propietario** (`cédula`, nombres, dirección)
* **Teléfonos** (`teléfono`, `cédula`)
* **Familiar** (`cédula`, nombres, `cédula_propietario`)
* **Telf_familiar** (`teléfono`, `cédula`)
* **Mascota** (`id`, fecha_nacimiento, nombre, tipo, `cédula`)
* **Personal** (`cédula`, nombre, código)
* **Veterinario** (`cédula`, nombre, código, fecha_alta, especialidad)
* **Auxiliar** (`cédula`, nombre, código, base_cotización)
* **Consulta** (`fecha_atención`, diagnóstico, `id`, `cédula`)
> 🔗 `Veterinario.cédula` y `Auxiliar.cédula` son llaves foráneas hacia `Personal.cédula` (herencia).
 
#### 🛢️ Modelo SQL
 
**Diagrama:**
 
> <img width="1089" height="725" alt="image" src="https://github.com/user-attachments/assets/0b47b07f-cd90-4dd0-9323-b6fc7221dc6e" />

 
**Tablas:**
 
> <img width="941" height="253" alt="image" src="https://github.com/user-attachments/assets/1258f953-6ef6-4b50-be44-342eba9e215a" />

 
**Código:**
 
```sql
DROP DATABASE IF EXISTS ej9_veterinaria;
CREATE DATABASE ej9_veterinaria;
USE ej9_veterinaria;
 
CREATE TABLE propietario (
    cedula    VARCHAR(10) PRIMARY KEY,
    nombres   VARCHAR(100) NOT NULL,
    direccion VARCHAR(150)
);
 
CREATE TABLE telefonos (
    telefono VARCHAR(15) NOT NULL,
    cedula   VARCHAR(10) NOT NULL,
    PRIMARY KEY (telefono, cedula),
    FOREIGN KEY (cedula) REFERENCES propietario(cedula)
);
 
CREATE TABLE familiar (
    cedula             VARCHAR(10) PRIMARY KEY,
    nombres            VARCHAR(100) NOT NULL,
    cedula_propietario VARCHAR(10) NOT NULL UNIQUE,
    FOREIGN KEY (cedula_propietario) REFERENCES propietario(cedula)
);
 
CREATE TABLE telf_familiar (
    telefono VARCHAR(15) NOT NULL,
    cedula   VARCHAR(10) NOT NULL,
    PRIMARY KEY (telefono, cedula),
    FOREIGN KEY (cedula) REFERENCES familiar(cedula)
);
 
CREATE TABLE mascota (
    id               INT PRIMARY KEY,
    fecha_nacimiento DATE,
    nombre           VARCHAR(60) NOT NULL,
    tipo             VARCHAR(40),
    cedula           VARCHAR(10) NOT NULL,
    FOREIGN KEY (cedula) REFERENCES propietario(cedula)
);
 
CREATE TABLE personal (
    cedula VARCHAR(10) PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    codigo INT UNIQUE
);
 
CREATE TABLE veterinario (
    cedula       VARCHAR(10) PRIMARY KEY,
    nombre       VARCHAR(100) NOT NULL,
    codigo       INT,
    fecha_alta   DATE,
    especialidad VARCHAR(80),
    FOREIGN KEY (cedula) REFERENCES personal(cedula)
);
 
CREATE TABLE auxiliar (
    cedula          VARCHAR(10) PRIMARY KEY,
    nombre          VARCHAR(100) NOT NULL,
    codigo          INT,
    base_cotizacion FLOAT,
    FOREIGN KEY (cedula) REFERENCES personal(cedula)
);
 
CREATE TABLE consulta (
    fecha_atencion DATE NOT NULL,
    diagnostico    VARCHAR(255),
    id             INT NOT NULL,
    cedula         VARCHAR(10) NOT NULL,
    PRIMARY KEY (fecha_atencion, id, cedula),
    FOREIGN KEY (id)     REFERENCES mascota(id),
    FOREIGN KEY (cedula) REFERENCES veterinario(cedula)
);
```
 
