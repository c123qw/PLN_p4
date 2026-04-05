# PLN P4

Aplicacion TUI en Python para recuperar informacion sobre *Don Quijote de la Mancha* a partir de consultas en lenguaje natural. El proyecto implementa tres modos de trabajo:

- Busqueda clasica por lemas con ranking TF-IDF.
- Busqueda semantica por embeddings.
- RAG, combinando recuperacion clasica y semantica para generar una respuesta con Ollama.

## Participantes

- Carlos Mantilla Mateos - cmantill@ucm.es
- Diego Alonso Arceiz - diegal04@ucm.es

## Descripcion del proyecto

La aplicacion carga el corpus del Quijote desde un HTML, lo divide en chunks con overlap y analiza cada pasaje con spaCy. A partir de ese preprocesado se construyen dos formas de recuperacion:

- Recuperacion clasica: se eliminan stopwords, se lematiza la consulta y se ordenan los resultados con TF-IDF.
- Recuperacion semantica: cada chunk y cada consulta se representan como un embedding y se comparan mediante similitud coseno.

En el modo RAG se recuperan resultados de ambos enfoques, se fusionan con Reciprocal Rank Fusion (RRF) y los mejores pasajes se pasan a un modelo de Ollama para responder la consulta apoyandose en el texto recuperado.

## Estructura

- `p4.py`: interfaz Textual y orquestacion general.
- `preprocessing.py`: carga del corpus, chunking, lematizacion y calculo de embeddings.
- `modes/classic_mode.py`: busqueda clasica.
- `modes/semantic_mode.py`: busqueda semantica.
- `modes/rag_mode.py`: fusion de resultados y generacion con Ollama.
- `2000-h.htm`: corpus por defecto.

## Funcionamiento

Al iniciar la aplicacion se intenta procesar automaticamente `2000-h.htm`. Si el usuario introduce otra ruta, el corpus se vuelve a indexar con ese nuevo archivo.

La interfaz permite:

- Elegir el modo de consulta.
- Escribir la consulta en lenguaje natural.
- Indicar el modelo de Ollama que se quiere usar para RAG.
- Explorar los pasajes recuperados y sus scores.

## Ejecucion

Con `uv`:

```bash
uv run p4.py
```

## Notas

- El modo RAG requiere tener disponible la libreria `ollama` y un modelo instalado localmente.
- Los embeddings se guardan en memoria dentro de los `ChunkRecord` generados al indexar el corpus.
- La busqueda semantica actual funciona como un almacen vectorial simple en memoria, sin persistencia ni indice ANN.
