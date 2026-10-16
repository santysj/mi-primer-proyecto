# Entrega práctica 3 — Bases de datos NoSQL

- **Nombre: Santiago Sánchez Jeréz**
- **student_id:santiago**
- **Escala:small**

Respondé cada pregunta en una a tres oraciones. Cuando se pide un dato, copiá el valor que te dio el notebook.

## CAP y PACELC

**1.** En **modo CP**, luego de la partición «AB|C», ante la consulta por un _read_ del saldo, la réplica C respondió "ERROR: no disponible". Esto demuestra cómo CP sacrcifica disponibilidad. Por el otro lado, ante el mismo escenario pero en **modo AP**, la réplica C respondió "100", sacrificando consistencia (notar que los nodos A y B recibieron un _write saldo = 80_ pero que esto no llegó a C debido a la partición) en pos de la disponibilidad. 

**2.** En **modo CP**, se mantuvo el saldo escrito sobre la mayoría de los nodos, que es el que llegó a los nodos A y B, mientras que se perdió el _write saldo = 50_ que se había ejecutado sobre el nodo minoritario C (en realidad, nunca se registró por falta de quórum). Por el otro lado, en **modo AP** el saldo que quedó luego de la reparación es el que produjo el _write = 50_ sobre C, pues fue el último saldo escrito, que en este caso prepondera sobre el quórum como criterio (_last-write-wins_). 

**3.** Según el gráfico, la latencia de W=1 es, con p99, **9.6 ms**, mientras que la latencia de "=3 con p99 es **55.6 ms**

**4.** Sólo pudo seguir escribiendo el lado ABC, pues es la mayoría (el que logra quórum). 

## Clave-valor

**5.** El GET en memoria fue 5.370.000 veces más rápido que el GET con SPARK sobre Delta. Redis guarda los datos en memoria porque el problema que resuelve es encontrar un valor enseguida. 

**6.** Hubo que leer y parsear todos los valores porque la estructura clave-valor no "muestra a simple vista" los valores, sino sólo las claves, lo que quiere decir que para conocer los valores de un objeto, hay que leerlo en el momento. Como AR es un valor –no una clave–, hay que leer todos los objetos; parsearlos sirve para tener que hacerlo sólo una vez, en la que mandás las claves con valor AR a un set. 

**7.** Con **hash mod N** se movió un 79.6% de las claves, mientras que con **hashing consistente** se se movió un 21.2%. Conviene **hashing consistente** porque los nodos equilibrados garantizan un procesamiento eficiente y homogéneo: todos los nodos son igual de grandes, todos operables a un mismo nivel de complejidad. 

## Documental

**8.**

**9.**

**10.**

**11.**

## Grafos

**12.**

**13.**

**14.**

**15.**

## Vectorial

**16.**

**17.**

**18.**

## Columnar

**19.**

**20.**
