# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Running the Application
- **Start the server**: `./run.sh` (or `cd backend && uv run uvicorn app:app --reload --port 8000`)
- **Install dependencies**: `uv sync`
- **Environment setup**: Create `.env` file with `ANTHROPIC_API_KEY=your_key_here`

### Dependencies Management
- This project uses `uv` as the Python package manager
- Dependencies are defined in `pyproject.toml`
- The project requires Python 3.13+

## Architecture Overview

This is a **Retrieval-Augmented Generation (RAG) system** for course materials with the following key architecture:

### Core Components
- **RAGSystem** (`rag_system.py`): Main orchestrator that coordinates all components
- **VectorStore** (`vector_store.py`): ChromaDB-based vector storage for semantic search
- **AIGenerator** (`ai_generator.py`): Claude API integration with tool-calling capabilities
- **DocumentProcessor** (`document_processor.py`): Processes course documents into structured chunks
- **SessionManager** (`session_manager.py`): Manages conversation history and user sessions

### Data Flow
1. Course documents (PDF, DOCX, TXT) are processed into `Course` and `CourseChunk` objects
2. Content is embedded and stored in ChromaDB collections
3. User queries trigger semantic search through tools
4. Claude generates responses using search results and conversation context
5. Frontend communicates via FastAPI REST endpoints

### Key Design Patterns
- **Tool-based search**: Claude uses search tools rather than direct RAG context injection
- **Session-based conversations**: Maintains conversation history per session
- **Modular component architecture**: Each component has a single responsibility
- **Pydantic models**: Strong typing for data structures (`Course`, `Lesson`, `CourseChunk`)

### Collections Structure
- `course_content`: Main document chunks for semantic search
- `course_metadata`: Course and lesson metadata
- Duplicate detection based on course titles

### Frontend Integration
- Static files served from `/frontend/` directory
- REST API endpoints under `/api/`
- WebSocket-style session management for conversations

## Configuration
- All settings centralized in `config.py`
- Uses dataclass pattern with environment variable loading
- Key settings: chunk size (800), overlap (100), max results (5), conversation history (2)

## File Processing
- Supports PDF, DOCX, and TXT course materials
- Documents expected in `/docs/` directory
- Automatic course detection and duplicate prevention
- Structured extraction of course titles, lessons, and content