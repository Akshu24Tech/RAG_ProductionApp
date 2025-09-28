# RAG Production App

A production-ready Retrieval-Augmented Generation (RAG) application that allows users to upload PDF documents, process them into a vector database, and query them using natural language with AI-powered responses.

## Features

- **PDF Document Ingestion**: Upload and process PDF files into searchable chunks
- **Vector Storage**: Uses Qdrant for efficient similarity search
- **AI-Powered Querying**: Ask questions about your documents using OpenAI's GPT models
- **Workflow Management**: Built with Inngest for reliable background processing
- **Web Interface**: Streamlit-based UI for easy document upload and querying
- **Production Ready**: FastAPI backend with proper error handling and rate limiting

## Architecture

The application consists of several key components:

- **FastAPI Backend** (`main.py`): Handles Inngest workflows for PDF processing and querying
- **Streamlit Frontend** (`streamlit_app.py`): User interface for document upload and querying
- **Vector Database** (`vector_db.py`): Qdrant integration for storing and searching embeddings
- **Data Processing** (`data_loader.py`): PDF parsing and text embedding using OpenAI
- **Type Definitions** (`custom_types.py`): Pydantic models for data validation

## Prerequisites

- Python 3.9+
- Qdrant vector database (running locally or remotely)
- OpenAI API key
- Inngest development server

## Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd rag-productionapp
```

2. Install dependencies:
```bash
pip install -e .
```

3. Set up environment variables:
```bash
cp .env.example .env
# Edit .env and add your OpenAI API key
```

## Configuration

### Environment Variables

Create a `.env` file with the following variables:

```env
OPENAI_API_KEY=your-openai-api-key-here
INNGEST_API_BASE=http://127.0.0.1:8288/v1  # Optional, defaults to local dev server
```

### Qdrant Setup

By default, the application expects Qdrant to be running on `http://localhost:6333`. You can:

1. **Run Qdrant locally with Docker**:
```bash
docker run -p 6333:6333 qdrant/qdrant
```

2. **Use Qdrant Cloud**: Update the `QdrantStorage` initialization in `vector_db.py` with your cloud URL and API key.

## Usage

### 1. Start the Inngest Development Server

```bash
npx inngest-cli@latest dev
```

### 2. Start the FastAPI Backend

```bash
uvicorn main:app --reload --port 8000
```

### 3. Start the Streamlit Frontend

```bash
streamlit run streamlit_app.py
```

### 4. Use the Application

1. **Upload PDFs**: Navigate to the Streamlit interface and upload PDF documents
2. **Wait for Processing**: The system will automatically chunk and embed your documents
3. **Ask Questions**: Use the query interface to ask questions about your uploaded documents
4. **Get AI Responses**: Receive contextual answers based on your document content

## API Endpoints

The FastAPI backend exposes Inngest webhook endpoints:

- `POST /api/inngest` - Inngest webhook endpoint for workflow execution

## Workflows

### PDF Ingestion Workflow (`rag/ingest_pdf`)

- **Trigger**: Manual event with PDF path
- **Process**: 
  1. Load and chunk PDF content
  2. Generate embeddings using OpenAI
  3. Store in Qdrant vector database
- **Rate Limiting**: 2 requests per minute, 1 per 4 hours per source

### PDF Query Workflow (`rag/query_pdf_ai`)

- **Trigger**: Manual event with question
- **Process**:
  1. Embed the question
  2. Search for relevant document chunks
  3. Generate AI response using retrieved context
- **Output**: Answer, sources, and context count

## Project Structure

```
rag-productionapp/
├── main.py              # FastAPI backend with Inngest workflows
├── streamlit_app.py     # Streamlit frontend interface
├── vector_db.py         # Qdrant vector database integration
├── data_loader.py       # PDF processing and embedding logic
├── custom_types.py      # Pydantic data models
├── .env                 # Environment variables
├── pyproject.toml       # Project dependencies
├── README.md           # This file
└── uploads/            # Directory for uploaded PDFs (created automatically)
```

## Dependencies

Key dependencies include:

- **FastAPI**: Web framework for the backend API
- **Streamlit**: Frontend web interface
- **Inngest**: Workflow orchestration and background jobs
- **Qdrant Client**: Vector database integration
- **OpenAI**: Embeddings and chat completions
- **LlamaIndex**: PDF reading and text processing
- **Pydantic**: Data validation and serialization

## Development

### Running Tests

```bash
# Add your test commands here
pytest
```

### Code Quality

```bash
# Format code
black .

# Lint code
flake8 .
```

## Deployment

For production deployment:

1. **Set up Qdrant**: Use Qdrant Cloud or deploy your own instance
2. **Configure Inngest**: Set up Inngest Cloud for production workflows
3. **Environment Variables**: Update `.env` with production values
4. **Deploy Backend**: Deploy FastAPI app to your preferred platform
5. **Deploy Frontend**: Deploy Streamlit app (consider using Streamlit Cloud)

## Troubleshooting

### Common Issues

1. **Qdrant Connection Error**: Ensure Qdrant is running and accessible
2. **OpenAI API Error**: Verify your API key is valid and has sufficient credits
3. **Inngest Timeout**: Check that the Inngest dev server is running
4. **PDF Processing Error**: Ensure uploaded files are valid PDFs

### Logs

Check the Inngest dashboard for workflow execution logs and debugging information.

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## License

[Add your license information here]

## Support

For questions or issues, please [create an issue](link-to-issues) or contact [your-contact-info].