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

**7.**

**8.**

**9.**

**10.**

**11.**

**12.**

## Comparar y elegir

**13.**

**14.**

**15.**

**16.**

## Registro, uso y monitoreo

**17.**

**18.**

**19.**

## Cierre

**20.**
