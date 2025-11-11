![logo-utn(1)](https://user-images.githubusercontent.com/70183535/190873516-80c3ce3f-310f-48bc-9a89-cd00ac7fbd6b.png)

# Proyecto Final - Gestión Avanzada de Datos (2024)
Proyecto para la materia Gestión Avanzada de Datos perteneciente a la carrera Ingeniería en Sistemas de Información en la UTN FRCU.
El proyecto consiste en el desarrollo de una aplicación para realizar búsquedas por similitud semántica sobre artículos científicos, utilizando una base de datos vectorial y embeddings de texto.

# Materia
* Gestión Avanzada de Datos

# Docentes
* Ing. Andrés Pascal
* Ing. Adrián Planas

# Equipo
* Forni, Lucas
* Lederhos, Federico
* Surracco, Santiago

# Descripción del Proyecto
Este proyecto aborda el **Problema de Asignación de Evaluadores (Reviewer Assignment Problem, RAP)**, un desafío clave en la organización de congresos científicos. La dificultad de asignar artículos a evaluadores idóneos se incrementa con el volumen de envíos y la alta especialización.

Nuestra solución propone un **buscador por similitud** que permite encontrar los artículos más relevantes dentro de una base de datos, basándose en el contenido semántico de un abstract proporcionado por el usuario.

La aplicación funciona de la siguiente manera:
1.  El usuario ingresa un texto (abstract) en la interfaz.
2.  El sistema genera un **embedding vectorial** de 1024 dimensiones para ese texto, utilizando el modelo BGE M3.
3.  Este vector de consulta se compara (usando **distancia coseno**) con los embeddings precalculados de 5.000 artículos almacenados en una base de datos **Postgres + PgVector**.
4.  La interfaz devuelve los 10 artículos más similares, ordenados por su grado de similitud.

Este enfoque supera las limitaciones de las búsquedas tradicionales por palabras clave, permitiendo encontrar contenido semánticamente similar, incluso si se utilizan términos diferentes, parafraseo o traducciones.

# Tecnología Utilizada
* **Base de Datos:** PostgreSQL con la extensión **PgVector**.
* **Lenguaje de Backend y Lógica:** Python.
* **Interfaz de Usuario (GUI):** **Streamlit**.
* **Modelo de Embeddings:** **BGE M3** (Beijing Academy of Artificial Intelligence - BAAI).
* **Procesamiento de Datos:** Librería **SentenceTransformer**, Pandas.
* **Métrica de Similitud:** Distancia Coseno.
* **Dataset:** 5.000 artículos del dataset de arXiv.

# Objetivos Específicos
* **Procesar y estructurar los datos:** Implementar un pipeline para procesar artículos, generar embeddings a partir de los abstracts y cargarlos en la base de datos vectorial.
* **Desarrollar el sistema de búsqueda por similitud:** Construir la lógica que permita al usuario ingresar un abstract y recupere los artículos más similares ordenados por una distancia.
* **Crear una interfaz de usuario interactiva:** Desarrollar una GUI que permita la entrada de texto y la visualización de los artículos recuperados.
* **Validar la precisión del modelo:** Evaluar la efectividad del modelo mediante un conjunto de pruebas controladas.
* **Validar los tiempos de respuesta:** Asegurar que los tiempos de respuesta en una búsqueda sean menores a 5 segundos por consulta (con radio de 5 artículos).

# Resultados de la Evaluación
Se generó una base de datos de pruebas con 50 abstracts parafraseados y/o traducidos.
* **Precisión:** **100% de acierto en el primer lugar**. En el 100% de las 50 consultas de prueba, el artículo original fue devuelto como el resultado más similar.
* **Rendimiento:** El tiempo total para las 50 consultas de prueba contra los 5.000 artículos fue de 17,869 segundos, con un promedio de **0.357 segundos por consulta**.

# Observaciones (Índices)
Para el tamaño del dataset (5.000 registros) no se hizo uso de índices. Sin embargo, se realizaron pruebas con los índices **IVFFlat** y **HNSW** (proporcionados por pgVector) para evaluar la escalabilidad.
* **HNSW:** Un poco más lento que IVFFlat, pero mantiene el mismo nivel de exactitud.
* **IVFFlat:** Siendo más rápido, pierde precisión.
