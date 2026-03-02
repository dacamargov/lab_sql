
<img src="https://raw.githubusercontent.com/Databricks-BR/lab_sql/main/images/header_handson_sql.png">

# Hands-On LAB 03 - Explorando el Query Profiler 

Entrenamiento Hands-on en la plataforma Databricks con enfoque en las funcionalidades de Analytics (SQL, Query, DataViz, Genie).


# Objetivos del Ejercicio

El objetivo de este laboratorio es explorar las funcionalidades del plan de ejecución de las consultas (Query Profiler), identificando los cuellos de botella y las oportunidades de mejora del rendimiento. </br>
</br>


<img src="https://github.com/Gabriel-Rangel/lab_sql/blob/main/images/modelagem.png?raw=true">

Vamos a utilizar el "Editor SQL".

## Ejercicio 03.01 - Creación de la Query

``` sql
CREATE OR REPLACE TABLE big_table AS
SELECT 
  ve.id_loja AS id_loja,
  lo.cod AS codigo_loja,
  lo.varejista AS varejista,
  lo.xpto AS nome_da_loja,
  lo.tipo AS tipo_de_loja,
  lo.cep AS cep,
  lo.lat_long AS lat_long,
  ve.id_produto AS id_produto,
  me.nome_medicamento AS nome_medicamento,
  me.categoria_regulatoria AS categoria_regulatoria,
  me.numero_registro_produto AS numero_registro_produto,
  me.data_vencimento_registro AS data_vencimento_registro,
  me.numero_processo AS numero_processo,
  me.classe_terapeutica AS classe_terapeutica,
  me.cod_empresa AS cod_empresa,
  me.empresa_detentora_registro AS empresa_detentora_registro,
  me.principio_ativo AS principio_ativo,
  es.id_estoque AS id_estoque,
  es.data_estoque AS data_estoque,
  es.estoque AS quantidade_estoque,
  ve.id_venda AS id_venda,
  ve.dt_venda AS data_venda,
  ve.qt_venda AS quantidade_venda,
  ve.vl_venda AS vl_venda
FROM vendas ve
LEFT JOIN dim_loja lo
  ON ve.id_loja = lo.cod
LEFT JOIN dim_medicamento me
  ON ve.id_produto = me.id_produto
LEFT JOIN estoque es
  ON me.id_produto = es.id_produto

```

## Ejercicio 03.02 - Visualizando el Historial de ejecución de las Consultas


En el Menú, elija la opción "Query History"

<img src="https://github.com/Gabriel-Rangel/lab_sql/blob/main/images/v2_lab03_1.png?raw=true" style="height: 300px;">

Filtre las Consultas (p. ej. Seleccione sus propias Queries):

<img src="https://github.com/Gabriel-Rangel/lab_sql/blob/main/images/v2_lab03_2.png?raw=true">

## Ejercicio 03.03 - Analizando el Query Profiler

<img src="https://github.com/Gabriel-Rangel/lab_sql/blob/main/images/v2_lab03_3.png?raw=true" style="height: 400px;">


## Ejercicio 03.04 - Analizando el Plan de Ejecución

<img src="https://github.com/Gabriel-Rangel/lab_sql/blob/main/images/v2_lab03_4.png?raw=true" style="height: 300px;">

<img src="https://github.com/Gabriel-Rangel/lab_sql/blob/main/images/v2_lab03_5.png?raw=true">


