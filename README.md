# Análisis No Supervisado con K-Means

Este documento resume el trabajo realizado para la actividad práctica EA3, donde se aplica un método de aprendizaje no supervisado sobre el conjunto de entrenamiento del proyecto de scoring crediticio. El objetivo es identificar patrones o segmentos de clientes que ayuden a complementar el modelo supervisado.

-----------------------

## 1. Técnica utilizada y motivo de la elección

Se utilizó K-Means para agrupar clientes del dataset de entrenamiento en segmentos basados en variables demográficas y financieras.

Motivos de la elección:
- Forma grupos homogéneos de clientes.
- Es simple de interpretar.
- Permite comparar la tasa de mora entre segmentos.
- Facilita identificar posibles sesgos del modelo supervisado.
- Funciona bien con la cantidad de datos del proyecto Home Credit.

-----------------------

## 2. Cómo ejecutar el código

El archivo principal es:

unsupervised_kmeans.py

-----------------------

### Requisitos:

Instalar dependencias:

pip install pandas numpy scikit-learn pyarrow

-----------------------

Archivos necesarios:
Debe existir en la misma carpeta:

application_.parquet

-----------------------
Este archivo contiene el conjunto de entrenamiento con la variable TARGET.

Ejecución:
python unsupervised_kmeans.py
El script generará estos archivos:

kmeans_k_search_results.csv
Tabla con los valores de inercia y silhouette para distintos K.

kmeans_cluster_summary.csv
Cantidad de clientes por cluster, tasa de mora y porcentaje del total.

kmeans_cluster_profile_numeric.csv
Promedios de las variables numéricas utilizadas para el clustering.

application_with_clusters.parquet
Dataset original con la columna CLUSTER_KMEANS.

-----------------------

3. Análisis y resultados
Selección del número de clusters
Se evaluaron valores de K entre 2 y 8. Para cada uno se calculó:

Inercia

Coeficiente de silueta

-----------------------

El script selecciona automáticamente el K con mejor silhouette, que luego se utiliza en el modelo final.

Segmentos obtenidos
El análisis produce clusters que difieren en:

Nivel de ingresos

Antigüedad laboral

Indicadores externos de riesgo (EXT_SOURCE)

Monto del crédito solicitado

Tasa de mora

-----------------------

El archivo kmeans_cluster_summary.csv permite identificar clusters de bajo, medio y alto riesgo.
Si existe una columna con la probabilidad del modelo supervisado, también se calcula la PD media por cluster.

-----------------------

Observaciones generales (reemplazables por tus resultados)
Un cluster suele mostrar ingresos más altos y menor mora.

Otro cluster puede agrupar clientes jóvenes con mayor riesgo.

Algunos clusters intermedios combinan ingresos medios con montos moderados.

-----------------------

4. Uso potencial dentro del proyecto final
Variable adicional
La etiqueta CLUSTER_KMEANS puede agregarse al modelo supervisado como una nueva variable.

-----------------------

Análisis de riesgo
Permite segmentar estrategias según perfil de cliente:

Políticas de préstamo

Límites de crédito

Estrategias de cobranza

-----------------------

Validación del modelo supervisado
Comparar mora real vs. PD por cluster permite detectar:

Subestimación de riesgo

Sobreestimación

Sesgos hacia ciertos grupos


-----------------------
Limitaciones
K-Means asume clusters esféricos.

La técnica depende de la escala y selección de variables.

La interpretación requiere revisar cada cluster manualmente.

-----------------------

5. Conclusión
K-Means permitió identificar segmentos diferenciados dentro de la cartera del dataset de entrenamiento. Los clusters muestran diferencias claras en comportamiento y riesgo, lo cual complementa el modelo supervisado y ayuda a comprender mejor la estructura del dataset. La técnica es apropiada para el proyecto y puede ser utilizada como herramienta adicional de análisis, monitorización e incluso como variable en futuros modelos.
