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
<br>Se hicieron entonces análisis más sencillos: histogramas de la variable dependiente (haciendo la distinción de sus posibles resultados, _True_ o _False_) contra otras variables independientes (con pocas categorías para que se pudiera apreciar), además también se incluyó un _slider_ para poder manejar las horas (tiempo) y ver la variabilidad de la variable dependiente contra las independiente.

En la visualización creada podemos identificar que en la categoría _07d7df22_ se tiene el mayor número de clicks (1) independiente de la hora.
<br>![image](https://user-images.githubusercontent.com/67491368/199836336-30bed0e8-ce50-4227-bfee-55bbf9b29083.png)

<br>De igual manera podemos encontrar relativamente poca variabilidad en el comportamiento de las variables independientes contra la dependiente en cualquier momento del día, normalemente esta variabilidad es de aproximádamente 1 millón de clicks (cuando click = 1) y 2 millones de clicks (cuando click = 0). El slider también nos permite observar que al paso de los días (recordemos que se tienen 10 días de datos), el número de clicks disminuye, por ejemplo: en la hora 14102100 [`YYMMDDHH`] y en la hora 14102200 podemos ver que en casi todas las variables disminuye el número de clicks (click = 1 y click = 0).
<br>![image](https://user-images.githubusercontent.com/67491368/199836484-790b16dc-0b58-43bc-8c9a-37cb8dc9497c.png)
