
# ⚖️ Legal Brief Analyzer

> AI-Powered Legal Document Analysis System

A production-grade Flask application that uses AI to extract, analyze, and rank key legal arguments from PDF documents using RAG (Retrieval-Augmented Generation), vector search, and LLM analysis.

---

## 📋 Table of Contents

- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Installation](#-installation)
- [Configuration](#-configuration)
- [Usage](#-usage)
- [API Endpoints](#-api-endpoints)
- [File Details](#-file-details)

---

## ✨ Features

- 📄 **PDF Document Processing** - Upload and parse legal documents
- 🤖 **AI-Powered Analysis** - Extract key legal arguments using LLM (Groq/Llama 3.3)
- 🔍 **Vector Search** - FAISS-based semantic search for relevant chunks
- 📊 **Argument Ranking** - Intelligent scoring and ranking of legal points
- 🎯 **Metadata Extraction** - Page numbers, legal concepts, stance detection
- 🎨 **Modern UI** - Responsive web interface with real-time progress
- 🔐 **Production Ready** - Comprehensive validation, error handling, and logging

---

## 🛠️ Tech Stack

### Backend
- **Flask** - Web framework
- **Pydantic** - Configuration and data validation
- **LangChain + Groq** - LLM integration
- **FAISS** - Vector similarity search
- **Sentence Transformers** - Text embeddings
- **PDFPlumber** - PDF text extraction
- **Loguru** - Advanced logging

### Frontend
- **HTML5/CSS3** - Modern responsive design
- **Vanilla JavaScript** - No framework dependencies
- **Gradient UI** - Purple gradient theme

---

## 📁 Project Structure

```
legal-brief-analyzer/
│
├── app/                           # Main application package
│   ├── __init__.py               # Flask app factory
│   ├── config.py                 # Configuration management with Pydantic
│   ├── extensions.py             # Flask extensions initialization
│   │
│   ├── blueprints/               # Flask blueprints (routes)
│   │   ├── api/                  # API endpoints
│   │   │   ├── __init__.py
│   │   │   └── routes.py         # /api/analyze, /api/health endpoints
│   │   └── main/                 # Web page routes
│   │       ├── __init__.py
│   │       └── routes.py         # /, /results/<doc_id> endpoints
│   │
│   ├── models/                   # Data models
│   │   ├── __init__.py
│   │   └── schemas.py            # Pydantic models for legal data structures
│   │
│   ├── services/                 # Core business logic
│   │   ├── __init__.py
│   │   ├── pdf_processor.py     # PDF parsing and chunking
│   │   ├── metadata_extractor.py # Document metadata extraction
│   │   ├── vector_store.py      # FAISS vector database management
│   │   ├── llm_analyzer.py      # LLM-based argument extraction
│   │   ├── post_processor.py    # Ranking and deduplication
│   │   └── pipeline.py          # Main orchestration pipeline
│   │
│   ├── utils/                    # Utility functions
│   │   ├── __init__.py
│   │   ├── embeddings.py        # Sentence transformer embeddings
│   │   ├── helpers.py           # JSON parsing, hashing, text cleaning
│   │   └── validators.py        # File validation (PDF, size checks)
│   │
│   ├── static/                   # Frontend assets
│   │   ├── css/
│   │   │   └── style.css        # Complete styling (gradient theme)
│   │   └── js/
│   │       └── main.js          # Frontend utilities
│   │
│   └── templates/                # HTML templates
│       ├── base.html            # Base template with header/footer
│       ├── index.html           # Upload page
│       └── results.html         # Results display page
│
├── data/                         # Data directory
│   └── uploads/                 # Temporary PDF uploads (auto-created)
│
├── logs/                         # Application logs (auto-created)
│   └── app.log                  # Rotating log file
│
├── .env                          # Environment variables (not in repo)
├── .env.example                 # Example environment configuration
├── requirements.txt             # Python dependencies
├── run.py                       # Application entry point
└── README.md                    # This file
```

---

## 📦 Installation

### Prerequisites
- Python 3.9+
- pip (Python package manager)
- Groq API key ([Get one here](https://console.groq.com))

### Steps

1. **Clone the repository**
```bash
git clone <repository-url>
cd legal-brief-analyzer
```

2. **Create virtual environment**
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. **Install dependencies**
```bash
pip install -r requirements.txt
```

4. **Set up environment variables**
```bash
cp .env.example .env
```

Edit `.env` and add your API keys:
```env
GROQ_API_KEY=your-groq-api-key-here
SECRET_KEY=your-secret-key-here
```

Generate SECRET_KEY:
```bash
python -c "import secrets; print(secrets.token_hex(32))"
```

5. **Run the application**
```bash
python run.py
```

Visit `http://127.0.0.1:5000` in your browser.

---

## ⚙️ Configuration

All configuration is managed in `app/config.py` using Pydantic Settings.

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `GROQ_API_KEY` | Groq API key (required) | - |
| `SECRET_KEY` | Flask secret key | Auto-generated |
| `FLASK_ENV` | Environment (development/production) | production |
| `DEBUG` | Debug mode | False |
| `LLM_MODEL` | LLM model name | llama-3.3-70b-versatile |
| `LLM_TEMPERATURE` | LLM temperature | 0.0 |
| `EMBEDDING_MODEL` | Sentence transformer model | all-MiniLM-L6-v2 |
| `CHUNK_SIZE` | Text chunk size | 1500 |
| `CHUNK_OVERLAP` | Chunk overlap | 200 |
| `TOP_K_RETRIEVAL` | Initial retrieval count | 60 |
| `MAX_CONTENT_LENGTH` | Max file size (bytes) | 52428800 (50MB) |

---

## 🚀 Usage

### Web Interface

1. **Navigate to home page** (`/`)
2. **Upload a PDF** legal document (max 50MB)
3. **Wait for analysis** (30-60 seconds)
4. **View results** with ranked arguments

### API Usage

**Analyze Document**
```bash
curl -X POST http://127.0.0.1:5000/api/analyze \
  -F "file=@document.pdf"
```

**Response:**
```json
{
  "document_id": "abc123...",
  "document_name": "legal_brief.pdf",
  "total_pages": 45,
  "total_chunks": 123,
  "processing_time": 42.5,
  "key_points": [
    {
      "final_rank": 1,
      "summary": "Constitutional challenge to statute...",
      "importance": "Core constitutional argument",
      "importance_score": 0.95,
      "stance": "plaintiff",
      "supporting_quote": "The statute violates...",
      "legal_concepts": ["constitutional law", "due process"],
      "page_start": 12,
      "category": "constitutional",
      "combined_score": 0.92
    }
  ]
}
```

**Health Check**
```bash
curl http://127.0.0.1:5000/api/health
```

---

## 🔌 API Endpoints

### `POST /api/analyze`
Upload and analyze a legal document.

**Request:**
- Method: `POST`
- Content-Type: `multipart/form-data`
- Body: `file` (PDF document)

**Response:** JSON with extracted arguments and metadata

---

### `GET /api/health`
Check API health status.

**Response:**
```json
{
  "status": "healthy"
}
```

---

### `GET /`
Main upload page (web interface)

---

### `GET /results/<doc_id>`
View analysis results (web interface)

---

## 📄 File Details

### 🏗️ Core Application Files

#### `app/__init__.py`
**Purpose:** Flask application factory pattern  
**Functionality:**
- Loads configuration with validation
- Creates Flask app instance
- Registers blueprints (main, api)
- Initializes extensions

---

#### `app/config.py`
**Purpose:** Centralized configuration management  
**Functionality:**
- Pydantic-based settings with type validation
- Environment variable loading from `.env`
- Secure defaults for production
- Auto-creates upload directories
- Validates API keys and secret keys

**Key Classes:**
- `Settings` - Main configuration model
- `get_config()` - Configuration loader with error handling

---

#### `app/extensions.py`
**Purpose:** Initialize Flask extensions and logging  
**Functionality:**
- Configures Loguru for structured logging
- Sets up console and file logging
- Creates log rotation (5MB files, 7-day retention)
- Graceful fallback if logs directory unavailable

---

### 🌐 Blueprint Files (Routes)

#### `app/blueprints/main/routes.py`
**Purpose:** Web page routes  
**Endpoints:**
- `GET /` - Upload page
- `GET /results/<doc_id>` - Results page

---

#### `app/blueprints/api/routes.py`
**Purpose:** API endpoints for document analysis  
**Endpoints:**
- `POST /api/analyze` - Upload and analyze PDF
- `GET /api/health` - Health check

**Functionality:**
- File upload handling with security validation
- Temporary file management
- Pipeline orchestration
- Error handling and cleanup

---

### 📊 Data Models

#### `app/models/schemas.py`
**Purpose:** Pydantic data models for type safety  
**Models:**

1. **`DocumentType`** (Enum)
   - Legal document classification
   - Values: brief, motion, opinion, pleading, amicus_brief, other

2. **`Stance`** (Enum)
   - Argument stance/position
   - Values: plaintiff, defendant, amicus, for, against, neutral, unknown

3. **`ArgumentCategory`** (Enum)
   - Legal argument type
   - Values: statutory, regulatory, constitutional, case_law, procedural, policy, other

4. **`ExtractedPoint`**
   - Single legal argument extracted by LLM
   - Fields: summary, importance, stance, supporting_quote, legal_concepts, page_start/end, category, scores

5. **`FinalKeyPoint`**
   - Ranked argument with final position
   - Extends `ExtractedPoint` with `final_rank`

6. **`LLMAnalysisOutput`**
   - LLM response wrapper
   - Fields: extracted_points, confidence

---

### 🔧 Service Layer (Business Logic)

#### `app/services/pdf_processor.py`
**Purpose:** PDF parsing and text chunking  
**Class:** `PDFProcessor`

**Functionality:**
- Extracts text from PDF using PDFPlumber
- Splits text into overlapping chunks
- Preserves page number metadata
- Configurable chunk size and overlap
- Error handling for corrupted PDFs

**Key Methods:**
- `process_pdf(pdf_path, doc_id)` - Main processing entry point
- `_split_text_with_page()` - Text chunking with metadata

---

#### `app/services/metadata_extractor.py`
**Purpose:** Extract document metadata  
**Class:** `MetadataExtractor`

**Functionality:**
- Extracts document-level information
- Calculates total pages and chunks
- Generates document ID

**Key Methods:**
- `extract_metadata(chunks, doc_name)` - Extract metadata from chunks

---

#### `app/services/vector_store.py`
**Purpose:** FAISS vector database management  
**Class:** `VectorStore`

**Functionality:**
- Creates HNSW (Hierarchical Navigable Small World) index
- Stores document embeddings
- Semantic similarity search
- Configurable search parameters (M, efConstruction, efSearch)
- Distance-to-similarity score conversion

**Key Methods:**
- `initialize_index(doc_id)` - Create new FAISS index
- `add_chunks(chunks)` - Embed and index text chunks
- `search(query, top_k)` - Semantic search for relevant chunks

**Technical Details:**
- Uses `IndexHNSWFlat` for fast approximate nearest neighbor search
- Converts L2 distances to 0-1 similarity scores
- Handles NaN/Inf distances gracefully

---

#### `app/services/llm_analyzer.py`
**Purpose:** LLM-based legal argument extraction  
**Class:** `LLMAnalyzer`

**Functionality:**
- Sends retrieved chunks to Groq LLM
- Prompts for structured legal argument extraction
- Parses JSON responses
- Maps stances and categories
- Graceful fallback when LLM unavailable

**Key Methods:**
- `analyze_chunks(chunks_with_scores)` - Main LLM analysis
- `_prepare_context()` - Format chunks for LLM
- `_create_extraction_prompt()` - Generate analysis prompt

**Prompt Engineering:**
- Instructs LLM to act as "Supreme Court-level legal analyst"
- Requests specific fields: summary, importance, stance, quotes, legal concepts
- Enforces JSON output format
- Includes stance detection guidelines

---

#### `app/services/post_processor.py`
**Purpose:** Ranking and quality scoring  
**Class:** `PostProcessor`

**Functionality:**
- Combines multiple scoring signals
- Fuzzy matching for quote verification
- Deduplication (via scoring)
- Final ranking assignment

**Key Methods:**
- `process_and_rank(extracted_points, chunks)` - Main post-processing
- `_find_best_matching_chunk()` - Quote verification using RapidFuzz

**Scoring Formula:**
```
combined_score = 0.5 × importance_score 
                + 0.3 × retrieval_score 
                + 0.2 × match_confidence
```

---

#### `app/services/pipeline.py`
**Purpose:** Main orchestration pipeline  
**Class:** `AnalysisPipeline`

**Functionality:**
- Coordinates all analysis steps
- Manages service initialization
- Error handling and logging
- Timing and performance tracking

**Pipeline Steps:**
1. Validate and hash PDF file
2. Process PDF into chunks
3. Extract metadata
4. Initialize FAISS vector store
5. Embed and index chunks
6. Retrieve relevant chunks via semantic search
7. Analyze with LLM
8. Post-process and rank results
9. Return formatted JSON response

**Key Methods:**
- `analyze_document(pdf_path, filename)` - Main analysis flow

---

### 🛠️ Utility Files

#### `app/utils/embeddings.py`
**Purpose:** Text embedding generation  
**Class:** `EmbeddingService`

**Functionality:**
- Loads Sentence Transformer model
- Generates normalized embeddings
- Batch processing support
- GPU/CPU device selection

**Key Methods:**
- `encode(texts)` - Generate embeddings with normalization

**Default Model:**
- `sentence-transformers/all-MiniLM-L6-v2` (384 dimensions)

---

#### `app/utils/helpers.py`
**Purpose:** Helper functions for parsing and hashing  
**Functions:**

1. **`calculate_content_hash(content)`**
   - Generates SHA-256 hash of file content
   - Used for unique document IDs

2. **`parse_json_response(response_text)`**
   - Robust JSON parsing from LLM responses
   - Handles Markdown code fences
   - Extracts JSON from mixed text
   - Multiple fallback strategies

3. **`clean_text(text)`**
   - Normalizes whitespace
   - Strips extra spaces

**Parsing Strategies:**
- Direct JSON parsing
- Markdown code block extraction
- Bracket-based extraction
- Prefix removal

---

#### `app/utils/validators.py`
**Purpose:** Input validation and security  
**Functions:**

1. **`validate_pdf(file)`**
   - Checks file extension (.pdf)
   - Validates MIME type
   - Magic bytes verification (%PDF header)
   - Prevents disguised executables (e.g., file.pdf.exe)

2. **`validate_file_size(file, max_size)`**
   - Enforces file size limits
   - Checks for empty files
   - Graceful error handling

**Security Features:**
- Blocks double extensions
- Reads 1KB header for magic bytes
- MIME type verification

---

### 🎨 Frontend Files

#### `app/templates/base.html`
**Purpose:** Base HTML template  
**Features:**
- Common header with logo and title
- Main content block
- Footer with copyright
- Links to CSS and JS

---

#### `app/templates/index.html`
**Purpose:** Upload page  
**Features:**
- File upload form with drag-drop styling
- PDF-only file filter
- Real-time file name display
- Progress bar during analysis
- Error message display
- "How it works" section with 3 steps

**JavaScript Functionality:**
- Form submission handling
- File validation
- Progress indication
- API communication
- Result storage in sessionStorage
- Redirect to results page

---

#### `app/templates/results.html`
**Purpose:** Results display page  
**Features:**
- Document metadata display
- Ranked legal arguments
- Color-coded stance badges
- Page references
- Legal concepts tags
- Supporting quotes in blockquotes

**JavaScript Functionality:**
- Load results from sessionStorage
- Dynamic card generation
- Score formatting (percentage)
- Badge styling based on stance

---

#### `app/static/css/style.css`
**Purpose:** Complete application styling  
**Features:**
- Purple gradient background theme
- Responsive card layouts
- Modern button styles with hover effects
- Progress bar animation
- Badge color coding:
  - Plaintiff: Green
  - Defendant: Red
  - Neutral: Blue
  - Category: Light blue
  - Concepts: Yellow
- Mobile responsive design
- Smooth transitions and shadows

---

#### `app/static/js/main.js`
**Purpose:** Frontend utility functions  
**Functions:**
- `formatFileSize()` - Format bytes to human-readable
- `showToast()` - Toast notifications (optional)
- Console debugging

---

### 🚀 Entry Point

#### `run.py`
**Purpose:** Application entry point  
**Functionality:**
- Imports app factory
- Creates Flask application
- Runs development server on localhost:5000
- Debug mode for development

**Usage:**
```bash
python run.py
```

---

### 📦 Dependencies

#### `requirements.txt`
**Purpose:** Python package dependencies  
**Key Libraries:**

**Web Framework:**
- Flask 3.0.0 - Web framework
- Werkzeug 3.0.1 - WSGI utilities
- gunicorn 21.2.0 - Production WSGI server

**Data & Configuration:**
- pydantic 2.5.0 - Data validation
- pydantic-settings 2.1.0 - Settings management
- python-dotenv 1.0.0 - Environment variables

**PDF Processing:**
- pypdf 3.17.0 - PDF parsing
- pdfplumber 0.10.3 - Text extraction

**ML & AI:**
- faiss-cpu 1.7.4 - Vector search
- sentence-transformers 2.7.0 - Embeddings
- torch 2.1.0 - Deep learning framework
- groq 0.4.1 - LLM API client
- tiktoken 0.5.2 - Token counting

**Text Processing:**
- rapidfuzz 3.5.2 - Fuzzy string matching

**Logging:**
- loguru 0.7.2 - Advanced logging

---

### 🔐 Environment Configuration

#### `.env` (Not in repository)
**Purpose:** Store sensitive configuration  
**Required Variables:**
```env
GROQ_API_KEY=gsk_xxxxxxxxxxxxxxxxxxxxx
SECRET_KEY=64-character-hex-string
```

**Optional Variables:**
```env
FLASK_ENV=development
DEBUG=True
LLM_MODEL=llama-3.3-70b-versatile
CHUNK_SIZE=1500
TOP_K_RETRIEVAL=60
```

---

## 🔐 Security Features

- ✅ **File Validation** - Magic bytes and MIME type checking
- ✅ **Size Limits** - 50MB max file size
- ✅ **Secure Filenames** - Uses `secure_filename()` from Werkzeug
- ✅ **Temporary Files** - Auto-cleanup after processing
- ✅ **API Key Validation** - Pydantic validation at startup
- ✅ **Input Sanitization** - All inputs validated
- ✅ **Error Handling** - No sensitive data in error messages

---

## 📊 Analysis Pipeline Flow

```
1. PDF Upload
   ↓
2. File Validation (size, type, magic bytes)
   ↓
3. PDF Parsing → Text Extraction
   ↓
4. Text Chunking (1500 chars, 200 overlap)
   ↓
5. Embedding Generation (Sentence Transformers)
   ↓
6. FAISS Indexing (HNSW)
   ↓
7. Semantic Search (top 60 chunks)
   ↓
8. LLM Analysis (Groq/Llama 3.3)
   ↓
9. JSON Parsing & Validation
   ↓
10. Post-Processing (scoring, ranking)
    ↓
11. Return Top 10 Arguments
```

---

## 🐛 Error Handling

The application includes comprehensive error handling:

- **Configuration Errors** - Clear error messages for missing API keys
- **File Upload Errors** - Validation errors with user-friendly messages
- **PDF Processing Errors** - Handles corrupted PDFs gracefully
- **LLM Errors** - Fallback to empty results if LLM unavailable
- **JSON Parsing Errors** - Multiple parsing strategies
- **FAISS Errors** - NaN/Inf distance handling

All errors are logged with Loguru for debugging.

---

## 📝 Logging

**Log Locations:**
- Console: Real-time colored logs
- File: `logs/app.log` (rotated at 5MB, 7-day retention)

**Log Levels:**
- INFO - General application flow
- DEBUG - Detailed debugging information
- WARNING - Non-critical issues
- ERROR - Error conditions with stack traces

---

## 🚀 Production Deployment

### Gunicorn (Production WSGI Server)

```bash
gunicorn -w 4 -b 0.0.0.0:8000 "app:create_app()"
```

### Environment Variables for Production

```env
FLASK_ENV=production
DEBUG=False
SECRET_KEY=<secure-64-char-key>
GROQ_API_KEY=<your-key>
```

### Recommendations

- Use reverse proxy (Nginx)
- Enable HTTPS
- Set up proper firewall rules
- Monitor logs with log aggregation
- Use environment-specific configs

---

## 🤝 Contributing

This is a portfolio/interview project. For questions or feedback, please contact the developer.

---

## 📧 Contact

**Developer:** Aryan  
**Project:** Legal Brief Analyzer  
**Purpose:** Interview Submission for Daniel

---

## 📜 License

This project is created for interview purposes.

---

## 🙏 Acknowledgments

- **Groq** - Fast LLM inference
- **Meta** - Llama 3.3 model
- **FAISS** - Efficient similarity search
- **Sentence Transformers** - Text embeddings
- **Flask** - Web framework

---

**⭐ Thank you for reviewing this project!**
