# Clasificador de Commits con Inteligencia Artificial

## Descripción

Este proyecto implementa una API REST para clasificar mensajes de commits de un proyecto de software. La solución permite utilizar un motor basado en reglas denominado `eco` y un modelo de inteligencia artificial ejecutado localmente mediante Ollama. La API está desarrollada con FastAPI, utiliza PostgreSQL para almacenar el historial de inferencias y Docker Compose para facilitar la ejecución de todos los servicios.

## Integrantes

- Jhonatan Quintero
## Perfil de hardware utilizado

El proyecto fue desarrollado y probado en un equipo con las siguientes características:

| Recurso | Característica |
|---|---|
| Procesador | 12th Gen Intel(R) Core(TM) i5-1235U |
| Memoria RAM | 19 GB |
| Almacenamiento | 1007 GB |
| Espacio disponible durante las pruebas | Aproximadamente 950 GB |
| Arquitectura | Linux amd64 |
| Sistema operativo | Ubuntu 24.04 mediante WSL2 |
| Docker Engine | 29.7.0 |
| Python | 3.12 |
| Modelo de IA | gemma3:270m |
| Base de datos | PostgreSQL 16 Alpine |

## Requisitos mínimos

### Hardware

- 4 GB de RAM.
- 15 GB de espacio libre en disco.
- Procesador compatible con virtualización.
- Conexión a Internet.

### Software

- Windows 10 versión 2004 o superior con WSL2, o una distribución Linux.
- Ubuntu 24.04.
- Docker Engine.
- Docker Compose.
- Git.
- Python 3.12.
- Ollama.
- GitHub.

## Instalación

### 1. Clonar el repositorio

```bash
git clone git@github.com:USUARIO/clasificador-commits-equipo1.git
cd clasificador-commits-equipo1
