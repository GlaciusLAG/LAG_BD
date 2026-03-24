<p align="center">
  <a href="https://git.io/typing-svg"><img src="https://readme-typing-svg.demolab.com?font=Doto&size=50&duration=925&pause=200&color=F7E618&center=true&vCenter=true&width=435&lines=Gestor;de;Asistencia;(+%CD%A1%C2%B0+%CD%9C%CA%96+%CD%A1%C2%B0)" /></a>
  <br>
  <a href="https://git.io/typing-svg"><img src="https://readme-typing-svg.demolab.com?font=Doto&size=44&duration=100&pause=200&color=F7EA53&center=true&vCenter=true&width=435&lines=LUMEN;++UMEN;L+MEN;LU+EN;LUM+N;LUME+;LUMEN;LUM+N;LU+EN;L+MEN;+UMEN;LUMEN;%F0%9F%92%A1LUMEN%F0%9F%92%A1;LUMEN;%F0%9F%92%A1LUMEN%F0%9F%92%A1" /></a>
</p>

La arquitectura de esta base de datos ha sido normalizada para garantizar la integridad y persistencia de la información, asegurando que cada asistencia sea comprobable y cumpla con las reglas de la institución.
<p align='center'>
	<img src=https://img.shields.io/badge/SQLite-%23003B57?style=plastic&logo=SQLite&logoColor=white&link=https%3A%2F%2Fsqlite.org%2F>
	<img src=https://img.shields.io/badge/dbdiagram-%23003D8F?style=plastic&logo=diagrams.net&logoColor=white&link=https%3A%2F%2Fdbdiagram.io%2Fhome>
	<img src=https://img.shields.io/badge/Google_Sheets-%2334A853?style=plastic&logo=google%20sheets&logoColor=white&link=https%3A%2F%2Fsheets.google.com>
	<img src=https://img.shields.io/badge/XAMPP-%23FB7A24?style=plastic&logo=XAMPP&logoColor=white&link=https%3A%2F%2Fwww.apachefriends.org%2Fes%2Findex.html>
	<img src=https://img.shields.io/badge/DB_Browser-%23343839?style=plastic&link=https%3A%2F%2Fsqlitebrowser.org>
</p>

## :gear: Herramientas utilizadas:

* ### [dbdiagram](dbdiagram.io)

Plataforma online que trabaja con un Lenguaje Específico de Dominio (DSL) sencillo (DBML), para la generación de diagramas entidad-relación (ER).
![dbdiagram](img/dbdiagram.png)

* ### [Google Sheets](https://workspace.google.com/intl/es-419_mx/products/sheets/)

<p>
  <a href="https://htmlpreview.github.io/?https://github.com/GlaciusLAG/LAG_BD/blob/main/Proyecto_GA/src/diccionario.html" target="_blank">
    <img src="https://img.shields.io/badge/Diccionario_de_Datos-HTML-orange?style=for-the-badge&logo=html5&logoColor=white" />
  </a>
</p>

* ### [Visual Studio Code](https://code.visualstudio.com/download)
    * Versión 1.109.5
    + Extensiones
        * PHP IntelliSense (Intelephense)
        * MySQL (Database Client)

|![VisualStudioCode](img/visual.png)

* ### [XAMPP](https://www.apachefriends.org/es/download.html)
    + Ecosistema
        - XAMPP (centro de control)
        - MySQL
        - phpMyAdmin
        - PHP
    * Versión (8.2.12 / PHP 8.2.12)
  
![XAMPP](img/xampp.png)
![XAMPP_PANEL](img/xampp2.png)

* ### [DB Browser for SQLite](https://sqlitebrowser.org/dl/)
    * Versión 3.13.1

![DB_Browser](img/dbbrowser.png)
![DB_Browser](img/dbbrowser2.png)
![DB_Browser](img/dbbrowser3.png)

## :triangular_ruler: Diagrama Entidad-Relación


<p>
  <a href="src/diagrama.pdf" target="_blank">
    <img src="img/diagrama.png" alt="Ver Diagrama PDF" width="400" style="border: 1px solid #eee;">
    <br>
  </a>
</p>

## :key: Creación de la BD en SQLite

<p>
  <a href="https://github.com/GlaciusLAG/LAG_BD/raw/main/Proyecto_GA/src/tu_archivo.db">
    <img src="https://img.shields.io/badge/Descargar_Base_de_Datos-SQLite-003B57?style=for-the-badge&logo=sqlite&logoColor=white" />
  </a>
</p>

<table>
  <tr>
    <td> Base de Datos
    </td>
    <td> <b>GestorAsistencia.db</b>
  </tr>
</table>

###	Conceptos importantes en la base de datos.
- GLOB
- IN
- LIKE
- CHECK
- CONSTRAINT
- sqlite_secuence

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

---

- ### asistencia
```sql
CREATE TABLE "asistencia" (
	"asistencia_id"	INTEGER NOT NULL,
	"universal_id"	TEXT NOT NULL,
	"matricula_id"	TEXT NOT NULL,
	"fecha"	TEXT NOT NULL,
	"hora"	TEXT NOT NULL,
	"opcion_menu"	TEXT NOT NULL,
	"clase_id"	INTEGER NOT NULL,
	"sincronizado"	INTEGER NOT NULL DEFAULT 0,
	PRIMARY KEY("asistencia_id" AUTOINCREMENT),
	FOREIGN KEY("clase_id") REFERENCES "clase"("clase_id"),
	FOREIGN KEY("matricula_id") REFERENCES "estudiante"("matricula_id"),
	CONSTRAINT "error_formato_fecha" CHECK("fecha" GLOB '[0-9][0-9][0-9][0-9]-[0-1][0-9]-[0-3][0-9]'),
	CONSTRAINT "error_formato_hora" CHECK("hora" GLOB '[0-2][0-9]:[0-5][0-9]:[0-5][0-9]'),
	CONSTRAINT "error_asistencia_opcion_menu" CHECK("opcion_menu" IN ('entrada', 'salida', 'horario')),
	CONSTRAINT "error_asistencia_sincronizado" CHECK("sincronizado" IN (0,1))
);
```

---

- ### usuario
```sql
CREATE TABLE "usuario" (
	"usuario_id"	INTEGER NOT NULL,
	"nombre_usuario"	TEXT NOT NULL,
	"contrasena"	TEXT NOT NULL,
	"rol"	TEXT NOT NULL,
	PRIMARY KEY("usuario_id" AUTOINCREMENT),
	CONSTRAINT "error_nombre_rol" CHECK("rol" IN ('consultor', 'admin'))
);
```