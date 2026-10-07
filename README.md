# LangChain Chatbot with Memory

A conversational chatbot using LangChain, the OpenAI API (GPT-3.5-turbo) and Gradio, deployed on Hugging Face Spaces. It remembers earlier messages in the chat, so replies use the conversation so far.

> **Credit:** This is a GenAI workshop project. The `app.py` and `requirements.txt` were provided in the workshop (source: [WORKSHOP NAME / LINK]). My part: running the notebook, deploying the files to a Hugging Face Space, configuring the API key as a Space secret, and testing the chatbot. Built in 2023 with the library versions of that time.

## What the Code Does
- Answers general user queries with GPT-3.5-turbo (temperature 0.5)
- `ConversationBufferMemory` stores chat history and passes it to the model through a `PromptTemplate` with `{chat_history}`
- `LLMChain` connects the prompt, memory and model
- Gradio `ChatInterface` provides the chat UI
- The API key is read from the `OPENAI_API_KEY` environment variable

## Tech Stack
Python, LangChain, OpenAI API, Gradio, Hugging Face Spaces, Jupyter Notebook

## Run Locally
The code uses 2023-era LangChain imports, so install the older versions below in a clean virtual environment (Python 3.10 or 3.11 recommended).

**Windows (PowerShell)**
```powershell
python -m venv venv
venv\Scripts\Activate.ps1
pip install -r requirements.txt
$env:OPENAI_API_KEY="your-key-here"
python app.py
```

**Mac / Linux**
```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
export OPENAI_API_KEY="your-key-here"
python app.py
```

Then open the local URL Gradio prints (usually `http://127.0.0.1:7860`). You need your own OpenAI API key with available credit.

## Publishing the Code to Hugging Face
Done from a Jupyter/Colab notebook:

```python
from huggingface_hub import notebook_login
notebook_login()

from huggingface_hub import HfApi
api = HfApi()

HUGGING_FACE_REPO_ID = "<Hugging Face User Name/Repo Name>"
```

Download the project files:

```
%mkdir /content/ChatBotWithOpenAI
!wget -P /content/ChatBotWithOpenAI/ <app.py link from the workshop>
!wget -P /content/ChatBotWithOpenAI/ <requirements.txt link from the workshop>
%cd /content/ChatBotWithOpenAI
```

Upload them to the Space:

```python
api.upload_file(
    path_or_fileobj="./requirements.txt",
    path_in_repo="requirements.txt",
    repo_id=HUGGING_FACE_REPO_ID,
    repo_type="space")

api.upload_file(
    path_or_fileobj="./app.py",
    path_in_repo="app.py",
    repo_id=HUGGING_FACE_REPO_ID,
    repo_type="space")
```

### Adding the OpenAI key as a Space secret
1. Open your Space and click **Settings**.
2. Go to **Variables and secrets**.
3. Create a new secret named `OPENAI_API_KEY` with your key as the value.

## Notes and Limitations
- The Hugging Face Space is no longer running because the libraries have changed since 2023.
- Newer `langchain` and `openai` versions may break the imports, which is why `requirements.txt` pins older versions.
- Memory is one global object, so all users of one running app share a single history.
- Running it needs your own OpenAI API key.

## Files
- `app.py`: chatbot logic and Gradio interface
- `requirements.txt`: pinned dependencies
- `README.md`: this file
