# famaf-text-mining-stance-tweets
Proyecto de la materia Text Mining de FaMAF: Detección de posicionamientos respecto de temas de discusión en Twitter

## Resumen
A partir de datos sobre la discusión pública alrededor de la pandema de COVID-19 en la red social Twitter (actualmente X), buscamos desarrollar análisis que nos permitan comprender los temas centrales de discusiones, las posturas sostenidas respecto de ellos y las dinámicas de interacción entre diferentes posicionamientos.

## Hipótesis de trabajo
1. Es posible identificar temáticas centrales y dentro de estas, aquellas que presentan más polarización de opiniones.
2. Es posible identificar de manera semiautomática posicionamiento de opiniones alrededor de estos temas.
3. Es posible analizar de manera cuantitativa qué interacciones son de apoyo, de ataque o neutras.

## Conjunto de datos

Se trabajará sobre un dataset de tuits sobre COVID, recolectado durante los primeros meses de la pandemia COVID-19, usando la API de Twitter.

- periodo: Abril a Junio 2020
- idioma: español
- términos de búsqueda: covid, covid19
- cantidad de tuits: 24,6M
- cantidad de usuarios: 3,6M
- contiene información de retuits y respuestas

## Objetivos preliminares

Nos proponemos investigar y aplicar técnicas para detectar temáticas y dentro de estas identificar aquellas con más polarización (tal como se define en [1]). El siguiente paso será identificar posicionamiento de tuits respecto de estos temas. De ser posible, también se investigarán técnicas para caracterizar las interacciones entre tuits de respuesta.

## Etapas del trabajo

1. Detección de temas. Para ello se aplicarán técnicas como LDA[4] o Twitter-LDA[5].

2. Identificación de temas con polarización. Se estudiará el trabajo [2] y se adaptarán las técnicas allí empleadas.

3. Desarrollo, implementación y evaluación de técnicas para identificar la posición de tuits dentro de los temas centrales y controversiales identificados en 2.

4. Desarrollo, implementación y evaluación de técnicas para caracterizar interacciones entre tuits (apoyo, ataque, neutro). Se estudiará el trabajo [3] y se adaptarán las técnicas allí empleadas.

## Referencias

[1] Saif Mohammad, Svetlana Kiritchenko, Parinaz Sobhani, Xiaodan Zhu, and Colin Cherry. (2016).
[SemEval-2016 Task 6: Detecting Stance in Tweets.](https://aclanthology.org/S16-1003/)
In Proceedings of the 10th International Workshop on Semantic Evaluation (SemEval-2016), pages 31–41, San Diego, California. Association for Computational Linguistics.

[2] Furman, D. A., Marro, S., Cardellino, C., Popa, D. N., & Alonso Alemany, L. (2021). 
[You can simply rely on communities for a robust characterization of stances.](https://journals.flvc.org/FLAIRS/article/view/128515/130054)
The International FLAIRS Conference Proceedings, 34.

[3] Schmidt, Mariano (2021).
[Explotando características contextuales para la detección de posturas en Twitter en el marco de la vacunación del COVID-19 en Argentina.](https://docs.google.com/document/d/10kzaOA857nJynijoRkFd9J-OuBVBsMphwoVQr7EiRs8/edit?usp=sharing)
Trabajo Especial de Licenciatura en Ciencias de la Computación, FaMAF, UNC.

[4] David M. Blei, Andrew Y. Ng, Michael I. Jordan; 3(Jan):993-1022, 2003.
[Latent Dirichlet Allocation](http://www.jmlr.org/papers/volume3/blei03a/blei03a.pdf)

[5] Wayne Xin Zhao, Jing Jiang et al.,
[Comparing Twitter and traditional media using topic models.](http://link.springer.com/chapter/10.1007%2F978-3-642-20161-5_34)
 ECIR'11.
