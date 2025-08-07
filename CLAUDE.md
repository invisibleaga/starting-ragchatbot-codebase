# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Course Materials RAG (Retrieval-Augmented Generation) system built with FastAPI, ChromaDB, and Anthropic's Claude. It enables users to query course materials and receive intelligent, context-aware responses through semantic search.

## Development Commands

### Running the Application
```bash
# Quick start (preferred method)
./run.sh

# Manual start
cd backend
uv run uvicorn app:app --reload --port 8000
```

### Environment Setup
```bash
# Install dependencies
uv sync

# Set up environment variables
cp .env.example .env
# Then edit .env with your ANTHROPIC_API_KEY
```

### Application Access
- Web Interface: `http://localhost:8000`
- API Documentation: `http://localhost:8000/docs`

## Architecture Overview

### Core Components Architecture
The system follows a modular RAG architecture with these main components:

1. **RAGSystem** (`rag_system.py`) - Main orchestrator that coordinates all components
2. **VectorStore** (`vector_store.py`) - ChromaDB integration for semantic search using sentence-transformers
3. **AIGenerator** (`ai_generator.py`) - Anthropic Claude API integration with tool support
4. **DocumentProcessor** (`document_processor.py`) - Processes course documents into chunks
5. **SessionManager** (`session_manager.py`) - Manages conversation context
6. **ToolManager** (`search_tools.py`) - Provides search tools for Claude to use

### Data Models
- **Course**: Represents a course with title, description, and lessons
- **CourseChunk**: Text chunks from courses with metadata for vector storage
- **Lesson**: Individual lessons within courses

### API Endpoints
- `POST /api/query` - Submit questions to the RAG system
- `GET /api/courses` - Get course statistics and available courses

### Frontend Architecture
Simple vanilla JavaScript frontend (`frontend/`) with:
- Real-time chat interface
- Session management
- Course statistics display

## Key Configuration

### Environment Variables (`.env`)
- `ANTHROPIC_API_KEY` - Required for Claude AI integration

### Important Settings (`config.py`)
- `ANTHROPIC_MODEL`: "claude-sonnet-4-20250514" 
- `EMBEDDING_MODEL`: "all-MiniLM-L6-v2" (sentence-transformers)
- `CHUNK_SIZE`: 800 characters
- `CHUNK_OVERLAP`: 100 characters
- `MAX_RESULTS`: 5 search results
- `MAX_HISTORY`: 2 conversation exchanges

### Document Processing
- Supports PDF, DOCX, and TXT files in the `docs/` folder
- Documents are automatically loaded on startup
- Uses semantic chunking with overlap for better context preservation

### Tool-Enhanced Search
The system uses Claude's tool calling capabilities:
- Claude can search course content using the `CourseSearchTool`
- One search per query maximum to maintain performance
- Sources are tracked and returned with responses

## Data Storage
- **ChromaDB**: Vector database stored in `./chroma_db/`
- **Course metadata**: Stored separately from content chunks for optimized search
- **Automatic deduplication**: Existing courses are not re-processed on restart