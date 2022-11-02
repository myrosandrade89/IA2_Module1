# **Modelo inteligente de clasificación | PySpark**

_Autor: Myroslava Sánchez Andrade A01730712_

_Fecha de creación: 25/10/2022_

_Última modificación: 27/10/2022_

---

## **ETL**

Los datos fueron extraídos de la plataforma Kaggle, del set de datos **[Click-Through Rate Prediction, Kaggle](https://www.kaggle.com/competitions/avazu-ctr-prediction/overview)**. En la publicidad en línea, la CTR (Click Through Rate) es una de las métricas más utilizadas para evaluar el rendimiento de los anuncios; este conjunto de datos contiene 10 días de datos de 'clicks', con un tamaño de 5.87 GB (40428967 instancias).

- **_Variables independientes:_** id, hour, C1 (annonymized variable), banner_pos, site_id, site_domain, site_category, app_id, ap_domain, app_category, device_id, device_ip, device_model, device_type, device_conn_type, C14-C21 (annonymized variables)
  <br>Siendo categóricas las siguientes variables: `['C1', 'C14', ..., 'C21']`

- **_Varianle dependiente:_** click (0/1 for non-click/click)

El conjunto de datos fue cargado a GoogleDrive para que estos pudieran ser pocesados en GoogleColab (IA2_Module1_ETL).

Después de realizar las configuraciones básicas de PySpark (librerías, sesión de trabajo) y de GoogleColab, se leyó y almacenó el conjunto de datos.
<br>Las transformaciones que se llevaron a acabo fueron: eliminación de columnas con valores únicos (identificadores únicos) y codificación de columnas categóricas.
<br>Para la preparación de los datos, se hizo una división del 80% para el entrenamiento y 20% para el test (randomSplit)
<br>Por último, estos dos archivos (train y test) fueron exportados como archivos .csv a la misma carpeta de GoogleDrive.

## **Modelado**

Teniendo los archivos, train.csv y test.csv, se configuró y entrenó un modelo en el archivo IA2_Module1_Model.
De igual manera que en el proceso de ETL, se realizaron las configuraciones básicas de PySpark y GoogleColab. Se decidió utilizar como modelo el **Gadrient-boosted tree classifier**, este es un algoritmo basado en árboles (es un conjunto de árboles de decisión) que son utilizados para problemas de clasificación y regresión.

Una vez entrenado y obtenido el modelo de clasificación, se realizó la predicción del archivo de prueba, este dio una presición del %.
