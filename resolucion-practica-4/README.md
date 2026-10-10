<div align="center">
<b>Log y offsets (01_log_y_offsets)</b>
</div>

1. En el ejemplo en Python, ¿cuántos mensajes quedaron en la cola y cuántos en el log después de leer tres? ¿Qué diferencia hay entre una cola tradicional y un log como Kafka?
>a

2. Después de la llegada 2, ¿cuántos mensajes había leído el consumidor antifraude y cuántos reportes? ¿El atraso de reportes afectó a antifraude? ¿Por qué?
>a

3. ¿Qué es un offset y dónde lo guarda cada consumidor en esta práctica?
>a

4. ¿Cuántos mensajes leyó el replay? ¿Qué propone la arquitectura Kappa para reprocesar datos y en qué se diferencia de Lambda?
>a

___

<div align="center">
<b>Ingesta y enriquecimiento (02_ingesta_y_enriquecimiento)</b>
</div>

5. ¿Cuántas filas nuevas procesó Bronze en cada llegada y cuántas al reejecutar sin archivos nuevos? ¿Qué componente evita leer dos veces el mismo archivo?
>a
6. ¿Qué motivos de rechazo aparecieron en la cuarentena y en qué llegada? ¿Por qué Bronze guarda el texto original en lugar de descartar los registros con errores?
>a
7. ¿Qué columnas agregó el join con silver_customers y silver_products? ¿Qué tipo de join es (stream-stream, stream-tabla o tabla-tabla) según la teoría?
>a

___

<div align="center">
<b>Tiempo y ventanas (03_tiempo_y_ventanas)</b>
</div>

8. ¿Qué diferencia hay entre event time y processing time? Usá late_ok como ejemplo.
>a
9. ¿Cuánto valía el watermark al terminar cada llegada? ¿Cómo se calcula a partir del máximo event time visto?
>a
10. ¿En qué llegada aparecieron las primeras ventanas tumbling en modo append y por qué no antes?
>a
11. ¿Qué pasó con late_bad? ¿Por qué se aceptó late_ok y se descartó late_bad?
>a
12. Mirando el gráfico de ventanas, ¿en qué se diferencian tumbling, hopping y session? Da un ejemplo de negocio para cada una.
>a
13. Para la ventana [12:00, 12:05) del canal card, ¿cuántas veces y con qué valores se emitió en modo update y en modo append? ¿Qué ventaja y qué desventaja tiene cada modo?
>a
14. ¿Qué se gana y qué se pierde si el watermark fuera de 1 minuto en lugar de 10?
>a

___

<div align="center">
<b>Garantías y fallas (04_garantias_y_fallas)</b>
</div>

15. ¿Cuántas filas quedaron en el destino sin deduplicar y cuántas deduplicando por event_id? ¿Por qué el productor puede enviar el mismo evento dos veces?
>a
16. ¿Qué pasó al reiniciar con el mismo checkpoint y qué pasó al perderlo? ¿Qué garantía de entrega se observa en cada caso?
>a
17. ¿Por qué con MERGE el resultado no cambió aunque se perdiera el checkpoint? ¿Qué significa que una escritura sea idempotente?
>a

___

<div align="center">
<b>CDC (05_cdc)</b>
</div>

18. ¿Cuántos eventos de cambio generó el log y de qué tipos? ¿Por qué un UPDATE genera dos eventos?
>a
19. ¿Se pudo reconstruir la tabla a partir del log? ¿Qué información tiene el log que la tabla actual no tiene?
>a

___

<div align="center">
<b>Cierre</b>
</div>

20. Según la comparación de motores de la teoría, ¿cuándo elegirías Spark Structured Streaming, Flink o Kafka Streams? Mencioná una ventaja de cada uno.
>a
