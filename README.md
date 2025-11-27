Análisis No Supervisado con K-Means para Segmentación de Clientes
------------------------------------------------------------------

Integrantes: Daniel Fernandez, Giovanni Ortiz 
Proyecto: Scoring Crediticio (Home Credit)

------------------------------------------------------------------
1. Descripción de la técnica utilizada y justificación
------------------------------------------------------------------

En esta actividad aplicamos la técnica de aprendizaje no supervisado K-Means con el objetivo de analizar y segmentar a los clientes del dataset de Home Credit. 

Elegimos K-Means porque:
- Permite identificar grupos homogéneos de clientes de forma clara y eficiente.
- Facilita la interpretación de segmentos gracias a los centroides.
- Es adecuado para datasets grandes como los de Home Credit.
- Complementa el modelo supervisado, permitiendo analizar subpoblaciones con riesgo diferenciado.
- Ayuda a detectar posibles sesgos, patrones ocultos y diferencias de comportamiento entre segmentos.

Todo el análisis se realizó exclusivamente sobre el conjunto de entrenamiento para evitar data leakage, tal como exige el enunciado.

------------------------------------------------------------------
2. Instrucciones de ejecución del código
------------------------------------------------------------------

1. Asegurar que el archivo application_train.parquet (o equivalente) está ubicado en la carpeta /data del proyecto.
2. Editar en el script:
   - PATH_APPLICATION_TRAIN → ruta correcta del archivo.
   - PD_COL → dejar en None si aún no existe un score supervisado.
   - K_FINAL → ajustar después de revisar los resultados de búsqueda de K.
3. Ejecutar el script:

   python unsupervised_kmeans.py

El script realiza automáticamente:
- Carga del dataset de entrenamiento.
- Selección y preprocesamiento de variables.
- Escalamiento y codificación one-hot.
- Búsqueda del mejor número de clusters mediante inercia y coeficiente de silueta.
- Entrenamiento de K-Means.
- Asignación de una etiqueta de cluster a cada cliente.

Archivos generados:
- kmeans_k_search_results.csv
- kmeans_cluster_summary.csv
- kmeans_cluster_profile_numeric.csv
- application_train_with_clusters.parquet

------------------------------------------------------------------
3. Análisis e interpretación de resultados
------------------------------------------------------------------


Tras evaluar varios valores de K, seleccionamos el número final de clusters basándonos en:
- Método del codo (inercia).
- Coeficiente de silueta.
- Interpretabilidad de los grupos.

Los clusters encontrados mostraron diferencias claras entre segmentos de clientes. De forma general:

- Algunos clusters agrupan clientes con ingresos más altos, mayor estabilidad laboral y tasas de mora más bajas.
- Otros clusters muestran perfiles más riesgosos, como ingresos más bajos, mayor utilización relativa del crédito o valores más débiles en las variables externas EXT_SOURCE.
- La distribución de mora por cluster permitió identificar subpoblaciones con riesgo significativamente mayor.
- En caso de contar con la PD del modelo supervisado, también se puede comparar PD promedio vs. mora real para detectar subestimación o sobreestimación del riesgo.

K-Means permitió descubrir patrones que no eran evidentes en un análisis supervisado tradicional.

------------------------------------------------------------------
4. Discusión sobre incorporación al proyecto final
------------------------------------------------------------------

El uso de K-Means puede aportar valor al proyecto de scoring crediticio en distintos niveles:

Aporte al modelo supervisado:
- El cluster asignado a cada cliente puede usarse como una feature adicional.
- Permite detectar segmentos donde el modelo supervisado podría estar calibrado de forma imperfecta.
- Facilita revisión de subpoblaciones con mejor o peor desempeño.

Aporte al negocio:
- Permite crear estrategias diferenciadas por segmento (políticas de crédito, tasas, condiciones).
- Identifica grupos de mayor riesgo que requieren monitoreo más frecuente.
- Ayuda a detectar patrones de comportamiento que no estaban claros en un análisis tradicional.

Limitaciones:
- K-Means asume que los clusters son aproximadamente esféricos.
- Requiere estandarización y selección de ciertas variables para funcionar correctamente.
- La elección de K siempre tiene un componente de juicio humano.

Conclusión:
K-Means sí puede incorporarse al proyecto final debido a que aporta segmentación, análisis de riesgo por subpoblación y una mejor comprensión de los clientes. Su uso fortalece tanto el modelo supervisado como la interpretación del portafolio crediticio.
