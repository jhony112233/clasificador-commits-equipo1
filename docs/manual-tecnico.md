# Manual técnico — Clasificador de commits

## 1. Arquitectura

La solución está compuesta por una API desarrollada con FastAPI, una base
de datos PostgreSQL y dos motores de clasificación: el motor eco basado
en reglas y el motor Ollama utilizando el modelo `gemma3:270m`.

La arquitectura funciona de la siguiente manera:

```text
Cliente
   |
   | HTTP
   v
FastAPI
   |
   +--------------------+
   |                    |
   v                    v
Motor eco           Ollama
   |                 gemma3:270m
   |                    |
   +---------+----------+
             |
             v
       PostgreSQL
        (inferencias)
