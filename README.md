# Definición y realización de los Experimentos
![Windows](https://img.shields.io/badge/Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white) ![TensorFlow](https://img.shields.io/badge/TensorFlow-%23FF6F00.svg?style=for-the-badge&logo=TensorFlow&logoColor=white) ![OpenCV](https://img.shields.io/badge/opencv-%23white.svg?style=for-the-badge&logo=opencv&logoColor=white) ![Matplotlib](https://img.shields.io/badge/Matplotlib-%23ffffff.svg?style=for-the-badge&logo=Matplotlib&logoColor=black) ![NumPy](https://img.shields.io/badge/numpy-%23013243.svg?style=for-the-badge&logo=numpy&logoColor=white) ![Jupyter Notebook](https://img.shields.io/badge/jupyter-%23FA0F00.svg?style=for-the-badge&logo=jupyter&logoColor=white)

Para todos los experimentos realizados se utilizó la [base de datos **CBIS-DDSM**](https://github.com/LuisGuillermoRL/EDA_CBIS-DDSM/blob/main/docs/sdata2017177.pdf) y mediante la exploración, etiquetado y obtención de imágenes de esta base de datos ([EDA](https://github.com/LuisGuillermoRL/EDA_CBIS-DDSM/tree/main)) fue posible realizar los experimentos que se describen más abajo. La base de datos cuenta con dos "anormalidades" encontradas en las mastografías: **Masas** y **Calcificaciones**; a su vez divididas por sus patologías: **Benignas** y **Malignas**. Cabe aclarar que aunque la base de datos cuenta con las mastografías completas, las máscaras o ROIs de los cuáles se extraen los parches (*Cropped images*) y los parches, solamente se trabajó con estos últimos y con un tamaño (*resize*) de $224 \times 224$, ya que en estos se presenta la información más relevante, es decir, no se necesita toda la mastografía completa para empezar a atacar la problemática. A continuación se muestran los parches (Cropped) con los que se realizaron los experimentos.

![Masas](./docs/Masas.png)

![Calc](./docs/Calc.png)

 En este repositorio se muestran algunos ejemplos y resultados de los experimentos realizados para el tema de Tesis. A continuación se definen estos y se ilustran algunos de estos.

* [**Experimento Masas**](./Mass_InV3.ipynb). Como su nombre lo indica, este experimento es de *clasificación binaria*: una clase es **Masa Benigna** y la otra **Masa Maligna**. Este experimento ayuda a detectar cáncer al utilizar solo las imágenes correspondientes a las **Masas**. Una vez entrenada alguna CNN se le puede ingresar un parche (que contenga una masas) del tamaño con el que fue entrenada ($224 \times 224$) y así poder obtener la clase de esta, es decir, benigna o maligna.

* [**Experimento Calcificaciones**](./Calc_CLAHE_DenseNet121.ipynb). De igual forma al experimento previamente descrito. este es de *clasificación binaria*: una clase es **Calcificación Benigna** y la otra **Calcificación Maligna**. Este experimento también ayuda a detectar cáncer al utilizar solo las imágenes correspondientes a las **Calcificaciones**. Una vez entrenada alguna CNN se le debe presentar un parche (que contenga una calcificaciones) del tamaño con el que fue entrenada ($224 \times 224$).

* [**Experimento Multiclase**](./4C_ResNet50.ipynb). Como se puede observar, hasta ahorita existen 4 clases: **Masa Benigna**, **Masa Maligna**, **Calcificación Benigna** y **Calcificación Maligna**. En este experimento para la detección de cáncer, las 4 clases (todas las imágenes) fueron utilizadas para entrenar las distintas CNNs, solo que aquí a estos modelos se les puede presentar un parche que contenga una masa o calcificaciones para así obtener la clase correspondiente.

* [**Experimento Malignas-Benignas (M-B)**](./M-B_CLAHE_VGG19.ipynb). Con el fin de utilizar todas las imágenes para detectar cáncer *de manera distinta* al **Experimento Multiclase**, en este se definieron dos clases: **Malignas** y **Benignas**, sin importar si son masas o calcificaciones (su anormalidad). Es decir, se juntaron Masas y Calcificaciones Malignas para representar la clase  **Malignas**  y se juntaron Masas y Calcificaciones Benignas para representar la clase  **Benignas**.

* [**Experimento Masas-Calcificaciones (M-C)**](./M-C_ResNet50.ipynb). Este experimento es de *clasificación binaria*: una clase es **Masa** y la otra **Calcificación**. En este experimento se usaron todas las imágenes, pero este no ayuda a detectar cáncer, ya que solo distingue entre Masas y Calcificaciones. Este fue realizado para evaluar el rendimiento de distintas CNNs así como para comparar los resultados obtenidos por [**Lai**](https://github.com/leoll2/MedicalCNN/tree/master) ya que este autor lo realizó.

De manera rápida se menciona lo siguiente:

* Se experimentó con cada una de las CNNS variando el número de Neuronas
* Debido a que no son muchas imágenes, se utilizó el **Image Data Generator de TensorFlow**, abreviado aquí por **IDG**. Este ayuda a evitar el *sobreajuste* y a elevar un poco la tasa de clasificación. Para esto se exploraron a la par distintos parámetros para el **IDG**, llamados en la tesis *Parámetros Propuestos* y *Parámetros Iniciales* (los que usó [**Lai**](https://github.com/leoll2/MedicalCNN/tree/master)). A continuación se ilustra una muestra de las operaciones realizadas a un parche con los *Parámetros Propuestos* indicados en el **IDG** de Tensorflow.

![Muestra](./docs/Muestra.png)

* También se experimentó sustituyendo la capa *Flatten* de las CNNs por la capa *Global Average Pooling 2D* (GAvgP2D), la cual sirve como un reductor de dimensionalidad (se ilustra abajo la idea de como funciona). 

![GAVGP2D](./docs/gavgp.png)

* La técnica de **CLAHE** también se les aplicó a las imágenes para tratar de ayudar a elevar la tasa de clasificación, para esto, se utilizó la librería de **OpenCV**, con el método *cv2.createCLAHE()* con los parámetros standar. A continuación se muestran distintos resultados al aplicarle CLAHE (con distintos parámetros) a un parche correspondiente a una masa.

![muestra CLAHE](./docs/CLAHE_mass.png)

* Se utilizó la técnica de *Ensamble: soft voting*, es decir, dado que las predicciones de las CNNs son probabilidades, se pueden promediar para intentar elevar la tasa de clasificación, sin embargo, esto no siempre ayuda a mejorar bastante esta tasa, ya que al relizar esta técnica si bien se promedian sus aciertos, también se promedian sus errores.

## Resultados

Como se puede ver, fueron muchas combinaciones las realizadas, por lo que de manera resumida se puede decir que **no hubo un experimento que sobresaliera tanto de los demás**, sin embargo, los resultados en los experimentos en los que se utilizó la capa *GAvgP2D* fueron casi siempre ligeramente menores con respecto al *accuracy* obtenido, manteniéndose más parejo en los restantes. A continuación se muestran dos tablas, una con el resumen experimental con la mejor CNN en cada experimento y otra con el uso de *soft voting*

**Experimento** | **CNN con accuracy obtenido** | **Jaamour et al. (2023)** | **[Lai](https://github.com/leoll2/MedicalCNN/tree/master)**|

Para terminar se menciona que en los otros dos bloques experimentales se experimentó de manera diferente y con código más organizado, involucrando el aprendizaje de máquina y más métodos de ensamble. 

**Nota:** Etos experimentos se realizaron en un entorno virtual de **Python (venv)** en *Windows* con **TensorFlow** versión **11** y una **GPU NVIDIA RTX 4080**.



