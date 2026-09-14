
# Unidad de Aprendizaje: Sistemas de almacenamiento de la información

**Módulo:** Gestión de base de datos
**RA1:** Reconoce los elementos de las bases de datos, analizando sus funciones y valorando la utilidad de los sistemas gestores.

**Contenidos básicos:**
1. Ficheros (planos, indexados y de acceso directo, entre otros).
2. Bases de datos: conceptos, usos y tipos según el modelo de datos y según la ubicación de la información.
3. Sistemas gestores de bases de datos: funciones, componentes y tipos.

---

## Indice <!-- omit from toc -->
- [1. Ficheros](#1-ficheros)
  - [1.1 Ficheros planos (secuenciales)](#11-ficheros-planos-secuenciales)
  - [1.2 Ficheros indexados](#12-ficheros-indexados)
  - [1.3 Ficheros de acceso directo (aleatorio o relativo)](#13-ficheros-de-acceso-directo-aleatorio-o-relativo)
  - [1.4 Comparativa de tipos de ficheros](#14-comparativa-de-tipos-de-ficheros)
  - [1.5 Limitaciones de los sistemas de ficheros frente a las bases de datos](#15-limitaciones-de-los-sistemas-de-ficheros-frente-a-las-bases-de-datos)
- [2. Bases de datos: conceptos, usos y tipos](#2-bases-de-datos-conceptos-usos-y-tipos)
  - [2.1 Concepto de base de datos](#21-concepto-de-base-de-datos)
  - [2.2 Usos de las bases de datos](#22-usos-de-las-bases-de-datos)
  - [2.3 Tipos de bases de datos según el modelo de datos](#23-tipos-de-bases-de-datos-según-el-modelo-de-datos)
  - [2.4 Tipos de bases de datos según la ubicación de la información](#24-tipos-de-bases-de-datos-según-la-ubicación-de-la-información)
- [3. Sistemas gestores de bases de datos (SGBD): funciones, componentes y tipos](#3-sistemas-gestores-de-bases-de-datos-sgbd-funciones-componentes-y-tipos)
  - [3.1 Concepto de SGBD](#31-concepto-de-sgbd)
  - [3.2 Funciones de un SGBD](#32-funciones-de-un-sgbd)
  - [3.3 Componentes (elementos) de un SGBD](#33-componentes-elementos-de-un-sgbd)
  - [3.4 Tipos de sistemas gestores de bases de datos](#34-tipos-de-sistemas-gestores-de-bases-de-datos)
- [4. Esquema-resumen](#4-esquema-resumen)
- [5. Glosario de términos clave](#5-glosario-de-términos-clave)
- [6. Actividades propuestas (autoevaluación)](#6-actividades-propuestas-autoevaluación)


## 1. Ficheros

Antes de la aparición de las bases de datos, la información se almacenaba organizada en **ficheros** (archivos), gestionados directamente por los programas de aplicación o por el sistema operativo. Un **fichero** es un conjunto de **registros**, y cada registro está formado por **campos**.

Según cómo se organizan y se accede a los registros dentro del fichero, distinguimos varios tipos:

### 1.1 Ficheros planos (secuenciales)

- Los registros se almacenan **uno detrás de otro**, en el orden en que se van grabando (o según un criterio de ordenación, como una clave).
- Para acceder a un registro concreto hay que **leer desde el principio** hasta encontrarlo (acceso secuencial).
- **Ventajas:** estructura simple, fácil de crear y mantener, muy eficiente cuando se procesan **todos** los registros uno tras otro (por ejemplo, generar un listado completo).
- **Inconvenientes:** acceso lento a un registro concreto cuando el fichero es grande, ya que hay que recorrerlo desde el inicio; las operaciones de inserción/borrado en medio del fichero son costosas.
- **Ejemplo de uso:** ficheros de texto plano (`.txt`, `.csv`), copias de seguridad, procesos por lotes (*batch*).

### 1.2 Ficheros indexados

- Además de los datos, se mantiene una **estructura de índice** (similar al índice de un libro) que asocia cada valor de una clave con la posición física del registro correspondiente en el fichero.
- Permiten **dos formas de acceso**: secuencial (recorriendo todos los registros) y **por clave** (a través del índice), combinando lo mejor de ambos mundos.
- El acceso mediante el índice es mucho más rápido que recorrer todo el fichero, ya que el índice suele organizarse en estructuras eficientes de búsqueda (por ejemplo, árboles).
- **Inconvenientes:** ocupan más espacio (hay que guardar el índice además de los datos) y hay que mantener el índice actualizado cada vez que se modifica el fichero, lo que añade cierta sobrecarga.
- **Ejemplo de uso:** ficheros ISAM (*Indexed Sequential Access Method*), muy usados en sistemas de gestión antiguos y como base de algunos motores de bases de datos.

### 1.3 Ficheros de acceso directo (aleatorio o relativo)

- Permiten acceder **directamente** a cualquier registro sin necesidad de recorrer los anteriores, normalmente calculando la posición física del registro a partir de su clave (por ejemplo, mediante una función *hash* o porque los registros tienen tamaño fijo y se calcula un desplazamiento).
- También se conocen como ficheros de **acceso aleatorio** o de **acceso relativo**.
- **Ventajas:** acceso muy rápido a un registro concreto, independientemente de su posición en el fichero.
- **Inconvenientes:** es necesario conocer o calcular la posición del registro; no son eficientes para procesar todos los registros de forma ordenada; puede haber colisiones si dos claves generan la misma posición (en el caso de acceso por *hash*).
- **Ejemplo de uso:** bases de datos que necesitan localizar rápidamente un registro concreto (por ejemplo, una cuenta bancaria a partir de su número de cuenta).

### 1.4 Comparativa de tipos de ficheros

| Tipo | Forma de acceso | Velocidad de acceso a un registro | Velocidad de proceso secuencial | Uso típico |
|---|---|---|---|---|
| **Plano (secuencial)** | Uno tras otro, desde el principio | Lenta | Muy rápida | Listados completos, copias de seguridad |
| **Indexado** | Secuencial o por índice | Rápida (vía índice) | Rápida | Sistemas con consultas frecuentes por clave |
| **Acceso directo** | Directa (cálculo de posición) | Muy rápida | Lenta/poco práctica | Localización puntual de registros concretos |

### 1.5 Limitaciones de los sistemas de ficheros frente a las bases de datos

El uso de ficheros gestionados de forma independiente por cada aplicación presenta problemas:

- **Redundancia e inconsistencia de datos** (el mismo dato duplicado en varios ficheros).
- **Dificultad para compartir información** entre distintas aplicaciones.
- **Dependencia entre datos y programas** (cualquier cambio en la estructura obliga a modificar el programa).
- **Falta de control centralizado de la seguridad y de la integridad**.
- **Problemas de acceso concurrente** (varios usuarios/programas accediendo a la vez).

Estas limitaciones son las que justifican la aparición de las **bases de datos** y de los **sistemas gestores de bases de datos (SGBD)**.

---

## 2. Bases de datos: conceptos, usos y tipos

### 2.1 Concepto de base de datos

Una **base de datos** es un conjunto de datos organizados, estructurados y relacionados entre sí, almacenados de forma conjunta, sin redundancias innecesarias, y diseñados para ser utilizados por diferentes usuarios y aplicaciones de forma simultánea.

A diferencia de un simple conjunto de ficheros, una base de datos:

- Está gestionada por un software específico (el **SGBD**).
- Los datos son **independientes** de los programas que los usan.
- Permite compartir la información entre varios usuarios y aplicaciones a la vez.
- Garantiza la **integridad**, **consistencia** y **seguridad** de los datos.

### 2.2 Usos de las bases de datos

Las bases de datos se utilizan prácticamente en cualquier ámbito donde haya que gestionar información de forma organizada:

- **Gestión empresarial:** clientes, proveedores, facturación, nóminas, inventario.
- **Administración pública:** censo, historiales médicos, registros civiles.
- **Comercio electrónico:** catálogos de productos, pedidos, usuarios.
- **Redes sociales y aplicaciones web:** perfiles de usuario, publicaciones, mensajes.
- **Sistemas científicos y de investigación:** almacenamiento de grandes volúmenes de datos experimentales.
- **Aplicaciones móviles y de escritorio:** almacenamiento local de configuración o datos del usuario.

### 2.3 Tipos de bases de datos según el modelo de datos

El **modelo de datos** define cómo se organizan, estructuran y relacionan los datos dentro de la base de datos.

| Modelo | Descripción | Ejemplos de SGBD |
|---|---|---|
| **Jerárquico** | Los datos se organizan en forma de árbol (relaciones padre-hijo, 1:N). Un hijo solo puede tener un padre. | IMS (IBM) |
| **En red** | Similar al jerárquico, pero un registro puede tener varios "padres" (relaciones N:M), formando una estructura en forma de grafo. | IDS, IDMS |
| **Relacional** | Los datos se organizan en **tablas** (filas o tuplas y columnas o atributos), relacionadas mediante claves. Es el modelo más utilizado en la actualidad. | MySQL, PostgreSQL, Oracle, SQL Server |
| **Orientado a objetos** | Los datos se representan como objetos con atributos y métodos, de forma similar a la programación orientada a objetos. | db4o, ObjectDB |
| **Objeto-relacional** | Modelo híbrido: es relacional, pero admite tipos de datos complejos y objetos. | PostgreSQL, Oracle |
| **NoSQL (no relacional)** | Modelos alternativos al relacional pensados para grandes volúmenes de datos y alta escalabilidad: documentales, clave-valor, basados en grafos o en columnas. | MongoDB (documental), Redis (clave-valor), Neo4j (grafos), Cassandra (columnas) |

> **Evolución histórica:** Jerárquico → En red → Relacional → Orientado a objetos / NoSQL. El modelo **relacional**, propuesto por E. F. Codd en 1970, sigue siendo el más extendido, aunque el modelo **NoSQL** ha ganado peso con el Big Data y las aplicaciones web de gran escala.

### 2.4 Tipos de bases de datos según la ubicación de la información

| Tipo | Descripción | Ventajas | Inconvenientes |
|---|---|---|---|
| **Centralizada** | Toda la información se almacena en un único lugar físico (un servidor o equipo). | Fácil de administrar, control y seguridad centralizados, menor coste inicial | Punto único de fallo, posibles cuellos de botella con muchos usuarios |
| **Distribuida** | La información está repartida en varios equipos o ubicaciones físicas distintas, aunque el usuario la percibe como una única base de datos. | Mayor disponibilidad y tolerancia a fallos, mejor rendimiento, escalabilidad | Mayor complejidad de diseño y administración, problemas de sincronización entre nodos |

Dentro de las bases de datos distribuidas se suelen distinguir además:

- **Replicadas:** existen copias idénticas de los datos en distintas ubicaciones (mejora la disponibilidad y el rendimiento de lectura).
- **Fragmentadas:** los datos se dividen en partes (fragmentos) que se reparten entre distintos nodos (cada nodo almacena solo una porción de los datos).
- **En la nube:** alojadas en servidores de un proveedor externo (AWS, Azure, Google Cloud), accesibles a través de Internet.

---

## 3. Sistemas gestores de bases de datos (SGBD): funciones, componentes y tipos

### 3.1 Concepto de SGBD

Un **Sistema Gestor de Bases de Datos (SGBD)**, en inglés *DBMS (Database Management System)*, es el software que permite **crear, definir, manipular y administrar** una base de datos, actuando como intermediario entre los usuarios/aplicaciones y los datos almacenados físicamente.

### 3.2 Funciones de un SGBD

- **Definición de la estructura de los datos:** permite crear tablas, especificar tipos de datos, relaciones y restricciones.
- **Manipulación de los datos:** inserción, consulta, actualización y eliminación de datos.
- **Independencia de los datos:** separa la estructura lógica de los datos de su almacenamiento físico, de modo que los programas no dependen de cómo se guardan realmente.
- **Reducción de la redundancia:** evita duplicar la misma información en distintos lugares.
- **Garantía de integridad y consistencia:** aplica reglas y restricciones para que los datos sean correctos y coherentes.
- **Control de acceso y seguridad:** define qué usuarios pueden acceder a qué datos y con qué permisos.
- **Control de la concurrencia:** gestiona el acceso simultáneo de múltiples usuarios sin conflictos ni pérdidas de información.
- **Gestión de transacciones:** garantiza que las operaciones se ejecuten de forma completa y correcta (propiedades **ACID**: Atomicidad, Consistencia, Aislamiento y Durabilidad).
- **Copias de seguridad y recuperación:** permite realizar *backups* y restaurar la base de datos ante fallos o pérdidas de datos.
- **Optimización de consultas:** determina la forma más eficiente de ejecutar cada consulta (uso de índices, orden de las operaciones).

### 3.3 Componentes (elementos) de un SGBD

| Componente | Función |
|---|---|
| **Motor de base de datos** | Núcleo del sistema; se encarga del almacenamiento, recuperación y actualización física de los datos. |
| **Lenguaje de Definición de Datos (DDL)** | Permite definir la estructura de la base de datos: tablas, tipos de datos, relaciones, restricciones (p. ej. `CREATE TABLE`). |
| **Lenguaje de Manipulación de Datos (DML)** | Permite consultar y modificar los datos: `SELECT`, `INSERT`, `UPDATE`, `DELETE`. |
| **Lenguaje de Control de Datos (DCL)** | Gestiona permisos y seguridad de acceso (`GRANT`, `REVOKE`). |
| **Diccionario de datos (catálogo del sistema)** | Almacena los **metadatos**: información sobre tablas, columnas, tipos, relaciones, usuarios y permisos. |
| **Gestor de transacciones** | Garantiza el cumplimiento de las propiedades ACID en cada operación. |
| **Gestor de la concurrencia** | Controla el acceso simultáneo de varios usuarios, evitando bloqueos e interbloqueos. |
| **Módulo de seguridad y autorización** | Controla la autenticación de usuarios y sus permisos sobre los datos. |
| **Módulo de copia de seguridad y recuperación** | Permite realizar copias de seguridad y restaurar la base de datos tras un fallo. |
| **Optimizador de consultas** | Analiza las consultas y decide la estrategia de ejecución más eficiente. |
| **Interfaz de usuario / API** | Permite a los usuarios y aplicaciones interactuar con el SGBD (línea de comandos, interfaz gráfica, conectores ODBC/JDBC). |

### 3.4 Tipos de sistemas gestores de bases de datos

Los SGBD se pueden clasificar según varios criterios:

- **Según el modelo de datos que implementan:** jerárquicos, en red, relacionales (MySQL, PostgreSQL, Oracle, SQL Server), orientados a objetos, objeto-relacionales y NoSQL (documentales, clave-valor, columnares, grafos).
- **Según el número de usuarios que soportan:**
  - **Monousuario:** solo un usuario puede acceder a la vez (p. ej. Microsoft Access en modo local).
  - **Multiusuario:** varios usuarios pueden acceder simultáneamente (p. ej. MySQL, Oracle, SQL Server).
- **Según la ubicación de los datos que gestionan:** SGBD centralizados y SGBD distribuidos.
- **Según la licencia:**
  - **Libres / de código abierto:** MySQL (Community), MariaDB, PostgreSQL, SQLite.
  - **Propietarios / comerciales:** Oracle Database, Microsoft SQL Server, IBM Db2.
- **Según el tamaño y ámbito de uso:**
  - **De escritorio / embebidos:** SQLite, Microsoft Access (pequeñas aplicaciones, poco volumen de datos).
  - **Empresariales / de gran capacidad:** Oracle, SQL Server, PostgreSQL, MySQL, Db2 (grandes volúmenes, alta concurrencia).

---

## 4. Esquema-resumen

```
Sistemas de almacenamiento de la información
│
├── 1. FICHEROS
│     ├── Planos (secuenciales)
│     ├── Indexados
│     └── De acceso directo (aleatorio/relativo)
│
├── 2. BASES DE DATOS
│     ├── Concepto y usos
│     ├── Según el MODELO DE DATOS
│     │     Jerárquico · En red · Relacional · Orientado a objetos ·
│     │     Objeto-relacional · NoSQL
│     └── Según la UBICACIÓN
│           Centralizada · Distribuida (replicada, fragmentada, en la nube)
│
└── 3. SGBD (Sistema Gestor de Bases de Datos)
      ├── Funciones: independencia, integridad, seguridad, concurrencia,
      │             transacciones (ACID), backup/recuperación, optimización
      ├── Componentes: motor, DDL, DML, DCL, diccionario de datos,
      │               gestor de transacciones, gestor de concurrencia,
      │               seguridad, backup/recovery, optimizador, interfaz
      └── Tipos: por modelo de datos, por nº de usuarios, por ubicación,
                 por licencia, por tamaño/ámbito de uso
```

---

## 5. Glosario de términos clave

- **Registro:** conjunto de campos que forman una unidad de información dentro de un fichero o tabla.
- **Campo:** cada uno de los datos elementales que componen un registro.
- **Índice:** estructura auxiliar que agiliza la búsqueda de registros por una clave.
- **SGBD/DBMS:** Sistema Gestor de Bases de Datos.
- **Metadatos:** datos que describen la estructura de otros datos.
- **Clave primaria (PK):** campo que identifica de forma única cada registro.
- **Clave ajena/foránea (FK):** campo que referencia la clave primaria de otra tabla.
- **Transacción:** conjunto de operaciones que se ejecutan como una unidad indivisible.
- **ACID:** Atomicidad, Consistencia, Aislamiento (Isolation) y Durabilidad.
- **Redundancia:** repetición innecesaria de datos.
- **Concurrencia:** acceso simultáneo de varios usuarios a los mismos datos.

---

## 6. Actividades propuestas (autoevaluación)

1. Explica la diferencia entre un fichero plano, uno indexado y uno de acceso directo, indicando una ventaja y un inconveniente de cada uno.
2. Explica la diferencia entre un sistema de ficheros y una base de datos, indicando al menos tres ventajas de esta última.
3. Pon un ejemplo real de empresa u organización que use una base de datos **distribuida** y explica por qué le conviene ese modelo frente a una centralizada.
4. Indica qué modelo de datos utilizarías para: (a) una red social con relaciones de amistad, (b) el catálogo de un supermercado, (c) el registro de sesiones de una web con millones de accesos por segundo.
5. Enumera los componentes de un SGBD y explica con tus palabras qué función cumple el **diccionario de datos**.
6. Clasifica los siguientes SGBD según su modelo de datos: MySQL, MongoDB, Neo4j, Redis, Oracle.
7. ¿Qué significa que un SGBD garantice la **independencia de los datos**? Pon un ejemplo.

---

