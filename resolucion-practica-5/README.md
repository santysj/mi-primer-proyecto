# Entrega práctica 5 — MLOps con MLflow

- **Nombre: Santiago Sánchez Jeréz**
- **student_id: santiago**
- **Escala: small**

Respondé cada pregunta en una a tres oraciones. Cuando se pide un dato, copiá el valor que te dieron tus notebooks.

## Mis runs

Pegá acá la tabla que imprime `03_comparar_y_elegir` (sección 5) e indicá cuál quedó registrado como `champion`.

**Run registrado:**

## Datos y tracking

**1.** El split «train» tiene 35.654 filas, el test 7.129. Ambas tienen un 10.4% de contracargos. Se separa por fecha porque estamos evaluando capacidad predictiva: anticipar un evento futuro en función de lo que ya pasó. 

**2.** Un modelo que siempre dice que no hay contragargo tiene Accuracy de 89.6%. La práctica utiliza PR-AUC para lidiar con las clases desbalanceadas: contracargo es ~10%, mientras que no-contracargo es ~90%. El PR-AUC contempla la capacidad predictiva para cada caso, independientemente de la predicción global, que se ve sesgada por las clases desbalanceadas. 

**3.** 
Parámetros: \
<img width="331" height="208" alt="image" src="https://github.com/user-attachments/assets/48281bbe-55c3-4ceb-ae3d-a911cfc220d4" /> 


Métricas: \
<img width="604" height="208" alt="image" src="https://github.com/user-attachments/assets/43623e88-a349-4a10-868e-43cffd1932b6" /> 

Artefactos: \
<img width="245" height="416" alt="image" src="https://github.com/user-attachments/assets/4ca0aa4a-107b-4ae3-889e-4f920be57b98" />

La diferencia entre run y experimento es que mientras un run es lo que guarda parámetros, métricas, artefactos y tags, un experimento es un conjunto de runs. 


**4.** Autolog regustra automáticamente parámetros, métricas de entrenamiento y el modelo; pero no registra, no conoce, el problema que se está tratando, no conoce nuestro conjunto de test ni que nos importa el PR-AUC. 

**5.** Hay que fijar los datos, el mismo código, la misma semilla y el mismo entorno.

**6.** Al llamar al modelo sin la columna _amount_, este se frenó solo. La **signature** sirve para que el modelo sepa qué columnas debe esperar y qué devuelve. 

## Tus experimentos

**7.** Mi mejor modelo fue un **gradient boosting** con C=1, n_estimators=100, max_depth=3, learning_rate=0.1, class_weight=balanced, umbral=0.5 y **features completas**. Tuvo PR-AUC=0.586. Mientras que "solo_regla" tiene una PR-AUC=0.17. 


**8.** Las PR-AUC con básicas y cliente (ceteris paribus respecto del mejor modelo) fueron de 0.328 y 0.514 respectivamente. Las variables más importantes fueron el modelo (GB), las features (completas), class weight(balanced), el umbral (0.5), learning rate (0.1) y max depth (3).

**9.** Anduvo mejor boosting. Un modelo de árboles puede aprovechar combinaciones que la logística no ve porque la logística no combina, pondera efectos individuales. 

**10.** En general, bajó un poco la precisión pero subió mucho el recall. Y sí, cambió mucho el PR-AUC. 

**11.** Al bajar el umbral, PR-AUC no cambió, mientras que f1 cayó. Esto se debe a que PR-AUC evalúa cómo ordena el modelo los casos (la probabilidad que da a cada uno), mientras que f1 evalúa los resultados para cada predicción (precisión, recall).

**12.** Barrí max depth y con 3 me dio el mejor PR-AUC. Hubo valores más altos pero no tanto. Este fenómeno se llama overfitting. 

## Comparar y elegir

**13.** No coincidieron mi mejor PR-AUC con mi mejor accuracy. Ambos son boosting con features completas, pero uno (mejor PR-AUC) tiene class weitght balanced y umbral=0.2 mientrs que el otro classweight=ninguno y umbral=0.5

**14.** El más sistemático en alejarse de la diagonal es random forest. Que se aleje significa que sobreajusta. 

**15.** _search_runs_ permite hacer consultas con sintaxis SQL. 
Usaría 'metrics.pr_auc > 0.55' 

**16.** Cuesta más una alerta falsa. Siendo que hay una cantidad fija de alertas a revisar por día, subiría el umbral cuanto más limitado esté.

## Registro, uso y monitoreo

**17.** Se pudo. \
Nombre: 'workspace.bigdata_santiago.fraud_model'\
Versión: 1 \
Alias: 'champion'\
La ventaja de cargar los modelos de esta forma es que, si tenés un modelo que pide un champion y el champion cambia, al tagear champion, no hay que tocar nada del código si el tag cambia. 

**18.** Se puntuaron 7.165 transacciones y 3.410 generaron alerta. Las columnas que permiten auditar cada decisión son: modelo, versión, run y umbral (como permite audita, por eso el umbral se guarda fuera del modelo). 

**19.** El PR-AUC fue parecido en las nuevas: 0.586 (train) vs 0.579 (producción). El porcentaje de alertas también se mantuvo parecido: 48.73% contra 47.59%. \
El drift es un cambio en los datos, en su composición. Se puede detectar antes de tener las etiquetas porque se nota primero en las distribuciones de entrada y de scores, que ocurre antes de las métricas y el etiquetado. 

## Cierre

**20.**
