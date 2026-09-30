<div align="center">
<b> Resolución de la práctica 1 </b>
</div>

---

<div align="center">
  <b> Tres observaciones sobre CSV/JSON, Parquet y Delta</b>
</div>

##### Observación 1 | Apartado 1.1 
>Los archivos CSV son texto plano (no hay definición explícita del tipo de dato), por eso todos los atributos extraídos de *transactions_raw* son string, incluido *amount*. Por otro lado, los JSON son semi-estructurados, lo que permite definir el tipo de dato al momento de extraer; lo vemos en el caso de *events_raw*. 

>Los Parquet ya vienen con la definición de los tipos de dato: ya la lectura sin inferencia de *products_raw* establece los tipos *long*, *string* y *decimal* para *product_id*, *category* y *price* respectivamente.


##### Observación 2 | Apartado 1.2: inferencia de tipos 
>Incluso al hacer *inferschema = True*, *amount* sigue siendo identificado con un string. Esto se podría deber al hecho de que, como csv es un formato de texto plano, la inferencia se dificulta si la columna está ensuciada (con N/A por ejemplo). 


##### Observación 3 | Delta 
>El formato Delta tiene más información que el Parquet (por ejemplo, *products_raw*): mientras que éste sólo se limita a definir el tipo de dato en cada caso, aquél agrega la información de registro de historial de versiones, autoría y "origen de ejecución" o las métricas más avanzadas que brinda *DESCRIBE DETAIL*. 

---

<div align="center">
  <b>Dónde aparecen las 5 Vs</b>
</div>

##### Variedad
> Hay diversidad de fuentes y formatos: csv, Parquet, JSON. 
##### Velocidad
> Hay velocidad de carga de los datos: procedimiento ELT (que, distinto del ETL, no pierde tiempo transformando los datos antes de cargarlos).
##### Volumen
> El notebook no trabaja con grandes volúmenes de datos, pero suponemos que es escalable.
##### Valor
> El valor que se aporta viene de la centralización de los datos (en el data lake).
##### Veracidad 
> Delta permite seguir el historial de los datos para confirmar su veracidad.
