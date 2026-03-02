<img src="https://raw.githubusercontent.com/Databricks-BR/lab_sql/main/images/header_handson_sql.png">

# Hands-On LAB 04 - Creación de una ALERTA

Entrenamiento Hands-on en la plataforma Databricks con enfoque en las funcionalidades de Analytics (SQL, Query, Dask, DataViz, SQL end-point).


## Objetivos del Ejercicio

El objetivo de este laboratorio es explorar las funcionalidades de creación de una ALERTA
</br></br>

## Ejercicio 04.01 - Creando la Alerta

Vamos a utilizar la opción del menú "ALERTS".

<img src="https://github.com/Gabriel-Rangel/lab_sql/blob/main/images/v2_lab04_1.png?raw=true" style="height: 200px;">

</br></br>

Haga clic en el botón CREATE ALERT

El SQL Editor ahora está integrado con la Alerta, copie la query a continuación y haga clic en RUN
``` sql

SELECT close AS valor_apple
FROM dbacademy.SEU_NOME.stock_bigtech
WHERE stock = 'AAPL'
ORDER BY date DESC;

```
<img src="https://github.com/Gabriel-Rangel/lab_sql/blob/main/images/v2_lab04_2.png?raw=true">
</br></br>

La query ejecutándose con éxito habilitará la parte de configuración a la izquierda, configure según la imagen a continuación

* No olvide nombrar su alerta: Sugerencia: "Query_Alert"+ <SU_LOGIN> y haga clic en CREATE

* En el campo Notify coloque el correo electrónico usado para crear la cuenta de Databricks Free Edition


<img src="https://github.com/Gabriel-Rangel/lab_sql/blob/main/images/v2_lab04_3.png?raw=true" style="height: 700px;">


