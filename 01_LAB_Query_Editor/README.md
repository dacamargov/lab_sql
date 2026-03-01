
<img src="https://raw.githubusercontent.com/Databricks-BR/lab_sql/main/images/header_handson_sql.png">

# Hands-On LAB 01 - Explorando el Editor de Consultas

Entrenamiento práctico en la plataforma Databricks con enfoque en las funcionalidades de Analytics (SQL, Consultas, Visualización de Datos, Genie).


## Objetivos del Ejercicio

El objetivo de este laboratorio es conocer las funcionalidades de consulta (Query) de la plataforma Databricks, utilizando el lenguaje SQL (y las interfaces visuales), explorando los potenciales Analíticos.
Los ejercicios deberán ejecutarse en la opción del Menú lateral "SQL Editor".

<img src="https://github.com/Gabriel-Rangel/lab_sql/blob/main/images/v2_lab01_1.png?raw=true">


</br></br></br>
## Sesión 01: Estructura de TABLAS, DATABASE y CATÁLOGOS

<img src="https://raw.githubusercontent.com/Databricks-BR/lab_sql/main/images/lab01_uc.png">


| Tópico | Comando |
| -- | -- |
| **Catálogo** | CREATE CATALOG <nome_catalogo> |
| **Schema** | CREATE DATABASE IF NOT EXISTS <nome_catalogo>.<nome_database>; |
| **Tabela** | CREATE OR REPLACE TABLE  <nome_catalogo>.<nome_database>.<nome_tabela>; |
| **View** |  CREATE OR REPLACE VIEW  <nome_catalogo>.<nome_database>.<nome_tabela> AS ...; |

#### Referencia::
* [Databricks Help - DDL Syntax](https://docs.databricks.com/sql/language-manual/sql-ref-syntax-ddl-create-table.html)

## Ejercicio 01.01 - Creación del catálogo y database

``` sql
--GRANT CREATE CATALOG ON METASTORE TO `account users`;
--CREATE CATALOG IF NOT EXISTS dbacademy;
USE CATALOG dbacademy;

CREATE DATABASE IF NOT EXISTS <seu_usuario>;
USE <seu_usuario>;
```

## Ejercicio 01.02 - Creación de la Tabla

En la primera consulta del laboratorio realizamos la creación de catálogo y database y utilizamos la cláusula USE, pero solo se persiste en tiempo de ejecución;
Siempre debemos asignar el nombre del catálogo y database antes del nombre de la tabla separado por "." (catalogo.<seu_usuario>.porte_empresa)
o podemos especificar el catálogo y database que queremos usar en el propio editor como se muestra en la imagen a continuación:
</br></br>
<img src="https://github.com/Gabriel-Rangel/lab_sql/blob/main/images/v2_lab01_setcatalago.png?raw=true">
</br></br>

3. Crear la siguiente tabla:

``` sql
CREATE OR REPLACE TABLE porte_empresa 
  ( id_natureza_juridica    INT     COMMENT "codigo del porte de la empresa",
    sig_natureza_juridica   STRING  COMMENT "sigla que representa la naturaleza jurídica de la empresa",
    desc_natureza_juridica  STRING  COMMENT "descripción de la naturaleza jurídica" )
COMMENT "Tabla auxiliar del tipo de naturaleza jurídica de las empresas";
```
</br>

 ## Ejercicio 01.03 - Insertando datos en la Tabla mediante SQL INSERT

 ``` sql
INSERT INTO porte_empresa VALUES (1, "MEI", "Microemprendedor Individual") ;
INSERT INTO porte_empresa VALUES (2, "EI", "Empresario Individual") ;
INSERT INTO porte_empresa VALUES (3, "SLU", "Sociedad Limitada Unipersonal") ;
INSERT INTO porte_empresa VALUES (4, "LTDA", "Sociedad Empresarial Limitada") ;
INSERT INTO porte_empresa VALUES (5, "SS", "Sociedad Simple") ;
INSERT INTO porte_empresa VALUES (6, "S/A", "Sociedad Anónima") ;
INSERT INTO porte_empresa VALUES (7, "NULL", "NULL") ;
```

 ## Ejercicio 01.04 - Verificando el contenido de la TABLA

 ``` sql
SELECT * 
FROM porte_empresa 
ORDER BY id_natureza_juridica;
```

 ## Ejercicio 01.05 - Modificando el contenido de la TABLA

 ``` sql
UPDATE porte_empresa  
SET desc_natureza_juridica = "OTROS" 
WHERE id_natureza_juridica = 7;
```

``` sql
SELECT * 
FROM porte_empresa
WHERE id_natureza_juridica = 7;
```

``` sql
DELETE 
FROM porte_empresa 
WHERE id_natureza_juridica = 7;
```

``` sql
SELECT * FROM porte_empresa;
```

## Ejercicio 01.06 - Liquid Clustering

El Liquid Clustering reemplaza la partición de tablas y el ZORDER para simplificar las decisiones de disposición de datos y optimizar el rendimiento de las consultas. Ofrece la flexibilidad de redefinir la clave de clustering sin reescribir los datos existentes, permitiendo que la disposición de los datos evolucione junto con las necesidades analíticas a lo largo del tiempo.

¿Pero cuándo aplicar liquid clustering?

*Tablas normalmente filtradas por columnas de alta cardinalidad.

*Tablas con gran distorsión en la distribución de datos.

*Tablas que crecen rápidamente y requieren mantenimiento y ajustes.

*Tablas con requisitos de escritura concurrente.

*Tablas con patrones de acceso que cambian con el tiempo.

*Tablas en las que una clave de partición típica podría dejar la tabla con muchas o pocas particiones.

La sintaxis para habilitar esta funcionalidad en una tabla ya creada es:
<span style="color:red"> NO EJECUTAR </span>
```sql
ALTER TABLE <table_name>
CLUSTER BY (<clustering_columns>)
```
Además, podemos dejar que la propia plataforma de datos inteligente de Databricks defina cuáles son las mejores columnas de nuestra tabla para ser definidas como clustering.
<span style="color:green"> EJECUTAREMOS ESTE EJEMPLO </span>
```sql
-- captura información antes de activar la funcionalidad
DESC EXTENDED porte_empresa;

ALTER TABLE porte_empresa
CLUSTER BY AUTO;

-- captura información después de activar la funcionalidad
DESC EXTENDED porte_empresa;
```
#### Referencias:
* [Databricks Liquid Clustering](https://docs.databricks.com/aws/pt/delta/clustering)
* [BLOG - Announcing Automatic Liquid Clustering](https://www.databricks.com/blog/announcing-automatic-liquid-clustering)

## Ejercicio 01.07 - Visualizando el Historial de Actualizaciones de la tabla

 ``` sql
DESCRIBE HISTORY porte_empresa ;
```

## Ejercicio 01.08 - Visualizando el contenido de la tabla en la versión anterior (TIME TRAVEL)

 ``` sql
SELECT * FROM porte_empresa VERSION AS OF 7;
```

## Ejercicio 01.09 - RESTAURANDO el contenido de la tabla a la versión anterior (TIME TRAVEL)

 ``` sql
RESTORE TABLE porte_empresa TO VERSION AS OF 7;
```

## Ejercicio 01.10 - Visualizando las propiedades de la Tabla

 ``` sql
DESCRIBE DETAIL porte_empresa ;
```

## Ejercicio 01.11 - Visualizando la información DETALLADA de la Tabla

 ``` sql
DESCRIBE TABLE EXTENDED porte_empresa;
```
