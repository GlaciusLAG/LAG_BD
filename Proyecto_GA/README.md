# Diseño y estructura de BD en gestión escolar.

La arquitectura de esta base de datos ha sido normalizada para garantizar la integridad y persistencia de la información, asegurando que cada asistencia sea comprobable y cumpla con las reglas de la institución.

## :gear: Herramientas utilizadas:

* ### [dbdiagram](dbdiagram.io)
<table style="border: none;">
  <tr>
    <td>
      <h4>Plataforma online que trabaja con un Lenguaje Específico de Dominio (DSL) sencillo, para la generación de giagramas entidad-relación (ER).</h4>
    </td>
    <td align="left">
      <img src="img/dbdiagram_logo.png" width="100%" alt="dbdiagram_Logo">
    </td>
  </tr>
</table>

* ### [Google Sheets](https://workspace.google.com/intl/es-419_mx/products/sheets/)
(Explicación / Imágenes)

* ### [Visual Studio Code](https://code.visualstudio.com/download)
    + (Explicación / Imágenes)
    * Versión 1.109.5
    + Extensiones
        * PHP IntelliSense (Intelephense)
        * MySQL (Database Client)

* ### [XAMPP](https://www.apachefriends.org/es/download.html)
    + Exosistema (Explicación / Imágenes)
        - XAMPP (centro de control)
        - MySQL
        - phpMyAdmin
        - PHP
    * Versión (8.2.12 / PHP 8.2.12)

* ### [DB Browser for SQLite](https://sqlitebrowser.org/dl/)
    + (Explicación / Imágenes)
    * Versión 3.13.1

## :triangular_ruler: Diagrama Entidad-Relación

## :key: Creación de la BD en SQLite
<table>
  <tr>
    <td> Base de Datos
    </td>
    <td> <b>GestorAsistencia.db</b>
  </tr>
</table>

## Estructuras de las tablas:

- ### carrera
```sql
CREATE TABLE "carrera" (
	"carrera_id"	INTEGER NOT NULL,
	"nombre"	TEXT NOT NULL,
	PRIMARY KEY("carrera_id" AUTOINCREMENT)
);
```

---

- ### cuatrimestre
Uso de `CONSTRAINT` / `CHECK`
```sql
CREATE TABLE "cuatrimestre" (
	"cuatrimestre_id"	INTEGER NOT NULL,
	"no_cuatrimestre"	INTEGER NOT NULL,
	PRIMARY KEY("cuatrimestre_id" AUTOINCREMENT),
	CONSTRAINT "error_numero_invalido" CHECK("no_cuatrimestre" >= 1 AND "no_cuatrimestre" <= 9)
);
```

---

- ### docente
```sql
CREATE TABLE "docente" (
	"matricula_docente"	TEXT NOT NULL,
	"nombre"	TEXT NOT NULL,
	"apellido_p"	TEXT NOT NULL,
	"apellido_m"	TEXT,
	PRIMARY KEY("matricula_docente")
);
```

---

- ### materia
```sql
CREATE TABLE "materia" (
	"clave_materia"	TEXT NOT NULL,
	"nombre"	TEXT NOT NULL,
	"carrera_id"	INTEGER NOT NULL,
	"cuatrimestre_id"	INTEGER NOT NULL,
	PRIMARY KEY("clave_materia"),
	FOREIGN KEY("carrera_id") REFERENCES "carrera"("carrera_id"),
	FOREIGN KEY("cuatrimestre_id") REFERENCES "cuatrimestre"("cuatrimestre_id")
);
```

---

- ### ciclo_escolar

Uso de la clausula `GLOB`
```sql
CREATE TABLE "ciclo_escolar" (
	"ciclo_id"	TEXT NOT NULL,
	"fecha_inicio"	TEXT NOT NULL,
	"fecha_fin"	TEXT NOT NULL,
	"estatus"	INTEGER NOT NULL DEFAULT 1,
	PRIMARY KEY("ciclo_id"),
	CONSTRAINT "error_formato_ciclo" CHECK("ciclo_id" GLOB '[0-9][0-9][0-9][0-9]-[1-3]'),
	CONSTRAINT "error_fecha_fin" CHECK("fecha_inicio" < "fecha_fin"),
	CONSTRAINT "error_formato_fecha" CHECK("fecha_inicio" GLOB '[0-9][0-9][0-9][0-9]-[0-1][0-9]-[0-3][0-9]' AND "fecha_fin" GLOB '[0-9][0-9][0-9][0-9]-[0-1][0-9]-[0-3][0-9]')
);
```

---

- ### clase
```sql
CREATE TABLE "clase" (
	"clase_id"	INTEGER NOT NULL,
	"clave_materia"	TEXT NOT NULL,
	"matricula_profesor"	TEXT NOT NULL,
	"cuatrimestre_id"	INTEGER NOT NULL,
	"ciclo_id"	TEXT NOT NULL,
	PRIMARY KEY("clase_id" AUTOINCREMENT),
	FOREIGN KEY("ciclo_id") REFERENCES "ciclo_escolar"("ciclo_id"),
	FOREIGN KEY("clave_materia") REFERENCES "materia"("clave_materia"),
	FOREIGN KEY("cuatrimestre_id") REFERENCES "cuatrimestre"("cuatrimestre_id"),
	FOREIGN KEY("matricula_profesor") REFERENCES "docente"("matricula_docente")
);
```

---

- ### horario
Uso de la clausula `IN`

(`IN` como manejo de booleans)
```sql
CREATE TABLE "horario" (
	"horario_id"	INTEGER NOT NULL,
	"hora_inicio"	TEXT NOT NULL,
	"hora_fin"	TEXT NOT NULL,
	"dia_semana"	TEXT NOT NULL,
	"clase_id"	INTEGER NOT NULL,
	PRIMARY KEY("horario_id" AUTOINCREMENT),
	FOREIGN KEY("clase_id") REFERENCES "clase"("clase_id"),
	CONSTRAINT "error_formato_horario_inicio" CHECK("hora_inicio" GLOB '[0-2][0-9]:[0-5][0-9]:[0-5][0-9]'),
	CONSTRAINT "error_formato_horario_fin" CHECK("hora_fin" GLOB '[0-2][0-9]:[0-5][0-9]:[0-5][0-9]'),
	CONSTRAINT "error_horario_fin" CHECK("hora_inicio" < "hora_fin"),
	CONSTRAINT "error_dia_semana" CHECK("dia_semana" IN ('Lunes', 'Martes', 'Miercoles', 'Jueves', 'Viernes'))
);
```

---

- ### estudiante
```sql
CREATE TABLE "estudiante" (
	"matricula_id"	TEXT NOT NULL,
	"nombre"	TEXT NOT NULL,
	"apellido_p"	TEXT NOT NULL,
	"apellido_m"	TEXT,
	"fecha_nacimiento"	TEXT NOT NULL,
	"estatus"	INTEGER NOT NULL DEFAULT 1,
	"sincronizado"	INTEGER NOT NULL DEFAULT 0,
	PRIMARY KEY("matricula_id"),
	CONSTRAINT "error_estudiante_nacimiento" CHECK("fecha_nacimiento" GLOB '[0-9][0-9][0-9][0-9]-[0-1][0-9]-[0-3][0-9]'),
	CONSTRAINT "error_estudiante_estatus" CHECK("estatus" IN (0,1)),
	CONSTRAINT "error_estudiante_sincronizado" CHECK("sincronizado" IN (0,1)),
	CONSTRAINT "error_formato_matricula" CHECK("matricula_id" GLOB '[0-9][0-9]-[0-9][0-9][0-9][0-9][0-9][0-9][0-9]')
);
```

---

- ### carga_academica
Creación de `UNIQUE INDEX`
```sql
CREATE TABLE "carga_academica" (
	"carga_id"	INTEGER NOT NULL,
	"matricula_id"	TEXT NOT NULL,
	"clase_id"	INTEGER NOT NULL,
	"ciclo_id"	TEXT NOT NULL,
	"sincronizado"	INTEGER NOT NULL DEFAULT 0,
	PRIMARY KEY("carga_id" AUTOINCREMENT),
	FOREIGN KEY("ciclo_id") REFERENCES "ciclo_escolar"("ciclo_id"),
	FOREIGN KEY("clase_id") REFERENCES "clase"("clase_id"),
	FOREIGN KEY("matricula_id") REFERENCES "estudiante"("matricula_id"),
	CONSTRAINT "error_carga_academica_sincronizado" CHECK("sincronizado" IN (0, 1))
);

CREATE UNIQUE INDEX "idx_carga_unica" ON "carga_academica" ("matricula_id", "clase_id", "ciclo_id");
```