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
| Calidad percibida (1 a 5) | Criterio personal | Pendiente |

## Observaciones

El modelo respondió correctamente a la prueba realizada mediante la API REST.
La latencia promedio obtenida fue de aproximadamente 3,62 segundos por respuesta.

