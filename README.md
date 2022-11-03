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

Una vez entrenado y obtenido el modelo de clasificación, se realizó la predicción del archivo de prueba, este dio una presición del 83.19%.

## **Tablero de análisis | Tableau**

En el archivo `CTR_Data_Analysis.twb` se realizó un tablero de visualización del conjunto de datos con la herramienta _Tableau_. Dado que todas las variables son categóricas y la mitad de las variables son anónimas, fue un poco complicado realizar un análisis profundo. En un principio se tenía planeado realizar una matriz de correlación de Pearson, pero los recursos de mi computadora portátil no fueron suficientes para llevar a cabo este procesamiento, se interrumpía el proceso.
<br>Se hicieron entonces análisis más sencillos: histogramas de la variable dependiente (haciendo la distinción de sus posibles resultados, _True_ o _False_) contra otras variables independientes (con pocas categorías para que se pudiera apreciar) y áreas de las horas (en 10 días) contra el número de clicks; además también se incluyó un _slider_ para poder manejar las horas (tiempo) y ver la variabilidad de la variable dependiente contra las independiente, y la selección del valor de click (0 o 1).
<br>![image](https://user-images.githubusercontent.com/67491368/199842061-98c99d6f-cccc-41f5-adbc-c16828382c3d.png)


En la visualización creada podemos identificar que en la categoría _07d7df22_ se tiene el mayor número de clicks (1) independiente de la hora.
<br>![image](https://user-images.githubusercontent.com/67491368/199842223-daf5779d-bbdf-418c-9c7c-0038b52608be.png)

<br>En la visualización del área, podemos notar un claro patrón: primero recordemos que la variable `hora`está en el formato `YYMMDDHH`, identificado las fechas de cada pico (tanto cuando $click = 1$ y $click = 0$) estas son diarias en las primeras horas del día (las personas se encuentran más expuestas en las madrugadas casi todos los días). De hecho, también podemos notar que hay una disminución notoria en $hour = 14102420$, ese día (24 de octubre del 2014) es un viernes y tiene sentido porque es cuando empieza el fin de semana.
<br>![image](https://user-images.githubusercontent.com/67491368/199843932-78529aa4-4fff-4bae-ae95-6cec338392c1.png)

