# SILVER, GOLD Y ORQUESTACIÓN
**Nombre: Santiago Sánchez Jeréz**\
**id: santiago**\
**Escala: small**



## Análisis de las tablas

1. ¿Cuántas filas físicas recibió cada lote en `bronze_transactions_incremental`? Escribí una consulta que muestre el resultado por `source_batch_id`.
> Cada lote recibió 204 filas.\
>\
><img width="345" height="77" alt="image" src="https://github.com/user-attachments/assets/d96aabef-26f9-4dc4-8b8d-525d5c78fa19" />


2. Para cada lote, ¿cuántas transacciones fueron aceptadas y cuántas quedaron en `silver_transactions_quarantine`? Reconciliá tus resultados con `gold_batch_summary`.
>200 transacciones fueron aceptadas en el batch_002 y 201 en el 003. Y en cada lote se rechazaron 2, 4 en total. Para el lote _initial_ se rechazaron 51 transacciones y se aceptaron 49999.\
>\
><img width="506" height="108" alt="image" src="https://github.com/user-attachments/assets/4ec67b13-616b-447d-a512-591ff7b3f914" />


3. ¿Qué motivos de rechazo aparecen en la cuarentena y cuántos registros tiene cada uno por lote? ¿Los rechazos observados coinciden con los casos introducidos por el generador?
>Aparecen dos tipos de motivo de rechazo: _cantidad inválida_ y _customer desconocido_. Tanto _batch_002_ como _batch_003_ tienen uno de cada motivo. Además, el lote _initial_ tiene como motivo de rechazo para las 51 transacciones, _cantidad inválida_.\
>\
><img width="414" height="158" alt="image" src="https://github.com/user-attachments/assets/5d7d3132-7d19-45ee-907d-42f9df89e929" />
>\
>Por otro lado, los rechazos coinciden con los casos introducidos por el generador.

4. Seguí la transacción `42` desde `bronze_transactions_all` hasta `silver_transactions`. ¿Cuántas versiones existen en Bronze y cuál quedó vigente en Silver? Mostrá las columnas que justifican la elección.
>En Bronze existen 3 y en silver 1.\
>\
><img width="393" height="58" alt="image" src="https://github.com/user-attachments/assets/d75ca8ad-d204-4f5b-9798-1669e6526608" /> <-- Bronze
>\
><img width="393" height="58" alt="image" src="https://github.com/user-attachments/assets/f09caf93-b0ff-4395-bd95-45367c732b5d" /> <-- Silver
>\
>\
>La columna que explica cuál transacción pasa a Silver es _updated_at_; Silver se queda con la última actualización. \
>\
><img width="392" height="109" alt="image" src="https://github.com/user-attachments/assets/ad540f18-296a-42e2-b12b-18fcb7249ed1" /> <-- Bronze
>\
><img width="448" height="59" alt="image" src="https://github.com/user-attachments/assets/e9d4e83e-8a8a-4f6a-9a61-0b9b988ff301" /> <-- Silver



5. Comprobá mediante una consulta que `silver_transactions` tiene una sola fila por `transaction_id`. ¿Qué resultado indicaría que la deduplicación falló?
> La consulta fue\
><img width="406" height="83" alt="image" src="https://github.com/user-attachments/assets/23a734c2-4b3c-4673-af09-4abfcb90bada" />
>\
> y no devolvió ninguna fila. Si hubiese habido alguna fila como resultado, eso indicaría que la deduplicación falló. 


6. Calculá la tasa de rechazo de cada lote como `rechazadas / (aceptadas + rechazadas)` en `gold_batch_summary`. ¿Es correcto comparar solamente las cantidades absolutas si los lotes tienen tamaños diferentes?
><img width="311" height="106" alt="image" src="https://github.com/user-attachments/assets/91d883af-0cda-4e16-9ee6-db03c15e9289" /> \
>\
>No es correcto comparar cantidades absolutas cuando los tamaños son distintos, pues eso no explica la dimensión proporcional, que es la que sirve para pensar el funcionamiento de algo a distintas escalas y comparar (in)eficacias. 

7. ¿Qué día presenta el mayor monto total y cuál presenta la mayor cantidad de transacciones? Consultá `gold_daily_sales` y explicá si ambos máximos coinciden.
><img width="658" height="57" alt="image" src="https://github.com/user-attachments/assets/dba1c0f4-531a-4b24-91a5-795994937647" /> \
><img width="658" height="57" alt="image" src="https://github.com/user-attachments/assets/6d8475d3-7993-4f9b-af77-417719285a36" /> \
>\
>No es cierto que ambos máximos coinciden. Las tablas lo explican. 



8. ¿Qué canal de pago tiene la mayor tasa global de fraude? Calculala como `SUM(fraud_transactions) / SUM(transaction_count)` y explicá por qué no corresponde promediar directamente `fraud_rate`.
>El canal de pago con mayor tasa global de fraude es transferencia. \
>\
><img width="369" height="57" alt="image" src="https://github.com/user-attachments/assets/1dc1927a-20b4-4fb1-98f9-14d3aa0c2d7b" /> \
>\
>No corresponde promediar directamente _fraude_rate_ porque esa tasa es diaria. Ponderar sobre la tasa diaria soslaya el hecho de que en días distintos hay diferentes volúmenes de pagos, de fraudes, y hace valer la tasa de cada día como de mismo peso que la tasa de cualquier día. Esto es inexacto para pensar la proporción real del fraude por canal. 



9. ¿Qué combinación de país y categoría concentra el mayor monto vendido? Mostrá también la combinación líder dentro de cada país.
> La combinación «BR–home» es la que concentra el mayor monto vendido. \
> \
> <img width="369" height="57" alt="image" src="https://github.com/user-attachments/assets/a540ee37-c576-4e70-978d-22f245885f6f" /> \
> \
> La tabla con las combinaciones lider para cada país es la siguiente. \
> \
> <img width="369" height="159" alt="image" src="https://github.com/user-attachments/assets/243f20ed-dfef-4ebd-a173-ee053b93c656" />




10. Compará las dos primeras filas de `pipeline_run_audit` correspondientes a la reejecución de `batch_002`. ¿Qué métricas permanecen iguales y qué columna demuestra que se realizó la comparación de idempotencia?
><img width="945" height="52" alt="image" src="https://github.com/user-attachments/assets/45bdf841-291a-4ab9-bdfe-9d24003b8051" /> \
> \
> Permanecen iguales todas menos la de _recorded_at_ y la de _idempotence_compared_. Esta última es la que demuestra que se realizó la comparación de idempotencia. 


___


### Interpretación del código y del pipeline

11. En `01_ingest_bronze_incremental.ipynb`, ¿qué problema resuelve `COPY INTO` y qué información utiliza para evitar cargar dos veces el mismo archivo físico?


12. ¿Por qué la vista `bronze_transactions_all` usa `UNION ALL` en lugar de eliminar duplicados? ¿En qué capa se resuelven los duplicados de negocio y por qué?


13. ¿Por qué las transacciones iniciales reciben `source_batch_id='initial'` y usan `event_ts` como `updated_at`? ¿Cómo afecta eso a la corrección de la transacción `42`?


14. En `quality_rules.py`, ¿qué ventaja ofrece `try_cast` frente a un `cast` convencional cuando llega un importe como `N/A`?


15. Las reglas de calidad asignan una única `quality_reason`. ¿Qué sucede si un registro viola más de una regla y por qué importa el orden de las condiciones?


16. Explicá cómo se construye `_record_key` y cómo se usa junto con `row_number`. ¿Qué caso cubre el hash cuando `transaction_id` no puede convertirse a un número?


17. Interpretá las dos cláusulas principales del `MERGE` de `silver_transactions`. ¿Cuándo se actualiza una fila existente y cuándo se inserta una nueva?


18. ¿Por qué las tablas Gold se reconstruyen completamente en esta práctica mientras Silver se actualiza con `MERGE`? Mencioná una ventaja y una limitación de cada estrategia.


19. ¿Por qué `expected_batch_id` no participa en la detección del archivo nuevo? Indicá qué parte del pipeline descubre `batch_003` y qué parte utiliza el parámetro.


20. Si la tarea `build_silver` falla, ¿qué ocurre con `build_gold` y `validate` en el Job? Explicá cómo las dependencias del DAG evitan publicar o validar resultados incompletos.
