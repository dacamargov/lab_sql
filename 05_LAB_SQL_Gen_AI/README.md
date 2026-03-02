<img src="https://raw.githubusercontent.com/Databricks-BR/lab_sql/main/images/header_handson_sql.png">

# Hands-On LAB 07 - Funcionalidades de Generative AI en SQL

Entrenamiento Hands-on en la plataforma Databricks con enfoque en las funcionalidades de Analytics (SQL, Query, Dask, DataViz, SQL end-point).

Ejercicio 07.01 - Sobre las Funcionalidades:

Una forma de aplicar los modelos de IA Generativa es utilizar las Databricks AI SQL Functions.

Estas permiten el uso de SQL, un lenguaje ampliamente utilizado por analistas de datos y de negocio, para ejecutar una LLM sobre nuestras bases de datos corporativas. Con esto, también podemos crear nuevas tablas con la información extraída para ser utilizadas en nuestros análisis de manera más sencilla.

Existen funciones nativas para ejecutar tareas predefinidas o enviar cualquier instrucción deseada para ser ejecutada. A continuación se describen:



| Gen AI SQL Function | Descrição |
| -- | -- |
| [ai_analyze_sentiment](https://docs.databricks.com/pt/sql/language-manual/functions/ai_analyze_sentiment.html) | Análisis de Sentimiento |
| [ai_classify](https://docs.databricks.com/pt/sql/language-manual/functions/ai_classify.html) | Clasificación de Texto |
| [ai_extract](https://docs.databricks.com/pt/sql/language-manual/functions/ai_extract.html) | Extracción de Terminos |
| [ai_fix_grammar](https://docs.databricks.com/pt/sql/language-manual/functions/ai_fix_grammar.html) | Corrección de Gramática |
| [ai_gen](https://docs.databricks.com/pt/sql/language-manual/functions/ai_gen.html) | Generación de Textos | 
| [ai_mask](https://docs.databricks.com/pt/sql/language-manual/functions/ai_mask.html) | Marcado de datos sensibles |
| [ai_query](https://docs.databricks.com/pt/sql/language-manual/functions/ai_query.html) | Consultas Gen Ai |
| [ai_similarity](https://docs.databricks.com/pt/sql/language-manual/functions/ai_similarity.html) | Análisis de Similitudes |
| [ai_summarize](https://docs.databricks.com/pt/sql/language-manual/functions/ai_summarize.html) | Resumen de Textos |
| [ai_translate](https://docs.databricks.com/pt/sql/language-manual/functions/ai_translate.html) | Traducción de Textos |


</br></br>
Para este ejercicio, vamos a explorar las funcionalidades mencionadas, según el ejemplo a continuación:

**ai_translate**
``` md
SELECT texto as original, 
       ai_translate(texto, 'br') as traducao
  FROM (select 'Hello, how are you?' as texto)
;
```
</br>

**ai_extract**

``` md
SELECT ai_extract(
    'Envie un email para jane.doe@example.com sobre la reunión que marcamos a las 10am.',
    array('email', 'time')
  );
```

**ai_mask**.
``` md

SELECT ai_mask(
    'Me llamo Flavio Da Silva. Contácteme al 555-1234 o visítenos en Av. Paulista, 1000.,
    array('phone', 'address')
);
```

**ai_gen**
``` md
SELECT principio_ativo
     , ai_gen(concat('Proporcione más detalles sobre cómo el medicamento ',principio_ativo,' actúa en el organismo de una persona adulta. Brinde un texto de hasta 50 palabras.')) 
  FROM dbacademy.<seu_database>.dim_medicamento 
 LIMIT 10
 ;
```

Ahora vamos a tomar nuestra tabla de medicamentos,</br>
crear una nueva columna asignando comentarios a cada medicamento de forma aleatoria usando la función **ai_gen </br>
y luego crear una nueva columna con el análisis de sentimiento de la columna de comentarios usando la función **ai_analyze_sentiment.

``` md
WITH 
sizing AS (SELECT * FROM dbacademy.<seu_database>.dim_medicamento LIMIT 10),
comentarios AS (
    SELECT
    CASE
            WHEN rand() > 0.5 THEN ai_gen(CONCAT('Escriba en español un comentario positivo sobre el medicamento: ', nome_medicamento))
            ELSE ai_gen(CONCAT('Invente un comentario negativo sobre el medicamento en español', nome_medicamento,'. Ese medicamento no existe. sea criativo'))
    END AS medicamento_review
    FROM sizing
)
    SELECT
    *,
    ai_analyze_sentiment(medicamento_review) AS sentimento_review
    FROM comentarios;
```
**ai_fix_grammar**.

``` md
SELECT ai_fix_grammar('Estamos com fome. Vamos fazer uma pausa e comer um pão de queijo mineiro bem gostoso?');
```

</br></br>
</br></br>


