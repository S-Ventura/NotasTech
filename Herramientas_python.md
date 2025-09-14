 # Herramientas python que explorar
 ## 🔗 1. SQLAlchemy

Uso: Conectarte y trabajar con bases de datos desde Python.

Es un ORM (Object-Relational Mapper).

Te permite escribir consultas SQL usando Python, sin escribir SQL puro (aunque también puedes hacerlo si quieres).

Soporta muchos motores: PostgreSQL, MySQL, SQLite, etc.

Ideal para cuando quieres estructurar tu acceso a base de datos en aplicaciones grandes o complejas.

```bash
from sqlalchemy import create_engine
engine = create_engine("sqlite:///data.db")
```
## 🧠 2. LangChain

Uso: Construir aplicaciones que usen modelos de lenguaje (LLMs) como GPT, Claude, Mistral, etc.

Sirve para orquestar LLMs junto con datos, herramientas, APIs, y memoria.

Te ayuda a crear agentes, chatbots, RAGs (retrieval-augmented generation), y más.

Soporta muchas plataformas: OpenAI, Ollama, Cohere, Hugging Face, etc.

🧠 Ejemplo típico:
```bash
from langchain.llms import OpenAI
llm = OpenAI()
response = llm.predict("Dime un chiste")
```
## 🤖 3. Ollama

Uso: Ejecutar modelos LLM localmente (en tu PC), sin necesidad de conectarte a la nube.

Puedes correr modelos como LLaMA, Mistral, Code LLaMA, Phi-3, etc., localmente.

Ollama expone una API HTTP local, que puedes usar con requests, LangChain, etc.

Muy útil para desarrollo privado, sin depender de OpenAI o Hugging Face.

🧠 Ejemplo típico:

```bash
ollama run mistral

```
## 🌐 4. Requests

Uso: Hacer peticiones HTTP desde Python.

Ideal para conectarte a APIs REST, descargar archivos, enviar datos a servidores, etc.

Muy simple y poderoso.

🧠 Ejemplo típico:
```bash
import requests
response = requests.get("https://api.example.com/data")
data = response.json()

```
## 💻 5. Streamlit

Uso: Crear interfaces web interactivas para tus scripts de Python, ¡sin ser desarrollador web!

Se usa mucho para dashboards, prototipos de apps de ML o data science.

Muy fácil de usar: solo escribes Python, y Streamlit genera el frontend.

Ideal para mostrar modelos, resultados o interfaces para clientes internos.

🧠 Ejemplo típico:


```bash
import streamlit as st

st.title("Demo de modelo")
name = st.text_input("Tu nombre")
st.write("Hola", name)

```
🧩 ¿Cómo se combinan?

En un proyecto, podrías usar todo así:

SQLAlchemy para extraer datos de una base.

LangChain + Ollama para consultar un LLM local usando esos datos.

Requests para conectarte a APIs externas o la API local de Ollama.

Streamlit para mostrarle al usuario una interfaz interactiva de todo eso.

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```