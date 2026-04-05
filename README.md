# RAG (Retrieval-Augmented Generation) Project

A comprehensive RAG implementation that processes YouTube video transcripts to enable question-answering using LangChain and OpenAI.

## Overview

This project demonstrates the complete RAG pipeline:

1. **Indexing** - Fetch and process YouTube video transcripts
2. **Retrieval** - Find relevant document chunks using similarity search
3. **Augmentation** - Combine retrieved context with user questions
4. **Generation** - Generate answers using LLM with retrieved context

## Features

- YouTube transcript fetching using `youtube-transcript-api`
- Document chunking with LangChain's `RecursiveCharacterTextSplitter`
- Vector embeddings using OpenAI's `text-embedding-3-small` model
- Similarity search with FAISS vector store
- Question answering using GPT-4o-mini

## Prerequisites

- **Python 3.12+**
- **UV package manager** - Install from [uv.rs](https://uv.rs/)
- **OpenAI API key** - Required for embeddings and chat completion

## Installation & Setup

### 1. Clone the Repository

```bash
git clone <repository-url>
cd RAG





### What the Script Does

1. **Fetches Transcript**: Downloads the transcript for YouTube video ID `Gfr50f6ZBvo`
2. **Processes Text**: Splits the transcript into manageable chunks (1000 characters with 200 overlap)
3. **Creates Embeddings**: Generates vector embeddings using OpenAI's embedding model
4. **Builds Vector Store**: Stores embeddings in FAISS for fast similarity search
5. **Answers Questions**: Responds to the question "is the topic of nuclear fusion discussed in this video? if yes then what was discussed"

### Customization

#### Change YouTube Video

Modify the `video_id` variable in `RAG-simple.py`:

```python
video_id = "your_youtube_video_id_here"
```

#### Ask Different Questions

Update the `question` variable:

```python
question = "your question here"
```

#### Adjust Chunking Parameters

Modify the text splitter configuration:

```python
splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,  # Size of each chunk
    chunk_overlap=200  # Overlap between chunks
)
```

## Project Structure

```
RAG/
├── RAG-simple.py              # Main RAG implementation
├── pyproject.toml             # UV project configuration
├── requirements.txt           # Generated dependencies list
├── uv.lock                   # UV lock file
├── .env                      # Environment variables (create this)
├── .venv/                    # Virtual environment
├── Copy_of_rag_using_langchain.ipynb  # Jupyter notebook version
├── agents_in_langchain.ipynb          # Agent examples
├── tools_in_langchain.ipynb           # Tool examples
└── tool_calling_in_langchain.ipynb    # Tool calling examples
```

## Dependencies

Key dependencies managed by UV:

- `langchain>=1.2.13` - Core LangChain framework
- `langchain-openai>=1.1.12` - OpenAI integration
- `langchain-community>=0.4.1` - Community tools
- `faiss-cpu>=1.13.2` - Vector similarity search
- `youtube-transcript-api>=1.2.4` - YouTube transcript fetching
- `python-dotenv>=1.2.2` - Environment variable management

## Troubleshooting

### Common Issues

1. **ModuleNotFoundError**: Ensure you've activated the virtual environment:

    ```bash
    source .venv/bin/activate
    ```

2. **Transcript Not Available**: Some videos don't have transcripts. The script handles this with a try-catch block.

3. **OpenAI API Errors**: Verify your API key is correctly set in the `.env` file.

### UV Commands

```bash
# Install new dependencies
uv add package_name

# Update dependencies
uv sync

# Run scripts without activating venv
uv run python RAG-simple.py

# Check for outdated packages
uv tree
```

## Advanced Usage

### Using Jupyter Notebooks

```bash
# Install Jupyter kernel for the project
uv add ipykernel

# Start Jupyter
uv run jupyter notebook
```


rag is a incontext model with short prompt tech

steps:
indexing -> retrieval -> augmentation -> generation


tool creation and calling 
- langchain_cummunity.tools - inbuilt tools , tool (custom tool creation decorator), structuredTool (needs pydantic but best for enterprise), 
- langchain.tools - basetool(wiht typing - type )
- tool combining using below process
- class MathToolkit:
    def get_tools(self):
        return [add, multiply]

Tool binding (few models can do )
- model.bind_tools([tool_name])


steps : 
1. tool creation using langchain_core.tools import tools and creation function (tool)
2. create a chatmodel and bind tool to it using model.bind_tools([tool_name])
3. if we invoke model with query then ai message having tool_call will be returned (having tool details and parameters that need for tool in ai message)
4. if again ai having this tool_call info it will give toolmessage having answer.
5. if this toolmessage is passed to model then it will give final response.

then the flow should be
```
query -> model -> tool_call ai message and tool message again give to model -> final response

```

 for sequential tool calling 
we need to use 
- langchain_core.tools import InjectedToolCall


what is AI agent?
- goal driven 
- autonomous planning 
- tool using
- context aware
- adaptive

so to create a actual ai agent we need to use 
- langchain.agents import create_react_agent, AgentExecutor # create react agent is a design pattern agent
- langchain import hub # predefined prompts

steps: 
create a aiagent
create a ai executor

