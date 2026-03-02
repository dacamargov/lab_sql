<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/header_genie.png">

# Hands-On LAB 03 - Creando el AI/BI Genie

Entrenamiento Hands-on en la plataforma Databricks con enfoque en las funcionalidades de preguntas y respuestas usando lenguaje natural.

</br></br>

## Objetivos do Exercício

El objetivo de este laboratorio es usar el AI/BI Genie para permitir el análisis de datos de ventas utilizando únicamente **Español**.</br> 
***Si aún no lo ha hecho, cargue los datos según lo descrito en el Lab 02 - LAB_Notebook..***
</br></br>

## Ejercicio 03.00 - Preparación

1. En algunos momentos, utilizaremos el SQL Editor. Déjelo preparado en otra ventana y seleccione su database.

<img src="https://github.com/Gabriel-Rangel/lab_sql/blob/main/images/v2_genie_1.png?raw=true">

</br></br>

## Ejercicio 03.01 - Crear AI/BI Genie

Vamos a comenzar creando un Genie para hacer nuestras preguntas. Para ello, sigamos los pasos a continuación:

1. En el menú principal (a la izquierda), haga clic en `New` > `Genie space`

<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/genie_01.png" width=300><br><br>

2. Configure su Genie
    - Seleccione las seguintes tablas:
        - vendas
        - estoque
        - dim_medicamento
        - dim_loja
    - Haga click en `Create`

<img src="https://github.com/Gabriel-Rangel/lab_sql/blob/main/images/v2_genie_3.png?raw=true" width=300></br></br> 

3. Cambie el nombre de su Genie Space y su espacio debe quedar así:

<img src="https://github.com/Gabriel-Rangel/lab_sql/blob/main/images/v2_genie_4.png?raw=true" width=700> </br>

4. Haga clic en **Instructions** y asigne la siguiente instrucción para que la respuesta de la Genie sea en español

    ``` %md
    * responda en Español 
    ```
<img src="https://github.com/Gabriel-Rangel/lab_sql/blob/main/images/v2_genie_port.png?raw=true" width=500></br></br> 
</br></br>

## Ejercicio 03.02 - Haciendo preguntas al AI/BI Genie

¡Con nuestra Genie preparada, podemos comenzar a hacer nuestros análisis!

Basta usar el chat para hacer las preguntas a continuación:
- ¿Cuál fue la facturación en oct/22?
- Ahora, desglóselo por producto
- Mantenga solo los 10 productos con mayor facturación
- Me gustaría obtener el nombre del producto en lugar del id
- ¿Cuál es el total de productos vendidos en genéricos?
- ¿Cuál fue el valor total vendido de ansiolíticos?
- ¿Qué productos tuvieron una proporción de ventas por stock mayor a 0.8 en octubre de 2022?

<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/genie_05.png"><br><br>

Noten que, incluso con muy poco contexto, la Genie ya logró:
- Inferir cuáles son las tablas y columnas relevantes para responder nuestras preguntas
- Aplicar filtros y agregaciones
- Responder preguntas adicionales sobre una respuesta anterior
- Entender cómo utilizar jerg
- Combinar diferentes tablas
- Calcular métricas derivadas
¡Aprovechen para explorar y hacer preguntas adicionales!
</br>

## Ejercicio 03.03 - Usando comentarios y constraints

Aun así, pueden ocurrir escenarios donde necesitaremos proporcionar algún contexto adicional a la Genie para obtener respuestas más precisas.

Ahora, vamos a explorar algunas formas de ayudar a la Genie cuando identifiquemos alguna necesidad.

La primera de ellas es documentar nuestras tablas. Todos los comentarios que agregamos a las tablas son utilizados por la Genie para entender mejor qué representa cada dato.

Otra buena práctica es agregar constraints a la tabla, ayudando a la Genie a realizar la asociación de manera correcta entre las tablas.

¡Vamos a ver cómo funciona!

1. Haga la seguinte pregunta:
   ```%md 
    ¿Cuál es el valor total de ventas por tienda? Muestre el nombre de la tienda.
    ```

2. ¡UPS! Parece que la Genie no logró realizar la consulta.

3. La columna **xpto** de la tabla **dim_loja** es la columna que contiene la información del nombre de las tiendas; explique que contiene esa información y que la columna **id_loja** de la tabla **dim_loja** no es el mejor campo para hacer los cruces con la tabla de ventas. En realidad, la columna correcta es cod.
Vamos a hacer estas correcciones ejecutando el siguiente comando en el **SQL EDITOR**.

    ``` sql
    ALTER TABLE dim_loja ALTER COLUMN xpto COMMENT 'Nome da loja';
    ALTER TABLE dim_loja ALTER column cod SET NOT NULL;
    ALTER TABLE dim_loja ADD CONSTRAINT pk_dim_loja PRIMARY KEY (cod);
    ALTER TABLE vendas ADD CONSTRAINT fk_venda_dim_loja FOREIGN KEY (id_loja) REFERENCES dim_loja(cod);
    ```
</br>

<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/genie_06.png"><br><br>

3. Haga nuevamente la pregunta anterior<br><br>

¡Listo! Con esta información, la Genie ya puede responder nuestra pregunta correctamente.

Documentar sus tablas con comentarios es siempre una buena práctica, así como asignar claves primarias y foráneas!</br>
Esto ayuda a la comprensión, el descubrimiento y la reutilización de estos datos por otras personas. Además, contribuye a mejorar las respuestas de la Genie.
</br></br>



## Ejercicio 03.04 - Usando instrucciones

Como hemos visto, la Genie utiliza toda la documentación de nuestras tablas para poder responder nuestras preguntas. Sin embargo, por motivos de seguridad, ¡no tiene acceso a los datos en sí!
Por eso, para complementar el conocimiento que la Genie ya posee sobre nuestras bases de datos, ¡también podemos crear instrucciones!
**Instrucciones** no son más que un conjunto de oraciones en lenguaje natural que pueden explicar a la Genie información importante, como:
- Significado de abreviaturas y términos técnicos comúnmente utilizados en su empresa
- Formato del dato (por ejemplo, si los registros están en mayúsculas o minúsculas)
- Tratamientos necesarios para determinados campos
- Cálculos de métricas
¡Vamos a ver cómo funciona!

1. Haga la pregunta:
    ``` %md
    Calcule la cantidad de ítems vendidos para prescripción
    ```

2. ¡Me parece que el resultado no es correcto! En nuestra base, el término prescripción realmente no se menciona ninguna vez. Pero sucede que aquí consideramos como medicamentos de prescripción aquellos que no son genéricos. Por eso, agregue la siguiente instrucción:
    ``` %md
    * para calcular indicadores sobre prescripción use  categoria_regulatoria <> 'GENÉRICO'
    ```


<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/genie_07.png"> </br>

3. Haga nuevamente la pregunta anterior

¡Listo! ¡Ahora la Genie ya puede responder preguntas sobre prescripciones también!

</br></br>

## Ejercicio 03.05 - Usando funciones

Otro recurso que podemos utilizar para ayudar a la Genie con cálculos complejos son las funciones.

**Funciones** permiten guardar y parametrizar lógicas complejas dentro de nuestro catálogo para ser reutilizadas por otras personas y/o otras consultas de forma sencilla, incluso fuera de la Genie.

En nuestro contexto, las funciones también funcionarán como herramientas validadas y certificadas por los equipos responsables, que la Genie puede decidir utilizar en sus respuestas.

¡Vamos a ver en la práctica!

1. Haga la pregunta:
    ``` %md 
    ¿Cuál es la ganancia proyectada del AAS? 
    ```

2.¡Realmente, no tenemos información suficiente en nuestra base para responder a esta pregunta! Para ello, cree la función siguiente con la lógica del cálculo de la ganancia promedio proyectada de un producto, ejecutando el ejemplo a continuación en el **SQL EDITOR**:

    ``` sql
    CREATE OR REPLACE FUNCTION calc_lucro(medicamento STRING)
    RETURNS TABLE(nome_medicamento STRING, lucro_projetado DOUBLE)
    COMMENT 'Use esta função para calcular o lucro projetado de um medicamento'
    RETURN 
    SELECT
    m.nome_medicamento,
    sum(case when m.categoria_regulatoria == 'GENÉRICO' then 1 else 0.5 end * v.vl_venda) / sum(v.qt_venda) as lucro_projetado
    FROM vendas v
    LEFT JOIN dim_medicamento m
    ON v.id_produto = m.id_produto
    WHERE m.nome_medicamento = trim(upper(calc_lucro.medicamento))
    GROUP BY ALL
    ```

3. Agregue esta función a su Genie, muy similar a lo realizado en el ejercicio anterior, pero necesitamos hacer clic en la flecha hacia abajo al lado de `Add` y seleccionar la opción `SQL function`. </br>
<img src="https://github.com/Gabriel-Rangel/lab_sql/blob/main/images/v2_genie_9.png?raw=true">
</br>

4. Ahora elija la función que acabamos de crear seleccionando el catálogo, schema/database y la función.

<img src="https://github.com/Gabriel-Rangel/lab_sql/blob/main/images/v2_genie_10.png?raw=true">

4. Haga nuevamente la pregunta anterior

¡Listo! ¡Con esto, logramos calcular la ganancia promedio de nuestro producto!

<br><br>

# Felicitaciones!

¡Ha concluido el laboratorio del AI/BI Genie!

Ahora, ya sabe cómo utilizar la Genie para analizar sus datos usando únicamente lenguaje natural, además de los principales recursos para refinar su precisión y poder responder las preguntas más complejas.
