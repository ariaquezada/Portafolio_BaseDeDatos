# 📐 Diagramas de Modelo Entidad-Relación (MER)

En esta sección se presentan los modelos conceptuales trabajados durante la **Unidad 1**, detallando los requerimientos de cada problema y la evidencia gráfica correspondiente.

> 📌 **Instrucción:** Agregar la evidencia gráfica (diagrama ER) correspondiente para cada uno de los siguientes ejercicios.

---

## 📑 Ejemplos Desarrollados

### 📖 Ejemplo 1: Sistema de Biblioteca
Una biblioteca registra libros y autores. Un libro puede tener varios autores y un autor varios libros. Los socios toman libros prestados; de cada préstamo interesan la fecha de retiro y la de devolución.

> 📷 **Evidencia Diagrama ER:**
>
> <img width="2196" height="1768" alt="image" src="https://github.com/user-attachments/assets/fb3287ed-83cd-4e43-ba1a-c13f97e0472a" />

---

### 🍽️ Ejemplo 2: Pedidos de Restaurante
Los clientes realizan pedidos que contienen varios platos con distinta cantidad. Los platos se agrupan en categorías. Cada pedido lo atiende un único mesero. El precio del plato debe registrarse tal como estaba al momento del pedido, aunque luego cambie.

> 📷 **Evidencia Diagrama ER:**
>
> <img width="1088" height="538" alt="Pedidos_en_un_restaurante drawio" src="https://github.com/user-attachments/assets/ff195a60-ca9e-42a2-b351-0dede465471b" />


---

### 🩺 Ejemplo 3: Clínica Médica
Los pacientes solicitan citas; cada cita la atiende un médico a un paciente en fecha, hora y consultorio. De algunas citas se genera una receta que incluye medicamentos con dosis y duración. Cada médico pertenece a una especialidad.

> 📷 **Evidencia Diagrama ER:**
>
> <img width="1278" height="668" alt="Clinica_medica drawio" src="https://github.com/user-attachments/assets/cdbe44be-14f9-444b-9988-fec55e4d495c" />


---

### ✈️ Ejemplo 4: Sistema de Aerolínea
Una aerolínea opera vuelos (cada uno con número, fecha, origen y destino), cada vuelo lo realiza un avión. Los pasajeros realizan reservas para un vuelo específico, indicando asiento y tarifa pagada en el momento de la reserva.

> 📷 **Evidencia Diagrama ER:**
>
> <img width="928" height="468" alt="Sistema de aerolineas drawio" src="https://github.com/user-attachments/assets/0735b2a3-056d-4e50-b51f-e500b9a15eac" />


---

## 🛠️ Ejercicios Propuestos

### 🎵 Ejercicio 5: Empresa Discográfica
Una empresa discográfica necesita modelar los datos sobre sus diferentes recursos según las siguientes características:
* **Manager:** De cada mánager se almacena un identificador de manager y su nombre. Un manager representa a una serie de artistas.
* **Artista:** Un artista es representado por un manager. De los artistas se almacena su nombre completo (usando un único atributo) y su NIF.
* **Evento de promoción:** Los artistas participan en eventos de promoción para dar a conocer sus trabajos. En un evento de promoción pueden participar varios artistas. De un evento se almacena un identificador único, la fecha de celebración y el número de asistentes.

> 📷 **Evidencia Diagrama ER:**
>
> <img width="4168" height="1728" alt="image" src="https://github.com/user-attachments/assets/95072c4f-39c1-4b02-8abb-2d07a147406e" />


---

### 💻 Ejercicio 6: Tienda Informática
Se desea crear una aplicación para la gestión de una tienda informática. La tienda dispone de una serie de productos que se venden a los clientes. De cada producto informático se desea guardar el código, descripción, precio y número de existencias. De cada cliente se desea guardar el código, nombre, apellidos, dirección y número telefónico. Un cliente puede comprar varios productos en la tienda y un mismo producto puede ser comprado por varios clientes. Cada vez que se compre un artículo quedará registrada la compra en la base de datos junto con la fecha en la que se compró el artículo. La tienda tiene contactos con varios proveedores que son los que suministran los productos. Un mismo producto puede ser suministrado por varios proveedores. De cada proveedor se desea guardar el código, nombres, apellidos, dirección, provincia y número de teléfono.

> 📷 **Evidencia Diagrama ER:**
>
> ![Diagrama ER - Tienda Informática](./img/ejercicio6_tienda_informatica.png)

---

### 📀 Ejercicio 7: Discos Musicales
* La tienda vende discos de diferentes géneros musicales y cantantes.
* Cada género musical tiene un identificador.
* Cada disco tiene título, género musical, precio y cantante.
* De un cantante se registra el nombre y su país. Todo disco pertenece a un cantante y un cantante puede tener muchos discos. Además, un disco tiene un conjunto de canciones (identificador y título).
* Un disco tiene muchas canciones y una canción puede estar en varios discos. Interesa conocer la posición de una canción en un determinado disco.

> 📷 **Evidencia Diagrama ER:**
>
> ![Diagrama ER - Discos Musicales](./img/ejercicio7_discos_musicales.png)

---

### 🚚 Ejercicio 8: Empresa de Camiones
Se desea informatizar la gestión de una empresa de transporte que reparte paquetes por toda España. Los encargados de llevar los paquetes son los camioneros, de los que se quiere guardar la cédula, nombre, teléfono, dirección, salario y población en que vive. De los paquetes transportados interesa conocer el código del paquete, descripción, destinatario y dirección del destinatario.

Un camionero distribuye muchos paquetes, y un paquete sólo puede ser distribuido por un camionero. De las provincias a las que llegan los paquetes interesa guardar el código de provincia y el nombre. Un paquete sólo puede llegar a una provincia. Sin embargo, a una provincia pueden llegar varios paquetes.

De los camiones que llevan los camioneros, interesa conocer la matrícula, modelo, tipo y potencia. Un camionero puede conducir diferentes camiones en fechas diferentes, y un camión puede ser conducido por varios camioneros.

> 📷 **Evidencia Diagrama ER:**
>
> <img width="4888" height="3368" alt="image" src="https://github.com/user-attachments/assets/dc4140c6-26e6-4a1c-8c33-7f9a48c01843" />


---

### 🐾 Ejercicio 9: Veterinaria
* **Propietario:** Cédula, apellidos, nombres, dirección y teléfonos.
* **Mascota:** Identificador, nombre, fecha de nacimiento y tipo.
  * Un propietario puede llevar una o varias mascotas.
  * Una mascota la lleva uno y solo un propietario.
* **Personal de la clínica:** Código, cédula y nombre.
  * Se distingue a los **veterinarios** y **auxiliares** (pudiendo haber otros tipos).
  * **Veterinario:** Fecha de alta y especialidad.
  * **Auxiliares:** Base de cotización.
* **Consultas:** Las mascotas pasan consulta con los veterinarios.
  * Un veterinario puede pasar consulta a ninguna o a varias mascotas.
  * Una misma mascota ha pasado consulta una o más veces en fechas diferentes con varios veterinarios.
  * Al pasar consulta se realiza un diagnóstico.
* **Familiar de contacto:** Cada propietario tiene un familiar de contacto para casos de emergencia.
  * Se registra la cédula, nombre y teléfono.
  * Si el propietario se da de alta en la clínica veterinaria, el familiar ya no interesa.
  * El familiar es contacto de un solo propietario.

> <img width="6328" height="2688" alt="image" src="https://github.com/user-attachments/assets/3fa6c9d3-5c35-40cd-a7df-c24708a0ba7a" />

>
> 
