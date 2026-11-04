# Proyecto: Base de datos para la gestión de emergencias (ECU 911 – Loja)

## 1. Título

**Diseño e implementación de una base de datos para la gestión de llamadas de emergencia y el despacho de instituciones en una central tipo ECU 911 de la provincia de Loja.**

---

## 2. Contexto del problema

**Situación.** En una central de emergencias, cada llamada empieza con una persona (el *alertante*) que se comunica con un *operador*. El operador registra la ubicación del incidente, lo clasifica según su gravedad (verde, naranja o rojo) y, dependiendo del nivel, avisa a una o varias instituciones: bomberos, SUCOD, militares, gestión de riesgo, Policía Nacional o ambulancia (multidespacho).

**Dificultades sin una base de datos.** Si esta información se maneja en papel u hojas sueltas:

- Se pueden perder o duplicar los datos del alertante y de sus teléfonos.
- Es difícil saber qué operador atendió cada incidente y a qué instituciones se avisó.
- Se confunden la dirección desde donde llama el alertante y la dirección a donde se envía la ayuda.
- No se puede consultar rápido el historial de incidentes por cantón, nivel de alarma o institución.

**A quiénes afecta.** A las personas que piden ayuda, a los operadores que atienden las llamadas, a las instituciones que responden y a los responsables de la central que necesitan llevar un control.

**Datos que se gestionan y por qué importan.**

| Dato | Por qué importa |
|---|---|
| Alertante y sus teléfonos | Permite identificar a quien llama y volver a contactarlo; un alertante puede tener varios teléfonos |
| Operador | Deja constancia de quién atendió cada incidente |
| Incidente (descripción, direcciones, referencia, fecha, multidespacho) | Es el registro central de la emergencia |
| Cantón | Ubica el incidente dentro de la provincia de Loja |
| Nivel de alarma | Define la prioridad: verde (puede esperar), naranja (medio), rojo (urgencia) |
| Instituciones despachadas | Registra qué instituciones fueron avisadas en cada incidente |

---

## 3. Objetivos

**Objetivo general**

Diseñar e implementar una base de datos relacional que permita registrar y consultar los incidentes, alertantes, operadores e instituciones de una central de emergencias tipo ECU 911, mediante el modelado entidad-relación y el lenguaje SQL, para organizar la información y agilizar la atención de las emergencias.

**Objetivos específicos**

1. Elaborar el modelo conceptual (diagrama entidad-relación) y el modelo lógico (modelo relacional) del sistema, identificando entidades, atributos, relaciones y cardinalidades.
2. Implementar el modelo en SQL (MySQL) creando las tablas con sus claves y restricciones, e insertar datos de prueba para realizar consultas que verifiquen su correcto funcionamiento.

---

## 4. Modelo entidad-relación

![Diagrama entidad-relación del sistema de emergencias](imagenes/modelo_er_emergencias.png)

**Entidades y atributos**

| Entidad | Atributos |
|---|---|
| Alertante | <u>id_alertante</u>, nombre |
| Teléfono | <u>id_telefono</u>, número, nombre (usuario registrado en la línea) |
| Operador | <u>id_operador</u>, nombre |
| Incidente | <u>id_incidente</u>, descripción, fecha, dirección del alertante, dirección del servicio, referencia, multidespacho |
| Cantón | <u>id_canton</u>, nombre |
| Nivel_alarma | <u>id_nivel</u>, color, significado |
| Institución | <u>id_institucion</u>, nombre |

**Relaciones**

| Relación | Entidades | Cardinalidad |
|---|---|---|
| tiene | Alertante – Teléfono | 1 : N (un alertante tiene de uno a varios teléfonos) |
| reporta | Alertante – Incidente | 1 : N |
| atiende | Operador – Incidente | 1 : N |
| ocurre en | Cantón – Incidente | 1 : N |
| clasifica | Nivel_alarma – Incidente | 1 : N |
| despacha | Incidente – Institución | N : M |

> Si la dirección del servicio coincide con la del alertante (por ejemplo, Sauces Norte), se copia automáticamente.

---

## 5. Modelo relacional

- **CANTON**(<u>id_canton</u>, nombre)
- **OPERADOR**(<u>id_operador</u>, nombre)
- **NIVEL_ALARMA**(<u>id_nivel</u>, color, significado)
- **ALERTANTE**(<u>id_alertante</u>, nombre)
- **TELEFONO**(<u>id_telefono</u>, numero, nombre, *id_alertante*)
- **INSTITUCION**(<u>id_institucion</u>, nombre)
- **INCIDENTE**(<u>id_incidente</u>, descripcion, fecha, direccion_alertante, direccion_servicio, referencia, multidespacho, *id_operador*, *id_canton*, *id_nivel*, *id_alertante*)
- **DESPACHO**(<u>*id_incidente*</u>, <u>*id_institucion*</u>)

*(subrayado = clave primaria, cursiva = clave foránea)*

---

## 6. Modelo SQL

![Diagrama del modelo SQL en DBeaver](imagenes/modelo_sql_emergencias.png)

### 6.1 Creación de tablas

```sql
CREATE DATABASE emergencias;
USE emergencias;

CREATE TABLE canton (
    id_canton INT PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL UNIQUE
);

CREATE TABLE operador (
    id_operador INT PRIMARY KEY,
    nombre VARCHAR(80) NOT NULL
);

CREATE TABLE nivel_alarma (
    id_nivel INT PRIMARY KEY,
    color VARCHAR(20) NOT NULL UNIQUE,
    significado VARCHAR(100) NOT NULL
);

CREATE TABLE alertante (
    id_alertante INT PRIMARY KEY,
    nombre VARCHAR(80) NOT NULL
);

CREATE TABLE telefono (
    id_telefono INT PRIMARY KEY,
    numero VARCHAR(15) NOT NULL UNIQUE,
    nombre VARCHAR(80) NOT NULL,
    id_alertante INT NOT NULL,
    FOREIGN KEY (id_alertante) REFERENCES alertante(id_alertante)
);

CREATE TABLE institucion (
    id_institucion INT PRIMARY KEY,
    nombre VARCHAR(60) NOT NULL UNIQUE
);

CREATE TABLE incidente (
    id_incidente INT PRIMARY KEY,
    descripcion VARCHAR(200) NOT NULL,
    fecha DATE NOT NULL,
    direccion_alertante VARCHAR(100) NOT NULL,
    direccion_servicio VARCHAR(100) NOT NULL,
    referencia VARCHAR(100),
    multidespacho VARCHAR(2) NOT NULL,
    id_operador INT NOT NULL,
    id_canton INT NOT NULL,
    id_nivel INT NOT NULL,
    id_alertante INT NOT NULL,
    FOREIGN KEY (id_operador) REFERENCES operador(id_operador),
    FOREIGN KEY (id_canton) REFERENCES canton(id_canton),
    FOREIGN KEY (id_nivel) REFERENCES nivel_alarma(id_nivel),
    FOREIGN KEY (id_alertante) REFERENCES alertante(id_alertante)
);

CREATE TABLE despacho (
    id_incidente INT NOT NULL,
    id_institucion INT NOT NULL,
    PRIMARY KEY (id_incidente, id_institucion),
    FOREIGN KEY (id_incidente) REFERENCES incidente(id_incidente),
    FOREIGN KEY (id_institucion) REFERENCES institucion(id_institucion)
);
```

### 6.2 Datos de prueba

```sql
INSERT INTO canton VALUES (1, 'Loja');
INSERT INTO canton VALUES (2, 'Espíndola');
INSERT INTO canton VALUES (3, 'Calvas');

INSERT INTO operador VALUES (1, 'Carlos Mora');
INSERT INTO operador VALUES (2, 'Ana Ruiz');

INSERT INTO nivel_alarma VALUES (1, 'Verde', 'Puede esperar');
INSERT INTO nivel_alarma VALUES (2, 'Naranja', 'Medio');
INSERT INTO nivel_alarma VALUES (3, 'Rojo', 'Urgencia');

INSERT INTO alertante VALUES (1, 'Marta Lozano');
INSERT INTO alertante VALUES (2, 'Juan Pérez');

INSERT INTO telefono VALUES (1, '0991234567', 'Marta Lozano', 1);
INSERT INTO telefono VALUES (2, '072570123', 'Luis Lozano', 1);
INSERT INTO telefono VALUES (3, '0987654321', 'Juan Pérez', 2);

INSERT INTO institucion VALUES (1, 'Bomberos');
INSERT INTO institucion VALUES (2, 'SUCOD');
INSERT INTO institucion VALUES (3, 'Militares');
INSERT INTO institucion VALUES (4, 'Gestión de riesgo');
INSERT INTO institucion VALUES (5, 'Policía Nacional');
INSERT INTO institucion VALUES (6, 'Ambulancia');

INSERT INTO incidente VALUES (1, 'Incendio en vivienda', '2026-11-03', 'Sauces Norte', 'Sauces Norte',
    'Antonio Canaletto, a 100 metros de la iglesia', 'SI', 1, 1, 3, 1);
INSERT INTO incidente VALUES (2, 'Accidente de tránsito con heridos', '2026-11-03', 'Sauces Norte',
    'Av. Universitaria y Miguel Riofrío', 'Frente a la gasolinera', 'SI', 2, 1, 3, 2);
INSERT INTO incidente VALUES (3, 'Ruido excesivo en vivienda', '2026-11-04', 'Centro de Amaluza', 'Centro de Amaluza',
    'Junto al parque central', 'NO', 2, 2, 1, 2);

INSERT INTO despacho VALUES (1, 1);
INSERT INTO despacho VALUES (1, 5);
INSERT INTO despacho VALUES (1, 6);
INSERT INTO despacho VALUES (2, 5);
INSERT INTO despacho VALUES (2, 6);
INSERT INTO despacho VALUES (3, 5);
```

### 6.3 Consultas de ejemplo

**a) Incidentes de nivel rojo**

```sql
SELECT id_incidente, descripcion, fecha
FROM incidente
WHERE id_nivel = 3;
```

**b) Incidentes con el nombre del operador que los atendió**

```sql
SELECT incidente.id_incidente, incidente.descripcion, operador.nombre
FROM incidente
JOIN operador ON incidente.id_operador = operador.id_operador;
```

**c) Instituciones despachadas al incidente 1**

```sql
SELECT institucion.nombre
FROM despacho
JOIN institucion ON despacho.id_institucion = institucion.id_institucion
WHERE despacho.id_incidente = 1;
```

**d) Teléfonos de un alertante**

```sql
SELECT alertante.nombre, telefono.numero
FROM alertante
JOIN telefono ON alertante.id_alertante = telefono.id_alertante
WHERE alertante.id_alertante = 1;
```

---

## 7. Herramientas utilizadas

- **MySQL** (en Docker) como gestor de base de datos
- **DBeaver** para ejecutar el SQL y generar el diagrama
- **draw.io** para el diagrama entidad-relación
