# Resolución de la práctica 1

## Tres observaciones sobre CSV/JSON, Parquet y Delta
### Observación 1 | Apartado 1.1 
Los archivos CSV son texto plano sin definición de tipo de dato, por eso todos los atributos extraídos de *transactions_raw* son string, incluido *amount*. Por otro lado, los JSON son semi-estructurados, lo que permite definir el tipo de dato al momento de extraer; lo vemos en el caso de *events_raw*. 

Los Parquet ya vienen con la información sobre los tipos de dato: ya la lectura sin inferencia de *products_raw* establece los tipos *long*, *string* y *decimal* para *product_id*, *category* y *price* respectivamente. 


---


### Observación 2 | Apartado 1.2: inferencia de tipos 
Incluso al hacer *inferschema = True*, *amount* sigue siendo identificado con un string. Esto se puede deber al hecho de que, como csv es un formato de texto plano –no binario, con metadatos que definan tipos de dato–, es dificil inferir el tipo de dato de un número con punto/coma (con los int sin punto parece que no hubo problema): podría ser float, long o incluso int, como es el caso. 


---


### Observación 3 | Delta 




## Dónde aparecen las 5 Vs 
