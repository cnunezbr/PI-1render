

<p align="center">
<img src="https://github.com/cnunezbr/images/blob/main/mlop.png"  height=300>
</p>
<h1 align=center>Analisis de Datos y Modelo de Machine Learning para la Plataforma de Juegos - Steam

## Introducción:
Este proyecto simula el papel de un MLOps Engineer, que combina las habilidades de un Data Engineer y un Data Scientist, para la plataforma internacional de videojuegos Steam. Con el propósito de llevar a cabo este proyecto, se proporcionaron datos y se solicitó la creación de un Producto Mínimo Viable (MVP) que incluye el despliegue de una API en un servicio en la nube y la implementación de dos modelos de Machine Learning. Estos modelos se enfocan en realizar un análisis de sentimientos de los comentarios de los usuarios de los juegos, así como en recomendar juegos basados en el nombre de un juego o las preferencias de un usuario en particular.

Datos:
Para llevar a cabo este proyecto, se utilizaron tres archivos JSON:

* Australian_user_reviews.json: Contiene comentarios de usuarios sobre los juegos, junto con información adicional, como recomendaciones, emoticones de diversión, y estadísticas de utilidad de los comentarios. Además, proporciona el ID del usuario que hizo el comentario, su URL de perfil y el ID del juego relacionado.

* Australian_users_items.json: Contiene información sobre los juegos jugados por los usuarios, incluyendo el tiempo total jugado por cada usuario en juegos específicos.

* Output_steam_games.json: Contiene datos relacionados con los juegos en sí, como títulos, desarrolladores, precios, especificaciones técnicas, etiquetas y otros detalles.


## Transformaciones:
Se realizó un proceso de Extracción, Transformación y Carga (ETL) en los tres conjuntos de datos proporcionados. Dado que dos de los conjuntos de datos tenían estructuras anidadas, lo que significa que contenían columnas con diccionarios o listas de diccionarios, se aplicaron diversas estrategias para transformar las claves de estos diccionarios en columnas. Se completaron valores faltantes en variables esenciales para el proyecto y se eliminaron columnas con un gran número de valores faltantes o que no aportaban al proyecto, con el fin de optimizar el rendimiento de la API y cumplir con las restricciones de almacenamiento en el despliegue. Estas transformaciones se llevaron a cabo utilizando la biblioteca Pandas.

## Analisis de Sentimientos:
En este proyecto, se cumplió con la solicitud de aplicar un análisis de sentimientos a las reseñas de los usuarios. Se creó una nueva columna llamada 'sentiment_analysis' que reemplaza la columna que contenía las reseñas y clasifica los sentimientos de los comentarios en una escala de 0 a 2, donde 0 representa un sentimiento negativo, 1 un sentimiento neutral o ausencia de reseña, y 2 un sentimiento positivo. Para llevar a cabo este análisis de sentimiento, se utilizó TextBlob, una biblioteca de procesamiento de lenguaje natural (NLP) en Python. Este análisis se realiza de manera básica, asignando un valor numérico a los textos (en este caso, a las reseñas de los usuarios) para representar si el sentimiento expresado en el texto es negativo, neutral o positivo.

Además, con el objetivo de optimizar el rendimiento de la API y teniendo en cuenta las restricciones de almacenamiento en el servicio de nube para el despliegue, se crearon dataframes para cada una de las funciones requeridas que se guardaron en formato Parquet, lo cual permite una compresión y codificación eficiente de los datos.

## Análisis Exploratorio de Datos (EDA):
Se realizó un Análisis Exploratorio de Datos a los tres conjuntos de datos después de la fase de **ETL**, con el objetivo de identificar las variables que serían útiles en la creación del modelo de recomendación. Para llevar a cabo este análisis, se utilizó la biblioteca Pandas para la manipulación de datos y las bibliotecas Matplotlib y Seaborn para la visualización de datos.

Específicamente para el modelo de recomendación, se optó por construir un dataframe específico que incluye el ID de los usuarios que realizaron reseñas, los nombres de los juegos en los que dejaron comentarios y una columna de calificación construida a partir del análisis de sentimiento y las recomendaciones de juegos.

## Modelo de Aprendizaje Automático:
Se crearon dos modelos de recomendación, cada uno generando una lista de 5 juegos. En el primer modelo, se utilizó un enfoque ítem-ítem, donde se recomiendan juegos similares a uno dado en función de su similitud. El segundo modelo aplicó un enfoque usuario-juego, encontrando usuarios similares y recomendando juegos que les gustaron a esos usuarios similares.

Estos modelos se basan en algoritmos de filtrado colaborativo que utilizan toda la base de datos para encontrar usuarios similares al usuario activo y utilizar sus preferencias para predecir las calificaciones del usuario activo. La similitud entre juegos (item_similarity) y entre usuarios (user_similarity) se midió utilizando la similitud del coseno, una medida comúnmente utilizada en sistemas de recomendación y análisis de datos para evaluar la similitud entre dos conjuntos de datos o elementos.

## Desarrollo de la API:
La API se desarrolló utilizando el framework **FastAPI** e incluye varias funciones:

* userdata: Devuelve información sobre un usuario, incluyendo el dinero gastado, el porcentaje de recomendaciones que hizo y la cantidad de juegos que consumió.

* countreviews: Proporciona estadísticas sobre la cantidad de usuarios que realizaron reseñas y el porcentaje de recomendaciones positivas en un rango de fechas específico.

* genre: Muestra la clasificación de un género de videojuego en función de las horas jugadas por los usuarios.

* userforgenre: Devuelve una lista de los 5 usuarios que jugaron más horas en un género de videojuego específico.

* developer: Proporciona información sobre la cantidad de juegos desarrollados por una empresa y el porcentaje de contenido gratuito por año.

* sentiment_analysis: Ofrece estadísticas sobre el análisis de sentimiento de las reseñas de usuarios según el año de lanzamiento de un juego.

* recomendacion_juego: Recomienda 5 juegos similares a uno dado.

* recomendacion_usuario: Recomienda 5 juegos para un usuario específico basado en similitudes con otros usuarios.

El código para generar la API se encuentra en el archivo **main.py**, y las funciones necesarias se encuentran en **apis.py**.

