# PromptVault

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.9+-blue.svg" alt="Python">
  <img src="https://img.shields.io/badge/License-MIT-green.svg" alt="License">
  <img src="https://img.shields.io/badge/Version-1.0.0-orange.svg" alt="Version">
  <img src="https://img.shields.io/badge/Prompts-Version%20Controlled-blueviolet.svg" alt="Prompts">
</p>

> **The definitive prompt engineering library with version control, search, and collaboration**

PromptVault is a comprehensive prompt engineering library that brings software engineering best practices to AI prompt development. Store, version, search, share, and manage your prompts with the same rigor you'd apply to source code.

---

## 🎬 Demo
![PromptVault Demo](demo.gif)

*Prompt versioning and management*

## Screenshots
| Component | Preview |
|-----------|---------|
| Prompt Editor | ![editor](screenshots/prompt-editor.png) |
| Version History | ![versions](screenshots/version-history.png) |
| Test Results | ![tests](screenshots/test-results.png) |

## Visual Description
Prompt editor shows prompts with variable substitution. Version history displays diffs between prompt versions. Test results show evaluation metrics for prompt variants.

---


## Table of Contents

- [Features](#features)
- [Architecture](#architecture)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Usage Examples](#usage-examples)
- [API Reference](#api-reference)
- [Configuration](#configuration)
- [Prompt Templates](#prompt-templates)
- [CLI Reference](#cli-reference)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

---

## Features

### 🔐 **Store**
- **Persistent Storage**: Save prompts with metadata, tags, and descriptions
- **Structured Organization**: Hierarchical categories and collections
- **Rich Metadata**: Track author, created date, modified date, version, usage count
- **Multiple Storage Backends**: SQLite (default), PostgreSQL, MongoDB, Redis
- **Encryption Support**: Encrypt sensitive prompts at rest

### 🔍 **Search**
- **Full-Text Search**: Search prompt content with instant results
- **Semantic Search**: Find prompts by meaning, not just keywords
- **Advanced Filtering**: Filter by tags, categories, date ranges, authors
- **Fuzzy Matching**: Handle typos and approximate matches
- **Highlighting**: Highlight search terms in results

### 👥 **Share**
- **Export/Import**: Share prompts as JSON, YAML, or Markdown files
- **Team Collaboration**: Share collections with team members
- **Public Registry**: Publish prompts to a public registry
- **Access Control**: Fine-grained permissions (read, write, admin)
- **Version Linking**: Share specific versions of prompts

### 📜 **Version Control**
- **Git-like Versioning**: Full version history with commit messages
- **Branching**: Create feature branches for prompt experimentation
- **Diff Viewing**: Compare versions side-by-side
- **Rollback**: Revert to any previous version instantly
- **Merge**: Combine changes from different branches

### ⚡ **Additional Features**
- **CLI Interface**: Full-featured command-line tool
- **REST API**: HTTP API for programmatic access
- **Python SDK**: Native Python library
- **Template System**: Parameterized prompts with variable substitution
- **Analytics**: Track prompt usage and performance
- **Web Dashboard**: Visual interface for prompt management

---

## Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              PromptVault Architecture                        │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                         User Interfaces                              │   │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────────────┐    │   │
│  │  │   CLI    │  │ REST API │  │   SDK    │  │  Web Dashboard  │    │   │
│  │  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────────┬─────────┘    │   │
│  └───────┼─────────────┼─────────────┼─────────────────┼──────────────┘   │
│          └─────────────┴──────┬──────┴──────────────────┘                   │
│                               ▼                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                         Core Services                                │   │
│  │  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌──────────────┐   │   │
│  │  │  Prompt    │  │  Version   │  │   Search   │  │   Sharing    │   │   │
│  │  │  Manager   │  │  Control   │  │   Engine   │  │   Service    │   │   │
│  │  └─────┬──────┘  └─────┬──────┘  └─────┬──────┘  └──────┬───────┘   │   │
│  │        │               │               │                │            │   │
│  │        └───────────────┴───────┬───────┴────────────────┘            │   │
│  │                               ▼                                     │   │
│  │  ┌──────────────────────────────────────────────────────────────┐   │   │
│  │  │                      Event Bus                               │   │   │
│  │  │          (Publish/Subscribe for internal events)             │   │   │
│  │  └──────────────────────────────────────────────────────────────┘   │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                               │                                             │
│                               ▼                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                       Storage Layer                                  │   │
│  │                                                                       │   │
│  │  ┌─────────────────────────────────────────────────────────────┐    │   │
│  │  │                    Storage Abstraction                       │    │   │
│  │  │                 (Pluggable Backends)                         │    │   │
│  │  └─────────────────────────────────────────────────────────────┘    │   │
│  │                                                                       │   │
│  │  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌────────────┐    │   │
│  │  │  SQLite    │  │ PostgreSQL │  │   MongoDB  │  │    Redis   │    │   │
│  │  │  (Local)   │  │ (Server)   │  │ (Document) │  │  (Cache)   │    │   │
│  │  └────────────┘  └────────────┘  └────────────┘  └────────────┘    │   │
│  │                                                                       │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Component Details

```
┌─────────────────────────────────────────────────────────────────┐
│                        Core Components                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────────────┐     ┌──────────────────┐                │
│  │    Prompt        │     │    Version       │                │
│  │    Manager       │     │    Control       │                │
│  ├──────────────────┤     ├──────────────────┤                │
│  │ • CRUD operations│     │ • Git-like model │                │
│  │ • Validation     │     │ • Branching      │                │
│  │ • Templates      │     │ • Merging        │                │
│  │ • Categories     │     │ • Diff/Compare   │                │
│  │ • Tags           │     │ • Rollback       │                │
│  └────────┬─────────┘     └────────┬─────────┘                │
│           │                        │                           │
│           └────────────┬───────────┘                           │
│                        ▼                                        │
│            ┌─────────────────────┐                            │
│            │   Prompt Entity      │                            │
│            ├─────────────────────┤                            │
│            │ id: UUID            │                            │
│            │ name: str           │                            │
│            │ content: str        │                            │
│            │ version: int        │                            │
│            │ metadata: dict      │                            │
│            │ history: List[Diff]  │                            │
│            └─────────────────────┘                            │
│                                                                  │
│  ┌──────────────────┐     ┌──────────────────┐                │
│  │    Search        │     │    Sharing       │                │
│  │    Engine        │     │    Service       │                │
│  ├──────────────────┤     ├──────────────────┤                │
│  │ • Full-text      │     │ • Export/Import  │                │
│  │ • Semantic       │     │ • Team sharing   │                │
│  │ • Filters        │     │ • Permissions   │                │
│  │ • Ranking        │     │ • Sync          │                │
│  └──────────────────┘     └──────────────────┘                │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Data Flow

```
┌────────────────────────────────────────────────────────────────────────┐
│                          Data Flow Diagram                             │
├────────────────────────────────────────────────────────────────────────┤
│                                                                        │
│   User Input ──► Validation ──► Business Logic ──► Storage ──► Cache  │
│       │              │               │               │          │      │
│       ▼              ▼               ▼               ▼          ▼      │
│   ┌──────────────────────────────────────────────────────────────┐    │
│   │                      Event Processing                          │    │
│   │  • PromptCreated   • PromptUpdated   • PromptDeleted          │    │
│   │  • VersionChanged  • PromptShared   • SearchPerformed        │    │
│   └──────────────────────────────────────────────────────────────┘    │
│                                    │                                   │
│                                    ▼                                   │
│   ┌──────────────────────────────────────────────────────────────┐    │
│   │                    Notification System                         │    │
│   │  • Webhooks  • Email alerts  • In-app notifications         │    │
│   └──────────────────────────────────────────────────────────────┘    │
│                                                                        │
└────────────────────────────────────────────────────────────────────────┘
```

---

## Installation

### Prerequisites

- **Python**: 3.9 or higher
- **pip**: Latest version recommended
- **Git**: Required for version control features
- **Database**: SQLite (default), PostgreSQL, MongoDB, or Redis (optional)

### Standard Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/PromptVault.git
cd PromptVault

# Create virtual environment (recommended)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -e .

# Or with optional dependencies
pip install -e ".[dev]"        # Development dependencies
pip install -e ".[postgres]"   # PostgreSQL support
pip install -e ".[mongodb]"   # MongoDB support
pip install -e ".[all]"        # All optional dependencies
```

### Docker Installation

```bash
# Pull the official image
docker pull promptvault/promptvault:latest

# Run with default SQLite storage
docker run -d \
  --name promptvault \
  -p 8080:8080 \
  -v promptvault_data:/data \
  promptvault/promptvault:latest

# Run with PostgreSQL
docker run -d \
  --name promptvault \
  -p 8080:8080 \
  -e PROMPTVAULT_DB_TYPE=postgres \
  -e PROMPTVAULT_DB_URL=postgresql://user:pass@host:5432/promptvault \
  -v promptvault_data:/data \
  promptvault/promptvault:latest
```

### Homebrew Installation (macOS)

```bash
brew tap promptvault/tap
brew install promptvault
```

### Verify Installation

```bash
# Check CLI is installed
promptvault --version

# Check Python SDK
python -c "import promptvault; print(promptvault.__version__)"

# Run health check
promptvault doctor
```

---

## Quick Start

### 1. Initialize PromptVault

```bash
# Initialize with default settings
promptvault init

# Initialize with custom configuration
promptvault init --storage ./prompts --db-type sqlite

# Initialize with PostgreSQL
promptvault init --db-type postgres --db-url postgresql://localhost/promptvault
```

### 2. Create Your First Prompt

```bash
# Create a simple prompt
promptvault create "My First Prompt" --content "Write a haiku about coding"

# Create with metadata
promptvault create "Code Reviewer" \
  --content "Review the following code for bugs and suggest improvements..." \
  --category "development" \
  --tags "code-review,python,best-practices" \
  --description "AI assistant prompt for code review"
```

### 3. Search for Prompts

```bash
# Full-text search
promptvault search "code review"

# Semantic search
promptvault search "analyze code quality" --type semantic

# Filter by category
promptvault search "review" --category "development"

# Combined filters
promptvault search "python" --category "development" --tags "api,testing" --limit 10
```

### 4. Use a Prompt

```bash
# View prompt details
promptvault show "Code Reviewer"

# Get prompt content for use
promptvault get "Code Reviewer"

# Export for use with AI
promptvault export "Code Reviewer" --format markdown > my_prompt.md
```

### 5. Version Control

```bash
# View version history
promptvault log "Code Reviewer"

# Create a new version
promptvault edit "Code Reviewer" --content "Updated content..."
promptvault commit "Code Reviewer" -m "Improved instructions for better bug detection"

# Compare versions
promptvault diff "Code Reviewer" --from v1 --to v2

# Rollback to previous version
promptvault rollback "Code Reviewer" --to v1
```

### 6. Share Prompts

```bash
# Export to file
promptvault export "Code Reviewer" --format json --output ./shared/

# Import from file
promptvault import ./shared/code-reviewer.json

# Share with team
promptvault share "Code Reviewer" --team "dev-team" --permission read

# Publish to registry
promptvault publish "Code Reviewer" --public
```

---

## Usage Examples

### Python SDK Examples

#### Basic CRUD Operations

```python
from promptvault import PromptVault

# Initialize the client
pv = PromptVault()

# Create a new prompt
prompt = pv.prompts.create(
    name="Sentiment Analyzer",
    content="""
    Analyze the sentiment of the following text.
    Text: {text}
    
    Provide a sentiment score from -1 (very negative) to 1 (very positive)
    and a brief explanation of your analysis.
    """,
    description="Analyze text sentiment using AI",
    tags=["nlp", "sentiment", "text-analysis"],
    category="analysis"
)

print(f"Created prompt: {prompt.id}")
print(f"Version: {prompt.version}")

# Retrieve a prompt
retrieved = pv.prompts.get(prompt.id)
print(f"Content: {retrieved.content}")

# Update a prompt
retrieved.content = retrieved.content.replace(
    "-1 (very negative) to 1 (very positive)",
    "-5 (extremely negative) to 5 (extremely positive)"
)
pv.prompts.update(retrieved)

# Delete a prompt
pv.prompts.delete(prompt.id)
```

#### Search Functionality

```python
from promptvault import PromptVault

pv = PromptVault()

# Full-text search
results = pv.search.query("code review python")
for result in results:
    print(f"{result.name}: {result.score}")

# Semantic search with embeddings
results = pv.search.semantic("how to analyze programming code")
for result in results:
    print(f"{result.name}: {result.similarity}")

# Advanced search with filters
results = pv.search.query(
    "api",
    filters={
        "category": "development",
        "tags": ["python", "rest"],
        "date_range": ("2024-01-01", "2024-12-31"),
        "author": "john@example.com"
    },
    limit=20,
    offset=0
)

# Fuzzy search
results = pv.search.fuzzy("coed review", threshold=0.8)
```

#### Version Control

```python
from promptvault import PromptVault

pv = PromptVault()

# Create initial prompt
prompt = pv.prompts.create(
    name="SQL Generator",
    content="Generate a SQL query to {query_type} from {table}"
)

# Make changes and commit
prompt.content = "Generate an optimized SQL query to {operation} from {database}.{table}"
pv.prompts.update(prompt)
pv.commits.create(prompt.id, message="Support for optimized queries")

# Make more changes
prompt.content = "Generate an efficient SQL query for {operation} on {database}.{table}"
pv.prompts.update(prompt)
pv.commits.create(prompt.id, message="Improved wording for efficiency")

# View history
history = pv.commits.list(prompt.id)
for commit in history:
    print(f"{commit.version}: {commit.message} ({commit.created_at})")

# View diff between versions
diff = pv.commits.diff(prompt.id, from_version=1, to_version=2)
print(diff)

# Rollback to version 1
pv.commits.rollback(prompt.id, version=1)

# Create a branch for experimentation
branch = pv.branches.create(
    prompt_id=prompt.id,
    name="experimental-sql",
    base_branch="main",
    description="Testing new SQL generation patterns"
)

# Switch to branch
pv.branches.checkout(branch.name)

# Make experimental changes
prompt.content = "Create sophisticated SQL for {operation}"
pv.prompts.update(prompt)
pv.commits.create(prompt.id, message="Experimental changes")

# Merge branch back
pv.branches.merge("experimental-sql", into="main")
```

#### Template System

```python
from promptvault import PromptVault
from promptvault.templates import TemplateRenderer

pv = PromptVault()
renderer = TemplateRenderer()

# Create a template prompt
prompt = pv.prompts.create(
    name="Email Generator",
    content="""
    Write a professional email with the following details:
    
    Recipient: {{recipient_name}}
    Subject: {{subject}}
    Tone: {{tone|default('professional')}}
    Key Points:
    {% for point in key_points %}
    - {{point}}
    {% endfor %}
    
    Sign-off: {{sign_off|default('Best regards')}}
    """,
    template=True
)

# Render the template
context = {
    "recipient_name": "John Doe",
    "subject": "Project Update",
    "tone": "friendly",
    "key_points": [
        "Project is on track",
        "Deadline extended by 2 weeks",
        "Budget remains within limits"
    ],
    "sign_off": "Cheers"
}

rendered = renderer.render(prompt.content, context)
print(rendered)

# Use built-in template functions
rendered = renderer.render(prompt.content, {
    "recipient_name": "Jane Smith",
    "subject": "Weekly Report",
    "key_points": ["Metrics improved 15%", "New features deployed"]
})
```

#### Sharing and Collaboration

```python
from promptvault import PromptVault

pv = PromptVault()

# Export prompts
export_data = pv.prompts.export(
    prompt_ids=["uuid1", "uuid2", "uuid3"],
    format="json",
    include_history=True,
    include_metadata=True
)

# Save to file
with open("my_prompts.json", "w") as f:
    f.write(export_data)

# Import prompts
with open("shared_prompts.json", "r") as f:
    pv.prompts.import_from(f.read(), format="json")

# Share with team
pv.sharing.share(
    prompt_id="uuid1",
    team="engineering",
    permission="write",
    expires_at="2024-12-31"
)

# List shared prompts
shared = pv.sharing.list_shared(prompt_id="uuid1")
for share in shared:
    print(f"{share.team}: {share.permission}")

# Revoke access
pv.sharing.revoke(prompt_id="uuid1", team="engineering")

# Publish to public registry
pv.sharing.publish(prompt_id="uuid1", public=True, tags=["utility", "productivity"])
```

### CLI Examples

```bash
# Create prompts interactively
promptvault create --interactive

# Batch operations
promptvault batch create --file prompts.csv
promptvault batch delete --file to_delete.txt --confirm

# Organize with categories
promptvault category create "Machine Learning"
promptvault category create "Machine Learning/Deep Learning"
promptvault category move "neural-network-prompt" "Machine Learning/Deep Learning"

# Tag management
promptvault tag add "code-review" --tags "review,qa,development"
promptvault tag list --prompts "code-review"
promptvault tag cleanup --remove-unused

# Generate completion stats
promptvault stats --format json --output stats.json
promptvault stats --charts --export chart.png

# Server management
promptvault server start --port 8080 --host 0.0.0.0
promptvault server stop
promptvault server status
promptvault server logs --lines 100

# Webhook configuration
promptvault webhook add --event "prompt.created" --url https://example.com/hook
promptvault webhook list
promptvault webhook test --id webhook-uuid
```

---

## API Reference

### REST API

PromptVault provides a comprehensive REST API for programmatic access.

#### Base URL

```
http://localhost:8080/api/v1
```

#### Authentication

```bash
# Using API key
curl -H "Authorization: Bearer YOUR_API_KEY" \
     http://localhost:8080/api/v1/prompts

# Using basic auth
curl -u username:password \
     http://localhost:8080/api/v1/prompts
```

#### Endpoints

##### Prompts

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/prompts` | List all prompts |
| `POST` | `/prompts` | Create a new prompt |
| `GET` | `/prompts/{id}` | Get a specific prompt |
| `PUT` | `/prompts/{id}` | Update a prompt |
| `DELETE` | `/prompts/{id}` | Delete a prompt |
| `GET` | `/prompts/{id}/versions` | Get version history |
| `POST` | `/prompts/{id}/versions` | Create a new version |
| `GET` | `/prompts/{id}/versions/{version}` | Get specific version |

##### Search

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/search` | Search prompts |
| `POST` | `/search/semantic` | Semantic search |
| `GET` | `/search/suggestions` | Get search suggestions |

##### Sharing

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/prompts/{id}/share` | Share a prompt |
| `GET` | `/prompts/{id}/shares` | List shares |
| `DELETE` | `/shares/{share_id}` | Revoke share |
| `POST` | `/prompts/{id}/publish` | Publish to registry |

##### Categories

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/categories` | List categories |
| `POST` | `/categories` | Create category |
| `PUT` | `/categories/{id}` | Update category |
| `DELETE` | `/categories/{id}` | Delete category |

##### Tags

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/tags` | List all tags |
| `POST` | `/tags` | Create tag |
| `GET` | `/tags/{name}/prompts` | Get prompts by tag |

#### API Examples

```bash
# Create a prompt
curl -X POST http://localhost:8080/api/v1/prompts \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "name": "Code Translator",
    "content": "Translate the following {source_lang} code to {target_lang}",
    "category": "development",
    "tags": ["translation", "code"],
    "metadata": {
      "language": "multilingual",
      "complexity": "intermediate"
    }
  }'

# Search prompts
curl "http://localhost:8080/api/v1/search?q=code+review&category=development&limit=10" \
  -H "Authorization: Bearer YOUR_API_KEY"

# Semantic search
curl -X POST http://localhost:8080/api/v1/search/semantic \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "query": "analyze code quality",
    "limit": 5
  }'

# Get version history
curl http://localhost:8080/api/v1/prompts/{id}/versions \
  -H "Authorization: Bearer YOUR_API_KEY"

# Export prompts
curl "http://localhost:8080/api/v1/prompts/export?ids=id1,id2,id3&format=json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -o prompts_export.json
```

### Python SDK Reference

#### PromptVault Client

```python
class PromptVault:
    def __init__(
        self,
        storage_path: str = "~/.promptvault",
        db_type: str = "sqlite",
        db_url: Optional[str] = None,
        api_key: Optional[str] = None,
        **kwargs
    ):
        """
        Initialize PromptVault client.
        
        Args:
            storage_path: Path to local storage directory
            db_type: Database type (sqlite, postgres, mongodb, redis)
            db_url: Database connection URL
            api_key: API key for remote server authentication
        """
        pass
    
    @property
    def prompts(self) -> PromptManager:
        """Get prompt manager instance."""
        pass
    
    @property
    def search(self) -> SearchEngine:
        """Get search engine instance."""
        pass
    
    @property
    def sharing(self) -> SharingService:
        """Get sharing service instance."""
        pass
    
    @property
    def commits(self) -> VersionControl:
        """Get version control instance."""
        pass
    
    @property
    def branches(self) -> BranchManager:
        """Get branch manager instance."""
        pass
```

#### PromptManager

```python
class PromptManager:
    def create(
        self,
        name: str,
        content: str,
        description: Optional[str] = None,
        category: Optional[str] = None,
        tags: Optional[List[str]] = None,
        metadata: Optional[Dict[str, Any]] = None,
        template: bool = False
    ) -> Prompt:
        """Create a new prompt."""
        pass
    
    def get(self, prompt_id: str) -> Prompt:
        """Get a prompt by ID."""
        pass
    
    def list(
        self,
        category: Optional[str] = None,
        tags: Optional[List[str]] = None,
        limit: int = 100,
        offset: int = 0
    ) -> List[Prompt]:
        """List prompts with optional filters."""
        pass
    
    def update(self, prompt: Prompt) -> Prompt:
        """Update an existing prompt."""
        pass
    
    def delete(self, prompt_id: str, force: bool = False) -> None:
        """Delete a prompt."""
        pass
    
    def export(
        self,
        prompt_ids: List[str],
        format: str = "json",
        include_history: bool = False
    ) -> str:
        """Export prompts to specified format."""
        pass
    
    def import_from(
        self,
        data: str,
        format: str = "json",
        conflict_resolution: str = "skip"
    ) -> ImportResult:
        """Import prompts from specified format."""
        pass
```

#### Prompt Entity

```python
@dataclass
class Prompt:
    id: str                    # Unique identifier (UUID)
    name: str                  # Human-readable name
    content: str               # Prompt content
    version: int               # Current version number
    description: Optional[str] # Optional description
    category: Optional[str]    # Category path
    tags: List[str]            # List of tags
    metadata: Dict[str, Any]   # Custom metadata
    author: str                # Author identifier
    created_at: datetime       # Creation timestamp
    updated_at: datetime       # Last update timestamp
    is_template: bool          # Whether prompt is a template
    is_public: bool           # Public visibility flag
    usage_count: int           # Number of times used
    
    def to_dict(self) -> Dict[str, Any]:
        """Convert to dictionary."""
        pass
    
    @classmethod
    def from_dict(cls, data: Dict[str, Any]) -> "Prompt":
        """Create from dictionary."""
        pass
```

#### SearchEngine

```python
class SearchEngine:
    def query(
        self,
        text: str,
        filters: Optional[SearchFilters] = None,
        limit: int = 20,
        offset: int = 0
    ) -> SearchResults:
        """Full-text search with filters."""
        pass
    
    def semantic(
        self,
        query: str,
        limit: int = 10,
        threshold: float = 0.7
    ) -> SemanticResults:
        """Semantic search using embeddings."""
        pass
    
    def fuzzy(
        self,
        text: str,
        threshold: float = 0.8
    ) -> FuzzyResults:
        """Fuzzy matching search."""
        pass
    
    def suggest(self, prefix: str) -> List[str]:
        """Get search suggestions."""
        pass
```

#### VersionControl

```python
class VersionControl:
    def list(self, prompt_id: str) -> List[Commit]:
        """List all commits for a prompt."""
        pass
    
    def create(
        self,
        prompt_id: str,
        message: str,
        author: Optional[str] = None
    ) -> Commit:
        """Create a new commit (version)."""
        pass
    
    def get(self, prompt_id: str, version: int) -> Prompt:
        """Get a specific version of a prompt."""
        pass
    
    def diff(
        self,
        prompt_id: str,
        from_version: int,
        to_version: int
    ) -> Diff:
        """Get diff between two versions."""
        pass
    
    def rollback(self, prompt_id: str, version: int) -> Prompt:
        """Rollback to a specific version."""
        pass
    
    def merge(
        self,
        prompt_id: str,
        source_version: int,
        target_version: int,
        strategy: str = "auto"
    ) -> MergeResult:
        """Merge two versions."""
        pass
```

---

## Configuration

### Configuration File

PromptVault uses a YAML configuration file located at:

- `~/.promptvault/config.yaml` (user-level)
- `./promptvault.yaml` (project-level)
- `./.promptvault.yaml` (alternative project location)

### Configuration Options

```yaml
# PromptVault Configuration File
# =========================================

# General Settings
general:
  # Application name
  name: "PromptVault"
  
  # Debug mode (default: false)
  debug: false
  
  # Log level: DEBUG, INFO, WARNING, ERROR, CRITICAL
  log_level: "INFO"
  
  # Log file path
  log_file: "~/.promptvault/logs/promptvault.log"
  
  # Enable telemetry (default: true)
  telemetry: true

# Storage Configuration
storage:
  # Storage type: sqlite, postgres, mongodb, redis
  type: "sqlite"
  
  # Database connection URL
  url: "~/.promptvault/data/promptvault.db"
  
  # Connection pool settings
  pool_size: 10
  max_overflow: 20
  
  # Backup settings
  auto_backup: true
  backup_interval: 3600  # seconds
  backup_retention: 7   # days
  
  # Encryption
  encryption:
    enabled: false
    key_file: "~/.promptvault/.encryption_key"

# Search Configuration
search:
  # Search backend: basic, elasticsearch, meilisearch
  backend: "basic"
  
  # Elasticsearch configuration
  elasticsearch:
    hosts:
      - "http://localhost:9200"
    index_prefix: "promptvault"
    timeout: 30
  
  # Meilisearch configuration
  meilisearch:
    url: "http://localhost:7700"
    api_key: "your_api_key"
    index: "prompts"
  
  # Semantic search
  semantic:
    enabled: true
    model: "sentence-transformers/all-MiniLM-L6-v2"
    device: "auto"  # cpu, cuda, auto
    batch_size: 32
  
  # Search settings
  settings:
    fuzzy_threshold: 0.8
    max_results: 100
    highlight_enabled: true
    snippet_length: 150

# Server Configuration
server:
  # Enable REST API server
  enabled: false
  
  # Server host
  host: "0.0.0.0"
  
  # Server port
  port: 8080
  
  # Number of workers
  workers: 4
  
  # Enable CORS
  cors:
    enabled: true
    origins: ["*"]
  
  # Authentication
  auth:
    enabled: true
    type: "api_key"  # api_key, jwt, oauth
    jwt:
      secret: "your-secret-key"
      algorithm: "HS256"
      expiration: 3600

# Sharing Configuration
sharing:
  # Export formats
  export_formats:
    - json
    - yaml
    - markdown
    - csv
  
  # Default export settings
  export:
    include_metadata: true
    include_history: false
    include_comments: true
  
  # Public registry
  registry:
    url: "https://registry.promptvault.io"
    api_key: ""
  
  # Webhooks
  webhooks:
    enabled: true
    retries: 3
    timeout: 10

# Version Control Configuration
versioning:
  # Maximum versions to keep per prompt (0 = unlimited)
  max_versions: 100
  
  # Auto-commit on every change
  auto_commit: false
  
  # Commit message template
  commit_template: "{author} updated {prompt_name} at {timestamp}"
  
  # Branch settings
  branches:
    default: "main"
    allowed: ["main", "develop", "feature/*"]
    auto_delete: false

# Templates Configuration
templates:
  # Template engine: jinja2, mako, cheetah
  engine: "jinja2"
  
  # Auto-escape HTML
  auto_escape: false
  
  # Custom filters
  custom_filters:
    - "promptvault.templates.filters:all_filters"
  
  # Variable validation
  strict_variables: true
  undefined: "strict"  # strict, ignore, debug

# CLI Configuration
cli:
  # Default output format: table, json, yaml, plain
  output_format: "table"
  
  # Color output
  colors: true
  
  # Editor for interactive editing
  editor: "vim"  # vim, nano, code, emacs
  
  # Pager for long output
  pager: "less"
  
  # Auto-confirm dangerous operations
  auto_confirm: false

# Categories Configuration
categories:
  # Allowed category names (regex)
  pattern: "^[a-zA-Z0-9/_-]+$"
  
  # Maximum depth
  max_depth: 5
  
  # Auto-create parent categories
  auto_create: true

# Tags Configuration
tags:
  # Maximum tags per prompt
  max_per_prompt: 20
  
  # Allowed tag pattern
  pattern: "^[a-zA-Z0-9_-]+$"
  
  # Case sensitive
  case_sensitive: false
  
  # Auto-suggest similar tags
  suggest_similar: true

# Analytics Configuration
analytics:
  # Track prompt usage
  enabled: true
  
  # Track search queries
  track_searches: true
  
  # Track performance metrics
  track_performance: true
  
  # Retention period
  retention_days: 90
```

### Environment Variables

All configuration options can be set via environment variables:

```bash
# General
export PROMPTVAULT_DEBUG=true
export PROMPTVAULT_LOG_LEVEL=DEBUG

# Storage
export PROMPTVAULT_DB_TYPE=postgres
export PROMPTVAULT_DB_URL=postgresql://user:pass@host:5432/promptvault

# Search
export PROMPTVAULT_SEARCH_BACKEND=elasticsearch
export PROMPTVAULT_ES_HOST=http://localhost:9200

# Server
export PROMPTVAULT_SERVER_ENABLED=true
export PROMPTVAULT_SERVER_PORT=8080
export PROMPTVAULT_AUTH_API_KEY=your_api_key

# Paths
export PROMPTVAULT_HOME=~/.promptvault
export PROMPTVAULT_CONFIG=./promptvault.yaml
```

---

## Prompt Templates

PromptVault supports parameterized templates using Jinja2 syntax.

### Template Variables

```jinja2
# Basic variable
{{ variable_name }}

# With default value
{{ variable_name | default("default_value") }}

# Required variable (raises error if missing)
{{! required_variable }}

# Conditional default
{{ variable_name | default(another_var, "fallback") }}
```

### Control Structures

```jinja2
# Loops
{% for item in items %}
- {{ item }}
{% endfor %}

# Conditionals
{% if condition %}
True branch
{% elif other_condition %}
Else if branch
{% else %}
False branch
{% endif %}

# Filters
{{ text | upper }}
{{ text | lower }}
{{ text | capitalize }}
{{ text | trim }}

# Number filters
{{ number | round(2) }}
{{ number | abs }}

# List filters
{{ list | join(", ") }}
{{ list | length }}
{{ list | sort }}
{{ list | unique }}
```

### Built-in Template Filters

| Filter | Description | Example |
|--------|-------------|---------|
| `default` | Provide default value | `{{ x \| default("N/A") }}` |
| `upper` | Convert to uppercase | `{{ name \| upper }}` |
| `lower` | Convert to lowercase | `{{ name \| lower }}` |
| `capitalize` | Capitalize first letter | `{{ name \| capitalize }}` |
| `trim` | Remove whitespace | `{{ text \| trim }}` |
| `join` | Join list elements | `{{ items \| join(", ") }}` |
| `length` | Get length | `{{ items \| length }}` |
| `sort` | Sort list | `{{ items \| sort }}` |
| `reverse` | Reverse list | `{{ items \| reverse }}` |
| `unique` | Remove duplicates | `{{ items \| unique }}` |
| `first` | Get first element | `{{ items \| first }}` |
| `last` | Get last element | `{{ items \| last }}` |
| `batch` | Split into groups | `{{ items \| batch(3) }}` |
| `tojson` | Convert to JSON | `{{ data \| tojson }}` |

### Example Templates

#### Code Review Template

```jinja2
# Code Review Prompt

## Repository
{{ repository_name }}

## Branch
{{ branch_name }}

## Changes Summary
{% if changes_summary %}
{{ changes_summary }}
{% else %}
{{! Provide a summary of the changes }}
{% endif %}

## Files Changed
{% for file in changed_files %}
- `{{ file.path }}` ({{ file.additions }}+/{{ file.deletions }}-)
{% endfor %}

## Review Focus Areas
{% for area in focus_areas %}
- {{ area }}
{% endfor %}

## Additional Context
{{ additional_context | default("No additional context provided.") }}

## Instructions
Please review the code changes for:
1. Potential bugs or security vulnerabilities
2. Code quality and style consistency
3. Performance implications
4. Test coverage
5. Documentation accuracy

Provide your feedback in the following format:
```
## Issues Found
[List any issues with severity (Critical/High/Medium/Low)]

## Suggestions
[Any improvement suggestions]

## Approval Status
[Approved/Changes Requested/Blocked]
```
```

#### Meeting Summary Template

```jinja2
# Meeting Summary: {{ meeting_title }}

**Date:** {{ date }}
**Time:** {{ start_time }} - {{ end_time }}
**Attendees:** {{ attendees | join(", ") }}

## Agenda
{% for item in agenda %}
{{ loop.index }}. {{ item }}
{% endfor %}

## Discussion Points
{% for point in discussion %}
### {{ point.topic }}
{{ point.notes }}

{% endfor %}

## Decisions Made
{% for decision in decisions %}
- {{ decision }}
{% endfor %}

## Action Items
| Task | Owner | Due Date |
|------|-------|----------|
{% for item in action_items %}
| {{ item.task }} | {{ item.owner }} | {{ item.due_date }} |
{% endfor %}

## Next Meeting
**Date:** {{ next_meeting_date | default("TBD") }}
**Topics:** {{ next_topics | join(", ") | default("TBD") }}
```

---

## CLI Reference

### Global Options

```bash
promptvault [OPTIONS] COMMAND [ARGS]...
```

| Option | Description |
|--------|-------------|
| `--config PATH` | Custom config file path |
| `--debug` | Enable debug mode |
| `--quiet` | Suppress output |
| `--output FORMAT` | Output format (table, json, yaml) |
| `--no-color` | Disable colored output |
| `--version` | Show version |
| `--help` | Show help |

### Commands

#### `init`

```bash
promptvault init [OPTIONS]
```

Initialize PromptVault.

| Option | Description |
|--------|-------------|
| `--storage PATH` | Storage directory path |
| `--db-type TYPE` | Database type (sqlite, postgres, mongodb) |
| `--db-url URL` | Database connection URL |
| `--force` | Overwrite existing configuration |

#### `create`

```bash
promptvault create NAME [OPTIONS]
```

Create a new prompt.

| Option | Description |
|--------|-------------|
| `--content TEXT` | Prompt content |
| `--file PATH` | Read content from file |
| `--description TEXT` | Prompt description |
| `--category TEXT` | Category path |
| `--tags TEXT` | Comma-separated tags |
| `--template` | Mark as template |
| `--interactive`, `-i` | Interactive creation mode |

#### `get`

```bash
promptvault get PROMPT_ID_OR_NAME [OPTIONS]
```

Get a prompt by ID or name.

| Option | Description |
|--------|-------------|
| `--version INT` | Get specific version |
| `--raw` | Output raw content only |
| `--format FORMAT` | Output format (text, json, yaml) |

#### `list`

```bash
promptvault list [OPTIONS]
```

List prompts.

| Option | Description |
|--------|-------------|
| `--category TEXT` | Filter by category |
| `--tag TEXT` | Filter by tag |
| `--author TEXT` | Filter by author |
| `--limit INT` | Maximum results (default: 50) |
| `--offset INT` | Results offset |
| `--sort FIELD` | Sort field (name, created, updated) |
| `--order ORDER` | Sort order (asc, desc) |

#### `search`

```bash
promptvault search QUERY [OPTIONS]
```

Search prompts.

| Option | Description |
|--------|-------------|
| `--type TYPE` | Search type (full, semantic, fuzzy) |
| `--category TEXT` | Filter by category |
| `--tags TEXT` | Filter by tags |
| `--limit INT` | Maximum results |
| `--highlight` | Show highlights |

#### `edit`

```bash
promptvault edit PROMPT_ID_OR_NAME [OPTIONS]
```

Edit a prompt.

| Option | Description |
|--------|-------------|
| `--content TEXT` | New content |
| `--name TEXT` | New name |
| `--description TEXT` | New description |
| `--category TEXT` | New category |
| `--tags TEXT` | New tags |
| `--editor` | Use editor |

#### `delete`

```bash
promptvault delete PROMPT_ID_OR_NAME [OPTIONS]
```

Delete a prompt.

| Option | Description |
|--------|-------------|
| `--force` | Skip confirmation |
| `--version INT` | Delete specific version only |

#### `log`

```bash
promptvault log PROMPT_ID_OR_NAME [OPTIONS]
```

View prompt version history.

| Option | Description |
|--------|-------------|
| `--limit INT` | Number of commits |
| `--format FORMAT` | Output format |

#### `commit`

```bash
promptvault commit PROMPT_ID_OR_NAME [OPTIONS]
```

Create a new version.

| Option | Description |
|--------|-------------|
| `--message TEXT` | Commit message |
| `--author TEXT` | Author name |

#### `diff`

```bash
promptvault diff PROMPT_ID_OR_NAME [OPTIONS]
```

Show differences between versions.

| Option | Description |
|--------|-------------|
| `--from VERSION` | From version |
| `--to VERSION` | To version |
| `--format FORMAT` | Diff format (unified, side-by-side) |

#### `rollback`

```bash
promptvault rollback PROMPT_ID_OR_NAME [OPTIONS]
```

Rollback to a previous version.

| Option | Description |
|--------|-------------|
| `--to VERSION` | Target version |

#### `export`

```bash
promptvault export [PROMPT_ID_OR_NAME] [OPTIONS]
```

Export prompts.

| Option | Description |
|--------|-------------|
| `--output PATH` | Output directory/file |
| `--format FORMAT` | Export format (json, yaml, markdown) |
| `--include-history` | Include version history |
| `--include-metadata` | Include metadata |

#### `import`

```bash
promptvault import PATH [OPTIONS]
```

Import prompts.

| Option | Description |
|--------|-------------|
| `--format FORMAT` | Import format (auto, json, yaml) |
| `--conflict MODE` | Conflict resolution (skip, overwrite, fail) |

#### `share`

```bash
promptvault share PROMPT_ID_OR_NAME [OPTIONS]
```

Share a prompt.

| Option | Description |
|--------|-------------|
| `--team TEXT` | Team name |
| `--user TEXT` | User email |
| `--permission PERM` | Permission (read, write, admin) |
| `--expires TEXT` | Expiration date |

#### `server`

```bash
promptvault server [COMMAND] [OPTIONS]
```

Manage the API server.

| Subcommand | Description |
|------------|-------------|
| `start` | Start the server |
| `stop` | Stop the server |
| `restart` | Restart the server |
| `status` | Show server status |
| `logs` | Show server logs |

---

## Troubleshooting

### Common Issues

#### Installation Issues

**Problem**: `pip install` fails with dependency conflicts

```bash
# Solution: Create a clean virtual environment
python -m venv promptvault_env
source promptvault_env/bin/activate
pip install --upgrade pip
pip install promptvault

# Or use pip-tools for better dependency resolution
pip install pip-tools
pip-compile requirements.in
pip-sync requirements.txt
```

**Problem**: `command not found: promptvault` after installation

```bash
# Solution: Check if pip installed in user site-packages
python -m site --user-base
export PATH="$PATH:$(python -m site --user-base)/bin"

# Or reinstall with --user flag
pip install --user promptvault

# Or use python -m promptvault
python -m promptvault --version
```

#### Storage Issues

**Problem**: Database locked error

```bash
# Solution 1: Check for other running instances
promptvault server stop
# or
pkill -f promptvault

# Solution 2: For SQLite, check file permissions
chmod 644 ~/.promptvault/data/promptvault.db

# Solution 3: Use WAL mode for better concurrency
promptvault config set storage.sqlite.journal_mode=WAL
```

**Problem**: Database corruption

```bash
# Solution: Restore from backup
ls ~/.promptvault/backups/
promptvault restore --backup 2024-01-15_120000.db
```

#### Search Issues

**Problem**: Search returns no results

```bash
# Solution 1: Rebuild search index
promptvault search --rebuild

# Solution 2: Check search configuration
promptvault config show search

# Solution 3: Verify content exists
promptvault list --limit 10
```

**Problem**: Semantic search is slow

```bash
# Solution 1: Use GPU for embeddings
promptvault config set search.semantic.device=cuda

# Solution 2: Reduce batch size
promptvault config set search.semantic.batch_size=16

# Solution 3: Use smaller model
promptvault config set search.semantic.model=sentence-transformers/paraphrase-MiniLM-L3-v2
```

#### Version Control Issues

**Problem**: Cannot merge branches

```bash
# Solution 1: Check for conflicts
promptvault merge --check source --target main

# Solution 2: Use manual merge
promptvault merge --strategy manual source --target main

# Solution 3: Force merge (discards conflicts)
promptvault merge --force source --target main
```

**Problem**: Lost commits after rollback

```bash
# Solution: Commits are not deleted, just create new version
promptvault log --all --prompt your_prompt
promptvault create-version --from-commit abc123 --message "Restore"
```

#### Server Issues

**Problem**: Server won't start

```bash
# Solution 1: Check port availability
lsof -i :8080
promptvault server start --port 8081

# Solution 2: Check logs
promptvault server logs --lines 50

# Solution 3: Verify configuration
promptvault config validate
```

**Problem**: API returns 401 Unauthorized

```bash
# Solution: Generate new API key
promptvault api-key generate
# Update your config or use the new key
```

### Debug Mode

Enable debug mode for detailed logging:

```bash
# CLI debug mode
promptvault --debug [command]

# Enable in config
promptvault config set general.debug true

# View debug logs
promptvault logs --level debug
```

### Performance Optimization

```bash
# Enable caching
promptvault config set search.cache enabled=true

# Optimize database
promptvault db optimize

# Vacuum storage
promptvault db vacuum

# Check disk usage
promptvault info --storage-stats
```

### Getting Help

```bash
# Show help
promptvault --help
promptvault [command] --help

# Check system status
promptvault doctor

# View configuration
promptvault config show

# Check version
promptvault --version
```

---

## Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

### Development Setup

```bash
# Fork and clone the repository
git clone https://github.com/yourusername/PromptVault.git
cd PromptVault

# Create a feature branch
git checkout -b feature/amazing-feature

# Install development dependencies
pip install -e ".[dev]"

# Run tests
pytest tests/ -v

# Run linting
flake8 src/
black src/
mypy src/
```

### Pull Request Process

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

<p align="center">
  <strong>PromptVault</strong> — Version control for your AI prompts
  
  Made with ❤️ for the AI engineering community
</p>
