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

En este trabajo nos interesa analizar temas activos (con muchos tuits) y polémicos (con discusión y distintos posicionamientos). Como heurística sencilla para encontrar temas polémicos decidimos calcular el _reply rate_: la cantidad promedio de respuestas que recibe un tuit dentro de un tema (independientemente de si las respuestas son o no dentro del mismo tema).

Como primera selección de temas tomamos la intersección entre los 1000 más activos y los 1000 más polémicos, resultando en 378 temas, luego de descartar el tema de índice '-1', que es el tema _de fondo_ (en este caso la pandemia de COVID 19). En este [reporte](https://github.com/pablocelayes/famaf-text-mining-stance-tweets/blob/main/covid_twitter_sample_2020-04-16_topics_report.txt) pueden verse más detalles de estos temas:
- Título
- Tuits con mayor probabilidad
- Tuits con respuestas


#### Hashtags
Aquí también hicimos un análisis de los hashtags más frecuentes en los tuits de la semana seleccionada, para ver si podemos utilizarlos como temas, reemplazando o complementando los temas detectados con BERTopic. La mayoría de los hashtags contiene en sí mismos una consigna con un posicionamiento definido (e.g.: #quedatencasa, #estevirusloparamosunidos, #conhambreymalgobierno) o son simplemente genéricos o informativos (e.g.: #covid19 #coronavirus #cuarentena #reporte). Como nos interesa analizar posicionamiento y contraposición de posturas, decidimos no usarlos.


### 3. Caracterización de interacciones entre tuits

Las interacciones de respuesta entre tuits en nuestro conjunto de datos son muy escasas (0.12% de los tuits son respuestas a otros tuits del mismo conjunto). Para hacer un mejor aprovechamiento de estas, en esta fase proponemos modificar la heurística de selección de temas polémicos, para seleccionar _tuits polémicos_ dentro de _temas activos_.

Se tomaron, para cada tema dentro de los 5000 más activos, todos los tuits en estos temas que recibieron al menos 3 respuestas. Esto resultó en un conjunto de 158 tuits con 970 respuestas en total. (pueden verse en esta [planilla](https://docs.google.com/spreadsheets/d/1jaoshgt0sF2avTzLH2dRczrT1VLIjLozUcizDkB7V0s/edit?gid=0#gid=0)).

#### 3.1 Etiquetado de interacciones

[04 - etiquetando interacciones con LLMs.ipynb](https://github.com/pablocelayes/famaf-text-mining-stance-tweets/blob/main/04%20-%20etiquetando%20interacciones%20con%20LLMs.ipynb)
El etiquetado se realizó en tres etapas:

- **Etiquetado preliminar con ChatGPT**, utilizando el siguiente _prompt_ (en inglés):
https://chatgpt.com/share/693ed840-e380-8013-b8ce-b9afd55645ac

- **Revisión y corrección manual** de una [muestra](https://docs.google.com/spreadsheets/d/1jaoshgt0sF2avTzLH2dRczrT1VLIjLozUcizDkB7V0s/edit?gid=1179560399#gid=1179560399) de las [etiquetas](https://docs.google.com/spreadsheets/d/1jaoshgt0sF2avTzLH2dRczrT1VLIjLozUcizDkB7V0s/edit?gid=51505042#gid=51505042) generadas por ChatGPT-4.  La cantidad de etiquetas correctas sobre las revisadas fue de sólo el 46%.

- **Etiquetado final con GPT-5.2.** En base a las correcciones realizadas en la etapa anterior, se generó un nuevo _prompt_ (en español) para etiquetar nuevamente todas las interacciones utilizando GPT-5.2 (con razonamiento) a través de la API de OpenAI.  Se revisó una [muestra](https://docs.google.com/spreadsheets/d/1jaoshgt0sF2avTzLH2dRczrT1VLIjLozUcizDkB7V0s/edit?gid=876422118#gid=876422118) de 50 interacciones, obteniéndose una precisión del 98%. El único tuit de cuya etiqueta se dudó fue uno que contenía sarcasmo, y podría también ser correcto.

#### 3.2 Entrenamiento y evaluación de modelo de clasificación

Dada la alta calidad de las etiquetas generadas por GPT-5.2, se decidió entrenar un modelo de clasificación automático para predecir las etiquetas de nuevas interacciones. Un modelo de este tipo permitiría escalar el análisis a un conjunto mayor de interacciones, sin depender de la generación de etiquetas por LLMs, que es lenta y costosa (el etiquetado de 970 interacciones llevó más de una hora y costó alrededor de 6 dólares).

[05 - entrenamiento modelo clasificacion interacciones.ipynb](



