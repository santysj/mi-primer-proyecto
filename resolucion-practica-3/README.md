# Entrega práctica 3 — Bases de datos NoSQL

- **Nombre: Santiago Sánchez Jeréz**
- **student_id: santiago**
- **Escala: small**

Respondé cada pregunta en una a tres oraciones. Cuando se pide un dato, copiá el valor que te dio el notebook.

## CAP y PACELC

**1.** En **modo CP**, luego de la partición «AB|C», ante la consulta por un _read_ del saldo, la réplica C respondió "ERROR: no disponible". Esto demuestra cómo CP sacrcifica disponibilidad. Por el otro lado, ante el mismo escenario pero en **modo AP**, la réplica C respondió "100", sacrificando consistencia (notar que los nodos A y B recibieron un _write saldo = 80_ pero que esto no llegó a C debido a la partición) en pos de la disponibilidad. 

**2.** En **modo CP**, se mantuvo el saldo escrito sobre la mayoría de los nodos, que es el que llegó a los nodos A y B, mientras que se perdió el _write saldo = 50_ que se había ejecutado sobre el nodo minoritario C (en realidad, nunca se registró por falta de quórum). Por el otro lado, en **modo AP** el saldo que quedó luego de la reparación es el que produjo el _write = 50_ sobre C, pues fue el último saldo escrito, que en este caso prepondera sobre el quórum como criterio (_last-write-wins_). 

**3.** Según el gráfico, la latencia de W=1 es, con p99, **9.6 ms**, mientras que la latencia de "=3 con p99 es **55.6 ms**

**4.** Sólo pudo seguir escribiendo el lado ABC, pues es la mayoría (el que logra quórum). 

## Clave-valor

**5.** El GET en memoria fue 5.370.000 veces más rápido que el GET con SPARK sobre Delta. Redis guarda los datos en memoria porque el problema que resuelve es encontrar un valor enseguida. 

**6.** Hubo que leer y parsear todos los valores porque la estructura clave-valor no "muestra a simple vista" los valores, sino sólo las claves, lo que quiere decir que para conocer los valores de un objeto, hay que leerlo en el momento. Como AR es un valor –no una clave–, hay que leer todos los objetos; parsearlos sirve para tener que hacerlo sólo una vez, en la que mandás las claves con valor AR a un set. 

**7.** Con **hash mod N** se movió un 79.6% de las claves, mientras que con **hashing consistente** se se movió un 21.2%. Conviene **hashing consistente** porque los nodos equilibrados garantizan un procesamiento eficiente y homogéneo: todos los nodos son igual de grandes, todos operables a un mismo nivel de complejidad. Como contraste, con **hash mod N** quedan nodos desbalanceados, algunos sobrecargados, otros con muy poca información; la velocidad y complejidad de operación se vuelve muy variable, dependiendo del nodo con el que te toque trabajar. 

## Documental

**8.** La consulta relacional devolvió 11 filas, mientras que la documental devolvió un solo documento, que embebió todos los datos, sin repetirlos, para el cliente 42. 

**9.** Spark rellena con _null_ los campos para los cuales no está el campo que sí está en otros. Por permitir que distintos documentos tengan distintos campos es por lo que se dice que el modelo documental tiene esquema flexible. 

**10.** El documento más grande pesa 5.2KB. Si un documento creciera sin límites, el problema que aparecería es que no se puede manejar el documento.

**11.** El modelo documental resuelve bien las consultas unitarias, por entidad; pero le va mal con las consultas transversales. 

## Grafos

**12.** El grafo tiene 2361 nodos Device, 5000 nodos Customer y 5.400 aristas. 

**13.** Para 2 y 4 saltos, SQL necesitó 3 y 5 joins respectivamente. Neo4J es más eficiente que una base relacional en cuanto a recorrer relaciones porque evita el problema de los JOINs: se puede ir de cualquier nodo a cualquiera otro con el que esté conectado con una sóla flecha que, a nivel computacional, se basa en _pointer hopping_.

**14.** Tardó 14 iteraciones. El componente más grande tiene 23 nodos. 

**15.** Al repartir con **hash(id) mod 4**, un 74.4% de aristas quedó entre máquinas distintas. Es dificil distirbuir una base de grafos porque siempre que una arista quede entre máquinas distintas, te ves obligado a hacer un salto de red. 

## Vectorial

**16.** El vector de cada cliente tiene n dimensiones y representa los n atributos que le corresponden (lo que sería los campos). La similitud coseno mide qué tanto se parece (el vector de) un cliente a(l del) otro: si la similitud vale 1, los vectores son iguales. 

**17.** Hay un 31.6% de vecinos de marcados que está marcado. Sirve buscar vecinos parecidos porque permite analizar/predecir comportamientos: «si A y B son vecinos y sé que A es X, probablemente B también sea X».

**18.** Con **nprobe=4** se escanea un 5.985% de los vectores y un recall de 0.872. Se gana en costo computacional, pero se paga el costo de pasar por alto vecinos verdaderos. 

## Columnar

**19.**

**20.**
