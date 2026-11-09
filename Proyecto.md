# INFORME DE FIN DE UNIDAD

**FEIRNNR - Carrera de Computación**
Universidad Nacional de Loja

## Datos Generales

| Campo | Detalle |
|---|---|
| **Asignatura** | Base de datos |
| **Ciclo** | 3 A |
| **Unidad** | 1 |
| **Resultado de aprendizaje de la unidad** | Modela bases de datos de problemas reales, bajo los principios de solidaridad, transparencia, responsabilidad y honestidad. |
| **Nombre del Docente** | Edwin René Guamán Quinche |
| **Fecha** | 30/10/2026 |
| **Enlace de Git** | |
| **Nombres y apellidos del estudiante** | Ariana Arlette Quezada Fernández |

---

## 1. TÍTULO

Diseño e implementación de una base de datos para la gestión de incidentes de emergencia y el despacho de instituciones de respuesta en la provincia de Loja.

---

## 2. CONTEXTO DEL PROBLEMA

Los servicios integrados de emergencia, como el ECU 911, reciben llamadas de ciudadanos que reportan accidentes, incendios, delitos u otras situaciones de riesgo en distintos cantones de la provincia de Loja. Cada llamada la atiende un operador, que debe registrar quién llama, dónde ocurre la emergencia, qué tan grave es y cuáles instituciones deben acudir: bomberos, SUCOD, militares, gestión de riesgos, policía nacional o ambulancia.

Sin una base de datos, esta información se registra de forma dispersa o manual. Eso dificulta saber cuántos incidentes se atendieron, en qué cantones ocurren más, qué nivel de urgencia tuvieron o qué institución respondió. También puede duplicar datos, perder referencias de ubicación y complicar la coordinación cuando un mismo incidente requiere varias instituciones a la vez (multidespacho).

El problema afecta a los ciudadanos que piden ayuda (los alertantes), a los operadores que atienden las llamadas, a las instituciones que responden en el lugar y a la organización que administra el servicio y necesita datos confiables para tomar decisiones.

Los datos a gestionar son los de cada incidente (fecha, descripción, dirección de la emergencia y referencia), el alertante (nombre, teléfono y dirección desde donde llama), el operador que atiende, el cantón, el nivel de alarma (verde si puede esperar, naranja si es medio y rojo si es una urgencia) y las instituciones despachadas. Manejarlos correctamente es importante porque un error o una demora en el registro puede retrasar la ayuda en una situación donde cada minuto cuenta.

---

## 3. OBJETIVOS

### 3.1. Objetivo general

Diseñar e implementar una base de datos relacional, mediante el modelo entidad-relación y el lenguaje SQL, para registrar y organizar los incidentes de emergencia atendidos en la provincia de Loja, con el fin de mejorar el control de las llamadas, la ubicación de las emergencias y el despacho de las instituciones de respuesta.

### 3.2. Objetivos específicos

1. Elaborar el diagrama entidad-relación y el modelo relacional del sistema, identificando las entidades (incidente, alertante, operador, cantón, nivel de alarma e institución), sus atributos, llaves y cardinalidades, y representando el multidespacho como una relación de muchos a muchos.
2. Construir el esquema en SQL con sus llaves primarias y foráneas, y comprobar su funcionamiento con datos de ejemplo y consultas sobre los incidentes, los cantones y las instituciones despachadas.

---

## 4. MODELOS CONCEPTUALES DE BASES DE DATOS

### 4.1. Modelo Entidad-Relación (E-R)

> *Nota: el diagrama original es una imagen. Aquí se transcribe como descripción y como diagrama Mermaid.*

<img width="1197" height="719" alt="image" src="https://github.com/user-attachments/assets/d223d137-2657-418e-8119-97845ec2f4b9" />

*Figura 1. Diagrama entidad-relación del sistema de gestión de emergencias ECU 911.*

### 4.2. Modelo Relacional
<img width="1123" height="490" alt="image" src="https://github.com/user-attachments/assets/ddc96767-08ed-424b-a3c5-82f300a3b951" />


*Figura 2. Modelo relacional del sistema de gestión de emergencias ECU 911.*

---

## 5. CÓDIGO SQL

### 5.1. Creación de Tablas (DDL)

```sql
DROP DATABASE IF EXISTS ECU;
CREATE DATABASE ECU;
USE ECU;

CREATE TABLE INSTITUCION (
  id_institucion INT NOT NULL,
  nombre VARCHAR(50) NOT NULL UNIQUE,
  PRIMARY KEY (id_institucion)
);

CREATE TABLE CANTON (
  id_canton INT NOT NULL,
  nombre VARCHAR(50) NOT NULL UNIQUE,
  PRIMARY KEY (id_canton)
);

CREATE TABLE NIVEL_ALARMA (
  id_nivelalarma INT NOT NULL,
  significado VARCHAR(50) NOT NULL,
  color VARCHAR(20) NOT NULL UNIQUE,
  PRIMARY KEY (id_nivelalarma)
);

CREATE TABLE OPERADOR (
  id_operador INT NOT NULL,
  nombre VARCHAR(50) NOT NULL,
  PRIMARY KEY (id_operador)
);

CREATE TABLE ALERTANTE (
  id_alertante INT NOT NULL,
  nombre VARCHAR(50) NOT NULL,
  telefono VARCHAR(15) NOT NULL,
  PRIMARY KEY (id_alertante)
);

CREATE TABLE INCIDENTE (
  id_incidente INT NOT NULL,
  direccion_alertante VARCHAR(100) NOT NULL,
  referencia VARCHAR(100),
  direccion_emergencia VARCHAR(100) NOT NULL,
  fecha DATE NOT NULL,
  descripcion VARCHAR(255) NOT NULL,
  id_canton INT NOT NULL,
  id_nivelalarma INT NOT NULL,
  id_operador INT NOT NULL,
  id_alertante INT NOT NULL,
  PRIMARY KEY (id_incidente),
  FOREIGN KEY (id_canton) REFERENCES CANTON(id_canton),
  FOREIGN KEY (id_nivelalarma) REFERENCES NIVEL_ALARMA(id_nivelalarma),
  FOREIGN KEY (id_operador) REFERENCES OPERADOR(id_operador),
  FOREIGN KEY (id_alertante) REFERENCES ALERTANTE(id_alertante)
);

CREATE TABLE DESPACHA (
  id_institucion INT NOT NULL,
  id_incidente INT NOT NULL,
  PRIMARY KEY (id_institucion, id_incidente),
  FOREIGN KEY (id_institucion) REFERENCES INSTITUCION(id_institucion),
  FOREIGN KEY (id_incidente) REFERENCES INCIDENTE(id_incidente)
);
```

### 5.2. Inserción de Datos (DML)

```sql
INSERT INTO INSTITUCION (id_institucion, nombre) VALUES
  (1, 'Bomberos'),
  (2, 'SUCOD'),
  (3, 'Militares'),
  (4, 'Gestión de Riesgos'),
  (5, 'Policía Nacional'),
  (6, 'Ambulancia');

INSERT INTO CANTON (id_canton, nombre) VALUES
  (1, 'Loja'),
  (2, 'Calvas'),
  (3, 'Espíndola'),
  (4, 'Catamayo'),
  (5, 'Macará'),
  (6, 'Saraguro');

INSERT INTO NIVEL_ALARMA (id_nivelalarma, significado, color) VALUES
  (1, 'Puede esperar', 'verde'),
  (2, 'Medio', 'naranja'),
  (3, 'Urgencia', 'rojo');

INSERT INTO OPERADOR (id_operador, nombre) VALUES
  (1, 'Carlos Armijos'),
  (2, 'María Jaramillo'),
  (3, 'Luis Ordóñez'),
  (4, 'Andrea Torres'),
  (5, 'Diego Cueva');

INSERT INTO ALERTANTE (id_alertante, nombre, telefono) VALUES
  (1, 'Juan Pérez', '0991112233'),
  (2, 'Rosa Morocho', '0984455667'),
  (3, 'Pedro Castillo', '0997788990'),
  (4, 'Lucía Valarezo', '0981234567'),
  (5, 'Marco Reyes', '0998765432'),
  (6, 'Elena Sarango', '0987654321');

INSERT INTO INCIDENTE (id_incidente, direccion_alertante, referencia,
                       direccion_emergencia, fecha, descripcion,
                       id_canton, id_nivelalarma, id_operador, id_alertante) VALUES
  (1, 'Sauces Norte, calle Antonio Canaletto', 'A 100 metros de la iglesia',
      'Sauces Norte, calle Antonio Canaletto', '2026-10-05',
      'Incendio en vivienda', 1, 3, 1, 1),
  (2, 'Barrio San Pedro, calle Colón', 'Frente a la gasolinera',
      'Vía Loja-Catamayo, sector El Tambo', '2026-10-07',
      'Accidente de tránsito con heridos', 4, 3, 2, 2),
  (3, 'Av. Universitaria y Miguel Riofrío', 'Junto al banco',
      'Av. Universitaria y Miguel Riofrío', '2026-10-09',
      'Robo a local comercial', 1, 2, 1, 3),
  (4, 'Cariamanga, barrio Centro, calle Sucre', 'Cerca del mercado',
      'Cariamanga, barrio Centro, calle Bolívar', '2026-10-12',
      'Deslizamiento de tierra que bloquea la vía', 2, 3, 3, 4),
  (5, 'Barrio El Valle, calle 18 de Noviembre', 'Frente a la farmacia',
      'Barrio El Valle, calle 18 de Noviembre', '2026-10-15',
      'Persona con crisis de salud', 1, 2, 4, 5),
  (6, 'Amaluza, calle Principal', 'Junto a la plaza central',
      'Amaluza, calle Principal', '2026-10-18',
      'Ruido molesto en vivienda', 3, 1, 5, 6),
  (7, 'Saraguro, barrio Centro', 'Camino a la comunidad',
      'Saraguro, comunidad Lagunas', '2026-10-20',
      'Incendio forestal', 6, 3, 2, 5),
  (8, 'Macará, barrio San Vicente', 'Cerca del parque central',
      'Macará, calle Bolívar y Sucre', '2026-10-22',
      'Persona extraviada', 5, 1, 3, 2);

INSERT INTO DESPACHA (id_institucion, id_incidente) VALUES
  (1, 1), (5, 1), (6, 1),
  (1, 2), (5, 2), (6, 2),
  (5, 3),
  (1, 4), (4, 4), (6, 4),
  (6, 5),
  (5, 6),
  (1, 7), (3, 7), (4, 7),
  (5, 8);
```

---

## CONSULTAS DE ÁLGEBRA RELACIONAL Y TRADUCCIÓN A SQL

### Consulta 1 — Operador JOIN

**Pregunta 1:** ¿Cuántos incidentes de nivel rojo ocurrieron en el cantón Loja?

**Álgebra Relacional:**

```text
S1 = incidente ⋈ nivel_alarma
S2 = S1 ⋈ canton
π nombre, color (σ nombre == "Loja" and color == "rojo" (S2))
```

**SQL:**

```sql
USE ECU;
SELECT c.nombre AS canton, na.color
FROM INCIDENTE inc
JOIN CANTON c ON inc.id_canton = c.id_canton
JOIN NIVEL_ALARMA na ON inc.id_nivelalarma = na.id_nivelalarma
WHERE c.nombre = 'Loja' AND na.color = 'rojo';
```

**Resultado esperado:** la consulta devuelve el cantón Loja y el nivel de alarma rojo para comprobar los incidentes con esa coincidencia.

**Tabla Relational:**

| nombre | color |
|---|---|
| Loja | rojo |

**Tabla SQL:**

| canton | color |
|---|---|
| Loja | rojo |

---

### Consulta 2 — Operador Diferencia (−)

**Pregunta 2:** ¿Qué operadores no han atendido incidentes de nivel rojo?

**Álgebra Relacional:**

```text
S1 = incidente ⋈ nivel_alarma
S2 = π id_operador (σ color == "rojo" (S1))
S3 = π id_operador (operador) - S2
S4 = S3 ⋈ operador
π id_operador, nombre (S4)
```

**SQL:**

```sql
SELECT o.id_operador, o.nombre
FROM OPERADOR o
WHERE o.id_operador NOT IN (
    SELECT inc.id_operador
    FROM INCIDENTE inc
    JOIN NIVEL_ALARMA na ON inc.id_nivelalarma = na.id_nivelalarma
    WHERE na.color = 'rojo'
);
```

**Resultado esperado:** se obtiene la lista de operadores (Andrea Torres y Diego Cueva) que no han estado a cargo de llamadas de nivel rojo.

**Tabla Relational:**

| id_operador | nombre |
|---|---|
| 5 | Diego Cueva |
| 4 | Andrea Torres |

**Tabla SQL:**

| id_operador | nombre |
|---|---|
| 4 | Andrea Torres |
| 5 | Diego Cueva |

---

### Consulta 3 — Operador Unión (∪)

**Pregunta 3:** ¿Qué instituciones han respondido a incidentes de nivel rojo o a incidentes de nivel naranja?

**Álgebra Relacional:**

```text
S1 = incidente ⋈ nivel_alarma
S2 = S1 ⋈ despacha
S3 = π id_institucion (σ color == "rojo" (S2))
S4 = π id_institucion (σ color == "naranja" (S2))
S5 = S3 ∪ S4
S6 = S5 ⋈ institucion
π id_institucion, nombre (S6)
```

**SQL:**

```sql
SELECT i.id_institucion, i.nombre
FROM INSTITUCION i
JOIN DESPACHA d ON i.id_institucion = d.id_institucion
JOIN INCIDENTE inc ON d.id_incidente = inc.id_incidente
JOIN NIVEL_ALARMA na ON inc.id_nivelalarma = na.id_nivelalarma
WHERE na.color = 'rojo'
UNION
SELECT i.id_institucion, i.nombre
FROM INSTITUCION i
JOIN DESPACHA d ON i.id_institucion = d.id_institucion
JOIN INCIDENTE inc ON d.id_incidente = inc.id_incidente
JOIN NIVEL_ALARMA na ON inc.id_nivelalarma = na.id_nivelalarma
WHERE na.color = 'naranja';
```

**Resultado esperado:** se obtiene el conjunto consolidado y sin duplicados de 5 instituciones (Policía Nacional, Militares, Gestión de Riesgos, Bomberos y Ambulancia).

**Tabla Relational:**

| id_institucion | nombre |
|---|---|
| 5 | Policía Nacional |
| 3 | Militares |
| 4 | Gestión de Riesgos |
| 1 | Bomberos |
| 6 | Ambulancia |

**Tabla SQL:**

| id_institucion | nombre |
|---|---|
| 1 | Bomberos |
| 5 | Policía Nacional |
| 6 | Ambulancia |
| 4 | Gestión de Riesgos |
| 3 | Militares |

---

### Consulta 4 — Operador División (÷)

**Pregunta 4:** ¿Qué instituciones participaron en todos los incidentes rojos?

**Álgebra Relacional:**

```text
S1 = incidente ⋈ nivel_alarma
S2 = π id_incidente (σ color == "rojo" (S1))
S3 = π id_incidente, id_institucion (despacha)
S4 = S3 ÷ S2
S5 = S4 ⋈ institucion
π id_institucion, nombre (S5)
```

**SQL:**

```sql
SELECT i.id_institucion, i.nombre
FROM INSTITUCION i
WHERE NOT EXISTS (
    SELECT inc.id_incidente
    FROM INCIDENTE inc
    JOIN NIVEL_ALARMA na ON inc.id_nivelalarma = na.id_nivelalarma
    WHERE na.color = 'rojo'
    AND NOT EXISTS (
        SELECT 1
        FROM DESPACHA d
        WHERE d.id_institucion = i.id_institucion
          AND d.id_incidente = inc.id_incidente
    )
);
```

**Resultado esperado:** devuelve únicamente la institución *Bomberos*, ya que participó en la totalidad de las emergencias de urgencia roja.

**Tabla Relational:**

| id_institucion | nombre |
|---|---|
| 1 | Bomberos |

**Tabla SQL:**

| id_institucion | nombre |
|---|---|
| 1 | Bomberos |

---

### Consulta 5 — Operador Renombrado (ρ)

**Pregunta 5:** Del incidente rojo de Saraguro, ¿quién lo atendió, quién lo reportó y qué instituciones respondieron?

**Álgebra Relacional:**

```text
S1  = incidente ⋈ nivel_alarma
S2  = σ color == "rojo" (S1)
S3  = S2 ⋈ canton
S4  = σ nombre == "Saraguro" (S3)
S5  = π id_incidente, descripcion, id_operador, id_alertante (S4)
S6  = S5 ⋈ operador
S7  = ρ nombre➡operador (S6)
S8  = S7 ⋈ alertante
S9  = ρ nombre➡alertante (S8)
S10 = S9 ⋈ despacha
S11 = S10 ⋈ institucion
π id_incidente, descripcion, operador, alertante, telefono, nombre (S11)
```

**SQL:**

```sql
SELECT
    inc.id_incidente,
    inc.descripcion,
    o.nombre AS operador,
    a.nombre AS alertante,
    a.telefono,
    i.nombre AS institucion
FROM INCIDENTE inc
JOIN NIVEL_ALARMA na ON inc.id_nivelalarma = na.id_nivelalarma
JOIN CANTON c ON inc.id_canton = c.id_canton
JOIN OPERADOR o ON inc.id_operador = o.id_operador
JOIN ALERTANTE a ON inc.id_alertante = a.id_alertante
JOIN DESPACHA d ON inc.id_incidente = d.id_incidente
JOIN INSTITUCION i ON d.id_institucion = i.id_institucion
WHERE na.color = 'rojo'
  AND c.nombre = 'Saraguro';
```

**Resultado esperado:** se muestran los detalles del incidente 7 (incendio forestal), identificando al operador (María Jaramillo), al alertante (Marco Reyes) y las tres instituciones despachadas.

**Tabla Relational:**

| id_incidente | descripcion | operador | alertante | telefono | nombre |
|---|---|---|---|---|---|
| 7 | Incendio forestal | María Jaramillo | Marco Reyes | 998765432 | Militares |
| 7 | Incendio forestal | María Jaramillo | Marco Reyes | 998765432 | Gestión de Riesgos |
| 7 | Incendio forestal | María Jaramillo | Marco Reyes | 998765432 | Bomberos |

**Tabla SQL:**

| id_incidente | descripcion | operador | alertante | telefono | institucion |
|---|---|---|---|---|---|
| 7 | Incendio forestal | María Jaramillo | Marco Reyes | 0998765432 | Bomberos |
| 7 | Incendio forestal | María Jaramillo | Marco Reyes | 0998765432 | Militares |
| 7 | Incendio forestal | María Jaramillo | Marco Reyes | 0998765432 | Gestión de Riesgos |

---

## CONCLUSIONES

- **Conclusión 1 (Modelado conceptual):** Diseñar el diagrama Entidad-Relación y pasarlo al modelo relacional nos ayudó a entender mejor cómo organizar la información del ECU 911. Lo más complejo fue manejar el multidespacho, ya que una emergencia puede requerir varias instituciones y una institución atiende muchos casos (M:N). Resolver esto con la tabla intermedia `DESPACHA` fue clave para evitar datos duplicados y mantener todo bien conectado.

- **Conclusión 2 (Implementación SQL):** Escribir el código SQL (DDL) definiendo llaves primarias, foráneas y reglas como `NOT NULL` o `UNIQUE` aseguró que los datos fueran coherentes y no quedaran incompletos. Además, insertar registros de prueba con DML sirvió para comprobar en la práctica que el diseño funcionaba bien al simular llamadas e incidentes reales.

- **Conclusión 3 (Álgebra Relacional):** Plantear las consultas primero en álgebra relacional nos ayudó a razonar la lógica paso a paso antes de pasarlas a SQL. Aprender a usar operadores como la diferencia (−) o la división (÷) resultó fundamental para resolver preguntas más complejas, como saber qué instituciones atendieron todos los incidentes rojos o qué operadores no estuvieron en emergencias graves.
