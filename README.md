# Simple Language Model Application with LangChain tutorial

This repository contains a simple Language Model (LLM) application built using LangChain. The application translates text from English into a target language, demonstrating basic components such as language models, prompt templates, and output parsers. You'll also learn to use LangChain Expression Language (LCEL) to chain components together and log application traces with LangSmith.


## Prerequisites

- Python 3.7+
- Jupyter Notebook

## Setup

1. **Clone this repository**:
   ```bash
   git clone https://github.com/yourusername/langchain-llm-translation
   cd langchain-llm-translation
   ```

2. **Install LangChain and Dependencies**:
   Install LangChain along with the required packages for the tutorial. You can use `pip`:
   ```bash
   pip install langchain langchain-openai jupyter
   ```

3. **Set Up LangSmith for Tracing**:
   Sign up for a LangSmith account and obtain an API key. Enable LangChain tracing by setting the environment variables:

   ```python
   import os
   import getpass

   os.environ["LANGCHAIN_TRACING_V2"] = "true"
   os.environ["LANGCHAIN_API_KEY"] = getpass.getpass()
   ```

## Usage

The main code file is `llm_translation.ipynb`, which contains the step-by-step implementation of the translation application.

To run the code:

1. **Open the Jupyter Notebook**:
   ```bash
   jupyter notebook llm_translation.ipynb
   ```

2. **Execute each cell** to follow the steps in building the LLM application.

## Components Overview

### 1. Using Language Models
LangChain supports multiple language models. In this project, we use `ChatOpenAI` to perform translation. Make sure to input your OpenAI API key:

```python
import os
from langchain_openai import ChatOpenAI
from langchain_core.messages import HumanMessage, SystemMessage

os.environ["OPENAI_API_KEY"] = getpass.getpass()
model = ChatOpenAI(model="gpt-4")
messages = [
    SystemMessage(content="Translate the following from English into Italian"),
    HumanMessage(content="hi!")
]
result = model.invoke(messages)
print(result)
```

### 2. Output Parsers
LangChain returns an AIMessage object with metadata. To retrieve the plain text output, we use `StrOutputParser`.

```python
from langchain_core.output_parsers import StrOutputParser

parser = StrOutputParser()
parsed_result = parser.invoke(result)
print(parsed_result)  # Output: 'Ciao!'
```

### 3. Prompt Templates
Use `PromptTemplates` to structure prompts dynamically. In this project, the template includes user input for `language` and `text`.

```python
from langchain_core.prompts import ChatPromptTemplate

system_template = "Translate the following into {language}:"
prompt_template = ChatPromptTemplate.from_messages(
    [("system", system_template), ("user", "{text}")]
)

result = prompt_template.invoke({"language": "italian", "text": "hi"})
print(result.to_messages())
```

### 4. Chaining Components with LCEL
LangChain's LCEL allows chaining components using the `|` operator, combining the prompt template, model, and output parser.

```python
# Chain Template, Model, and Parser
chain = prompt_template | model | parser
output = chain.invoke({"language": "italian", "text": "hi"})
print(output)  # Output: 'Ciao!'
```

