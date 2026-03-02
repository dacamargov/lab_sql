<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/header_genie.png">

# Hands-On LAB 06 - Creando el Dashboard AI/BI

Entrenamiento Hands-on en la plataforma Databricks con enfoque en las funcionalidades de Análisis Exploratorio y Paneles.
</br></br>

## Objetivos del Ejercicio

El objetivo de este laboratorio es crear un Panel, utilizando los datos de acciones de las Big Tech de la NASDAQ.</br>
***Si aún no lo ha hecho, cargue los datos según lo descrito en el Lab 02 - LAB_Notebook.***
</br></br>


## Ejercicio 06.01 - Creando el Dashboard

En el Menú Lateral, elija la opción DASHBOARDS:

Haga clic en la opción **CREATE DASHBOARD**

En la pantalla del Dashboard, haga clic en la PESTAÑA **Data** para agregar una fuente de datos:

<img src="https://raw.githubusercontent.com/Databricks-BR/lab_sql/main/images/lab05_ai_01.png" style="height: 300px;"></br>

Escoja la opción *"Add data source"*

<img src="https://github.com/Gabriel-Rangel/lab_sql/blob/main/images/v2_lab05_5.png?raw=true" style="height: 300px;">

Digite la palabra de busca **"<seu_database>.stock_bigtech"** escoja la tabla y despues vuelva para el área del **CANVAS**

Haga clic en el menú desplegable para agregar una nueva visualización. </br>
Seleccione con el mouse el área donde se ubicará el gráfico.

<img src="https://raw.githubusercontent.com/Databricks-BR/lab_sql/main/images/lab05_ai_04.png" style="height: 200px;">

</br></br>
En el campo de texto del ASISTENTE (Generative AI), haga la siguiente solicitud:
``` md
Gráfico de líneas del valor de cierre por día y por empresa
```

<img src="https://raw.githubusercontent.com/Databricks-BR/lab_sql/main/images/lab05_ai_05.png" style="height: 100px;"></br>


<img src="https://github.com/Gabriel-Rangel/lab_sql/blob/main/images/v2_lab05_1.png?raw=true" width="800px">
</br></br></br>

## Exercício 02.03 - Adicionando um FILTRO de página

Haga click en el menu azul en el incone de FILTRO.</br>
Escoja el atributo (Field):  "**company**"
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_06.png" width="850px">
</br></br></br>


## Ejercicio 02.04 - Cambiando el título del panel por una imagen

Ahora cree un nuevo objeto del tipo TEXT. En el cuadro que se creó </br>
inserte el código (markdown) a continuación: </br>
</br>

``` sql
![image](https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/header_painel.png)
```

</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_07.png" width="700px">
</br></br></br>


Organice el diseño del dashboard para que tenga la apariencia de la imagen a continuación.</br>
Realice la alineación adecuada del gráfico en el diseño.</br>
Cambie el nombre del Dashboard en la barra superior.</br>
Haga clic en el botón **Publish** para publicar el Panel.
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_08.png" width="700px">
</br></br></br>


## Ejercicio 02.05 - Creando un NUEVO contexto de datos

Ahora vamos a crear un nuevo contexto de datos.</br>
Para ello, ingrese a la opción **SQL Editor** en el MENÚ lateral de Databricks, </br>
seleccione el Catálogo y el Schema en la barra superior del Editor de Query,</br>
y escriba el texto a continuación:</br>

``` 
Seleccione el nombre de la empresa, stock,
valor mínimo de cierre, valor máximo de cierre
y porcentaje de variación entre el valor mínimo y el valor máximo de cierre
de la tabla dbacademy.<su_nombre>.stock_bigtech
agrupando por empresa y stock.
Use la columna company para obtener el nombre de la empresa.
```
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_09.png" width="700px">
</br></br></br>

Agregue al resultado la fila de concatenación con el nombre de la acción (STOCK), </br>
con el LINK (URL) de una imagen. </br>

``` sql

SELECT 
  "https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/" || stock || ".png" AS image,
  company,
  stock,
  MIN(close) AS min_close,
  MAX(close) AS max_close,
  ((MAX(close) - MIN(close)) / MIN(close) * 100) AS percentual_variacao
FROM dbacademy.<seu_database>.stock_bigtech
GROUP BY company, stock;

```
</br>
Al ejecutar la query (botón RUN),</br>
el resultado esperado es el mostrado a continuación:</br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_10.png" width="700px">
</br></br></br>

## Ejercicio 02.06 - Agregando el NUEVO contexto de datos al Dashboard

Ahora vamos a agregar el nuevo contexto de datos (calculado en la Query);</br>
como otra fuente de datos en el Panel.</br>
Para ello, regrese a la opción **Dashboards** en el menú lateral de Databricks,</br>
Elija el nombre del Panel que se creó,</br>
y haga clic en el botón (combo box) de **Published** a **Draft**.
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_10b.png" width="700px">
</br></br></br>

Haga click en la opción "**Data**".</br>
En el iten "Create another Dataset"</br>
Haga click en el botón "**+Create from SQL**".</br>

</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_11.png" width="700px">
</br></br></br>

Haga el Copy&Paste del código SQL generado en el ejercicio anterior:

``` sql

SELECT 
  "https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/" || stock || ".png" AS image,
  company,
  stock,
  MIN(close) AS min_close,
  MAX(close) AS max_close,
  ((MAX(close) - MIN(close)) / MIN(close) * 100) AS percentual_variacao
FROM dbacademy.<seu_database>.stock_bigtech
GROUP BY company, stock;

```
Y despues haga click en el botón "**RUN**"
</br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_12.png" width="700px">
</br></br></br>

## Ejercicio 02.07 - Agregando un nuevo Gráfico con el nuevo contexto de datos

1. Haga clic en el menú azul desplegable en la posición inferior del panel, </br>
en el botón con el ícono de gráfico </br>
2.En la barra de configuración (lado derecho del panel),</br>
elija el nombre del Dataset (que proviene de la Query SQL).</br> 
3. Configure el tipo de Visualización a "Tabla" (Table).</br>
4. Marque la opción para incluir TODOS los campos en la tabla.
5. En la configuración, haga clic en el campo (columna) con el nombre de "**image**".</br>
</br></br>
<img src="https://github.com/Gabriel-Rangel/lab_sql/blob/main/images/v2_lab05_2.png?raw=true" width="700px">
</br></br></br>

6. En la opción "Display", configúrela como "image".
7. En la opción "SIZE", coloque el valor "25" en el campo "width".
8. En la opción "Default column width", coloque el valor "150".
</br></br>
<img src="https://github.com/Gabriel-Rangel/lab_sql/blob/main/images/v2_lab05_3.png?raw=true" width="700px">
</br></br></br>

Como resultado esperado, tendremos la figura a continuación.</br>
Guarde (Publique) nuevamente el Panel.
</br></br>
<img src="https://github.com/Gabriel-Rangel/lab_sql/blob/main/images/v2_lab05_4.png?raw=true" width="700px">
</br></br></br>






