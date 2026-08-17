# Informe técnico - Caracterización del modelo local

## Caracterización del modelo

| Dato | Cómo obtenerlo | Valor |
|---|---|---|
| Perfil de hardware | Sección 2 de la guía | Perfil A |
| RAM total del equipo | `free -h` | 19 GiB |
| Modelo y etiqueta | `ollama list` | gemma3:270m |
| Tamaño en disco | `ollama list` | 676Gib|
| Latencia de 5 ejecuciones (ms) | `time curl` | 5432 ms, 2337 ms, 2883 ms, 3937 ms, 3505 ms |
| Latencia promedio | Promedio de las cinco | 3618,8 ms |
| RAM usada durante la inferencia | `free -h` mientras responde | 1.0 GiB |
| Calidad percibida (1 a 5) | Criterio personal | 4 |

## Observaciones

El modelo respondió correctamente a la prueba realizada mediante la API REST.
La latencia promedio obtenida fue de aproximadamente 3,62 segundos por respuesta.

##pruebas
## Pruebas
| ID | Tipo | Qué se verifica | Resultado esperado | Obtenido | Estado |
|---|---|---|---|---|---|
| P-01 | Funcional | GET /health responde | Código 200 y estado ok | Código 200 y estado ok | ✅ |
| P-02 | Funcional | POST /clasificar con motor eco | Código 200 y tipo correcto | Código 200 y tipo correcto | ✅ |
| P-03 | Funcional | Motor inválido | Código 400 | Código 400 | ✅ |
| P-04 | Acceso | Rol app_ia intenta DROP TABLE | Error de permisos | Error: must be owner of table inferencias | ✅ |
| P-05 | Conectividad | La API resuelve el host db | Devuelve una IP interna | Devuelve una IP interna | ✅ |
| P-06 | Disponibilidad | Reinicio del contenedor de BD | La API se recupera sola | La API se recuperó después del reinicio | ✅ |
| P-07 | Persistencia | down y up conservan los datos | Los registros siguen existiendo | Los registros siguen existiendo después de down y up | ✅ |
| P-08 | Carga | 10 usuarios sobre el motor eco | p95 < 800 ms y errores < 5 % | p95 = 56.92 ms; errores = 0.00 % | ✅ |
| P-09 | Caracterización | 10 inferencias con modelo | Promedio, mediana y p95 | Promedio = 828 ms; mediana = 436 ms; p95 = 1584 ms | ✅ |
##Análisis del cuello de botella

Al comparar P-08 y P-09 se observa una diferencia importante. En P-08, utilizando el motor eco bajo carga, la API presentó una latencia p95 de 56.92 ms y una tasa de errores de 0.00 %. En cambio, P-09, utilizando el modelo Ollama de forma secuencial, presentó un promedio de 828 ms, una mediana de 436 ms y un p95 de 1584 ms.

Esto indica que el principal cuello de botella está en la **inferencia del modelo**, no principalmente en la API ni en la base de datos. La diferencia entre ambas pruebas se debe principalmente al tiempo que necesita Ollama para procesar cada mensaje y generar la clasificación. La API y la base de datos tienen una participación mucho menor en comparación con el tiempo de inferencia del modelo.

### Propuestas de mejora

1. **Mantener el modelo cargado en memoria:** en equipos con memoria suficiente, mantener el modelo disponible reduce el tiempo necesario para cargarlo y puede mejorar la latencia de las primeras solicitudes.

2. **Agregar caché para mensajes repetidos:** si se reciben mensajes iguales o muy similares, se puede almacenar el resultado de la clasificación y devolverlo sin ejecutar nuevamente la inferencia.

3. **Limitar la concurrencia hacia el modelo:** controlar la cantidad de solicitudes simultáneas enviadas a Ollama puede evitar la saturación de los recursos del equipo y mantener tiempos de respuesta más estables.

4. **Utilizar el motor eco como filtro previo:** se pueden procesar primero los casos sencillos con el motor eco y enviar a Ollama únicamente los mensajes que necesiten una clasificación más compleja.
