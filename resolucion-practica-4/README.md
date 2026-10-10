
# ENTREGA PRÁCTICA 2 – STREAMING</b>


- **Nombre: Santiago Sánchez Jeréz**
- **student_id: santiago**
- **Escala: small**

<div align="center">
<b>Log y offsets (01_log_y_offsets)</b>
</div>

1. En el ejemplo en Python, ¿cuántos mensajes quedaron en la cola y cuántos en el log después de leer tres? ¿Qué diferencia hay entre una cola tradicional y un log como Kafka?
>En la cola quedaron los 2 mensajes no leídos, mientras que en el log permanecen los 5 originales. En una cola tradicional, los datos son consumidos, a medida que se leen, se borran; en un log Kafka, lo único que hay es un cambio en la posición del consumidor (offset), pero los mensajes se mantienen. 

2. Después de la llegada 2, ¿cuántos mensajes había leído el consumidor antifraude y cuántos reportes? ¿El atraso de reportes afectó a antifraude? ¿Por qué?
>Después de la llegada 2, antifraude leyó 120 mensajes y reportes 0. El atraso de reportes no afecta a antifraudes, pues son independientes uno de otro, cada cual con su propio checkpoint. Esto es posible gracias a que se está trabajando con un log de Kafka, que no borra lo que se lee, permitiendo que cada consumidor lo haga a su tiempo. 

3. ¿Qué es un offset y dónde lo guarda cada consumidor en esta práctica?
>El offset es la posición de un mensaje en su partición, la última versión del topic que leyó leyó cada consumidor. En esta práctica, el offset se guarda como una variable cuyo valor equivale a dónde se quedó el consumidor correspondiente (antifraude o reportes). 

4. ¿Cuántos mensajes leyó el replay? ¿Qué propone la arquitectura Kappa para reprocesar datos y en qué se diferencia de Lambda?
>Replay leyó 280 mensajes. La arquitectura Kappa propone un sólo código de steraming, o sea que reprocesar es hacer una consulta nueva. En cambio, la arquitectura Lambda propone una dicotomía: un flujo batch y otro stream, con la misma lógica escrita dos veces

___

<div align="center">
<b>Ingesta y enriquecimiento (02_ingesta_y_enriquecimiento)</b>
</div>

5. ¿Cuántas filas nuevas procesó Bronze en cada llegada y cuántas al reejecutar sin archivos nuevos? ¿Qué componente evita leer dos veces el mismo archivo?
>Con respecto a las filas procesadas, Bronze leyó, para cada archivo nuevo, en orden: 60, 120, 40, 40 y 20. Al releer, no se procesó fila alguna.\
>El componente que evita leer dos veces el mismo archivo es "Auto Loader". Es el "offset" para archivos.
6. ¿Qué motivos de rechazo aparecieron en la cuarentena y en qué llegada? ¿Por qué Bronze guarda el texto original en lugar de descartar los registros con errores?
>Los motivos de rechazo fueron o bien cantidad inválida o bien consumidor desconocido. Esto ocurrió en la segunda llegada. \
>Bronze guarda el texto original sin ningún tipo de descarte porque los datos crudos son valiosos, y algo que dentro de cierto pipeline se descarta puede ser requerido en otro; y puede pasar, por ejemplo, que haya un cambio de contrato y que mañana se quiera cambiar el pipeline. Por eso, Bronze no descarta nada. 
7. ¿Qué columnas agregó el join con silver_customers y silver_products? ¿Qué tipo de join es (stream-stream, stream-tabla o tabla-tabla) según la teoría?
>Agregó las columnas de _country_ y _category_. El join es de tipo «stream-tabla».

___

<div align="center">
<b>Tiempo y ventanas (03_tiempo_y_ventanas)</b>
</div>

8. ¿Qué diferencia hay entre event time y processing time? Usá late_ok como ejemplo.
>Event time es el momento en el que ocurre –se registra– el evento, mientras que processing time es el momento en el que este se procesa. El primero entrará o no dentro de un procesamiento dependiendo del tipo de el tipo de ventana y del processing time. Por ejemplo, una consulta con _tumbling window_ incluirá como _late_ok_ aquellos eventos cuyo _time event_ esté a menos de 5 minutos de distancia del momento en el que ésta se ejecuta (_processing time_).
9. ¿Cuánto valía el watermark al terminar cada llegada? ¿Cómo se calcula a partir del máximo event time visto?
> El _watermark_ valía 11:54, 11:58, 12:16, 12:17 y 12:35 al terminar, respectivamente, las llegadas 1, 2, 3, 4 y 5. A partir del _máximo event time visto_ se restan 10 minutos y ese es el _watermark_
10. ¿En qué llegada aparecieron las primeras ventanas tumbling en modo append y por qué no antes?
>Las ventanas tumbling aparecen recién en la llegada 3. No aparecieron antes porque la distancia entre events en las salidas 1 y 2 no llega a superar los 5 minutos, que es el _tresshold_ de cierre de ventana. 
11. ¿Qué pasó con late_bad? ¿Por qué se aceptó late_ok y se descartó late_bad?
>En la llegada 4 hay 20 eventos más viejos que el watermark, que fueron descartados: no suman en ninguna ventana. La razón para hacer esto es que se prefiere mantener el pipeline simple y predecible, evitando dobles ventanas, antes que preservar el dato. 
12. Mirando el gráfico de ventanas, ¿en qué se diferencian tumbling, hopping y session? Da un ejemplo de negocio para cada una.
>Tumbling consiste en ventanas que son particiones complementarias en función del tiempo. Este tipo de ventana será más valorado por negocios que no se preocupen por el movimiento continuo de sus datos, sino más bien por fotos en el tiempo.\
>Hopping consiste en vantanas que son particiones en función del tiempo, pero no son complementarias, sino que se superponen, porque la cantidad de tiempo que debe pasar para hacer una ventana nueva es más chica que la cantidad de tiempo "hacia atrás y adelante" que la ventana atrapa. Negocios que se preocupen por la media movil de sus datos valorarán esto. \
>Session se distingue porque la partición en el tiempo es al mismo tiempo una partición por cliente: se establecen parámetros de finalización de captura para cada emisor de datos. Por ejemplo, si dejás el celular sobre la mesa con un reel de instagram corriendo en loop y te vas a cocinar, Instagram debería tener un session tresshold (por ejemplo en función de toques de pantalla) que le permita cortar ese influjo de datos y darse cuenta de que el reel no te encanta, sino que no estás viendo el celular. Con Netflix lo mismo, si te quedaste dormido viendo la serie no es lo mismo que si la estás maratoneando a lo loco: el "¿sigues ahí?" sirve le sirve tanto al cliente del servicio Netflix (vos, que estás encantado o embolado con la serie) como al consumidor de datos. 
13. Para la ventana [12:00, 12:05) del canal card, ¿cuántas veces y con qué valores se emitió en modo update y en modo append? ¿Qué ventaja y qué desventaja tiene cada modo?
>Update tiene la ventaja de que las ventanas se corrigen, incorporando datos que llegan tarde; pero el precio a pagar es que se reciba la misma ventana dos veces. \
>Append se emite una sola vez, cuando el watermark cierra, y no da lugar a dobles lecturas, pero tarda más.\
>En el caso de la práctica, para la ventana 12:00-12:05, se emitió dos veces en _update_, con 6000.00 a la primera y 8400.00 luego; y una sola vez con _append_, con el valor final de 8400.00.
14. ¿Qué se gana y qué se pierde si el watermark fuera de 1 minuto en lugar de 10?
>Hay más instancias de procesamiento, lo que es más costoso, pero a cambio mantiene al consumidor más actualizado. 

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
