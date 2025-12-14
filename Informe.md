# famaf-text-mining-stance-tweets
Informe final del proyecto de la materia Text Mining de FaMAF:
_Detección de posicionamientos respecto de temas de discusión en Twitter._

## Propuesta, datos, hipótesis y objetivos
ver [README](https://github.com/pablocelayes/famaf-text-mining-stance-tweets/blob/main/README.md)

## Resultados por etapa

### 1. Detección de temas y análisis exploratorio
[01 - EDA](https://github.com/pablocelayes/famaf-text-mining-stance-tweets/blob/main//01%20-%20EDA.ipynb)

[02 - Exportar tuits a dataframe.ipynb](https://github.com/pablocelayes/famaf-text-mining-stance-tweets/blob/main/02%20-%20Exportar%20tuits%20a%20dataframe.ipynb)

Dado el gran volumen de tuits en el dataset, en este trabajo nos enfocamos en la semana de más actividad: 2020-04-12 a 2020-04-18.  El 3.4% de los tuits son respuestas a otros tuits, pero solo un 0.12% son respuestas a otros tweets de la misma semana en el dataset elegido (7400 tuits).

Aplicamos BERTopic para detectar temas en los tweets.
Como este proceso es computacionalmente costoso, se debió limitar aún más el conjunto de tuits, tomando el día más activo dentro de la semana ('2020-04-16', con 971,679 tuits).

[03.1 - embeddings.ipynb](https://github.com/pablocelayes/famaf-text-mining-stance-tweets/blob/main/03.1%20-%20embeddings.ipynb)

Aplicamos el modelo _sentence transformer_ 'paraphrase-multilingual-MiniLM-L12-v2' para obtener las representaciones de los tuits.

[03.2 - topic model train on sample.ipynb](https://github.com/pablocelayes/famaf-text-mining-stance-tweets/blob/main/03.2%20-%20topic%20model%20train%20on%20sample.ipynb)

Entrenamos el _modelo temático_ BERTopic sobre un muestra de un 10% de los tuits.

[03.3 - topic model extend to full.ipynb](https://github.com/pablocelayes/famaf-text-mining-stance-tweets/blob/main/03.3%20-%20topic%20model%20extend%20to%20full.ipynb
)
Aplicamos el modelo entrenado al conjunto completo de tuits del '2020-04-16'.

### 2. Temas polarizados

[03.4 - topic model analyze topics.ipynb](https://github.com/pablocelayes/famaf-text-mining-stance-tweets/blob/main/03.4%20-%20topic%20model%20analyze%20topics.ipynb)
Exploramos los temas descubiertos:
- Título del tema (generado por BERTopic)
- Tuits con mayor probabilidad respecto del tema.

En este trabajo nos interesa analizar temas activos (con muchos tuits) y polémicos (con discusión y distintos posicionamientos). Como heurística sencilla para encontrar temas polémicos decidimos calcular el _reply rate_: la cantidad promedio de respuestas que recibe un tuit dentro de un tema.

Como primera selección de tomamos la intersección entre los 1000 más activos y los 1000 más polémicos, resultando en 378 temas, luego de descartar el tema de índice '-1', que es el tema _de fondo_ (en este caso la pandemia de COVID 19). En este [reporte](https://github.com/pablocelayes/famaf-text-mining-stance-tweets/blob/main/covid_twitter_sample_2020-04-16_topics_report.txt) pueden verse más detalles de estos temas:
- Título
- Tuits con mayor probabilidad
- Tuits con respuestas





#### Hashtags
Aquí también hicimos un análisis de los hashtags más frecuentes en los tuits de la semana seleccionada, para ver si podemos utilizarlos como temas, reemplazando o complementando los temas detectados con BERTopic. La mayoría de los hashtags contiene en sí mismos una consigna con un posicionamiento definido (e.g.: #quedatencasa, #estevirusloparamosunidos, #conhambreymalgobierno) o son simplemente genéricos o informativos (e.g.: #covid19 #coronavirus #cuarentena #reporte). Como nos interesa analizar posicionamiento y contraposición de posturas, decidimos no usarlos.







### 3. Posicionamiento sobre temas

### 4. Caracterización de interacciones entre tuits
