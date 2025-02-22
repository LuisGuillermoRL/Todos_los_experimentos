# Definición y realización de los Experimentos

Para todos los experimentos realizados se utilizó la [base de datos **CBIS-DDSM**](https://github.com/LuisGuillermoRL/EDA_CBIS-DDSM/blob/main/docs/sdata2017177.pdf) y mediante la exploración, etiquetado y obtención de imágenes de esta base de datos ([EDA](https://github.com/LuisGuillermoRL/EDA_CBIS-DDSM/tree/main)) fue posible realizar los experimentos que se describen más abajo. La base de datos cuenta con 2 "anormalidades" encontradas en las mastografías: **Masas** y **Calcificaciones**; a su vez divididas por sus patologías: **Benignas** y **Malignas**. Cabe aclarar que aunque la base de datos cuenta con las mastografías completas, las máscaras o ROIs de los cuáles se extraen los parches (*Cropped images*) y los parches, solamente se trabajó con estos últimos y con un tamaño (*resize*) de $224 \times 224$, ya que en estos se presenta la información más relevante, es decir, no se necesita toda la mastografía completa para empezar a atacar la problemática. En todos los experimentos se utilizaron las **CNNs VGG19, VGG16, ResNet50, MobileNetV2, InceptionV3 y DenseNet**. A continuación se definen los experimentos realizados.

* **Experimento Masas**. Como su nombre lo indica, este experimento es de *clasificación binaria*: una clase es **Masa Benigna** y la otra **Masa Maligna**. Este experimento ayuda a detectar cáncer al utilizar solo las imágenes correspondientes a las **Masas**. Una vez entrenada alguna CNN se le puede ingresar un parche (que contenga una masas) del tamaño con el que fue entrenada ($224 \times 224$) y así poder obtener la clase de esta, es decir, benigna o maligna.

* **Experimento Calcificaciones**. De igual forma al experimento previamente descrito. este es de *clasificación binaria*: una clase es **Calcificación Benigna** y la otra **Calcificación Maligna**. Este experimento también ayuda a detectar cáncer al utilizar solo las imágenes correspondientes a las **Calcificaciones**. Una vez entrenada alguna CNN se le debe presentar un parche (que contenga una calcificaciones) del tamaño con el que fue entrenada ($224 \times 224$).

* **Experimento Multiclase**. Como se puede observar, hasta ahorita existen 4 clases: **Masa Benigna**, **Masa Maligna**, **Calcificación Benigna** y **Calcificación Maligna**. En este experimento para la detección de cáncer, las 4 clases (todas las imágenes) fueron utilizadas para entrenar las distintas CNNs, solo que aquí a estos modelos se les puede presentar un parche que contenga una masa o calcificaciones para así obtener la clase correspondiente.

* **Experimento Malignas-Benignas (M-B)**. Con el fin de utilizar todas las imágenes para detectar cáncer *de manera distinta* al **Experimento Multiclase**, en este se definieron dos clases: **Malignas** y **Benignas**, sin importar si son masas o calcificaciones (su anormalidad). Es decir, se juntaron Masas y Calcificaciones Malignas para representar la clase  **Malignas**  y se juntaron Masas y Calcificaciones Benignas para representar la clase  **Benignas**.

* **Experimento Masas-Calcificaciones (M-C)**. Este experimento es de *clasificación binaria*: una clase es **Masa** y la otra **Calcificación**. En este experimento se usaron todas las imágenes, pero este no ayuda a detectar cáncer, ya que solo distingue entre Masas y Calcificaciones. Este fue realizado para evaluar el rendimiento de distintas CNNs así como para comparar los resultados obtenidos por **LAIIIIIIIIIII**, ya que este autor lo realizó.

De manera rápida se menciona lo siguiente:

* Se experimentó con cada una de las CNNS variando el número de Neuronas
* Debido a que no son muchas imágenes, se utilizó el **Image Data Generator de TensorFlow**, abreviado aquí por **IDG**. Este ayuda a evitar el *sobreajuste* y a elevar un poco la tasa de clasificación.
* Se exploraron a la par distintos parámetros para el **IDG**, llamados en la tesis *Parámetros Propuestos* y *Parámetros Iniciales* (los que usó **LAAAIIII**), además, también se sustituyó la capa *Flatten* de las CNNs por la capa *Global Average Pooling 2D*, la cual sirve como un reductor de dimensionalidad.
* La técnica de **CLAHE** también se les aplicó a las imágenes para tratar de ayudar a elevar la tasa de clasificación, para esto, se utilizó la librería de **OpenCV**, utilizando la función **tallllll** con los parámetros standar.
* Se utilizaó la técnica de *Ensamble: soft voting*, es decir, dado que las predicciones de las CNNs son probabilidades, se pueden promediar para intentar elevar la tasa de clasificación, sin embargo, esto no siempre ayuda a mejorar bastante esta tasa, ya que al relizar esta técnica si bien se promedian sus aciertos, también se promedian sus errores.

Como se puede ver, son muchas combinaciones, pero en este repositorio se muestran algunos ejemplos y resultados de los experimentos previamente mencionados y realizados para el tema de Tesis. En los otros dos bloques experimentales se experimento de manera diferente. Cabe mencionar que estos experimentos se realizaron en un entorno virtual de **Python (venv)** en *Windows* con **TensorFlow** versión **11** y una **GPU NVIDIA RTX 4080**.



