# Google ADK Implementation Guide

**Agent Development Kit (ADK) - Technical Reference for ideas-to-cheatsheet**

This guide provides concrete implementation details for building agentic AI features using Google's Agent Development Kit (ADK). For project philosophy and architecture vision, see [CLAUDE.md](CLAUDE.md).

---

## Table of Contents

1. [Quick Start](#1-quick-start)
2. [Installation with uv](#2-installation-with-uv)
3. [Core Concepts](#3-core-concepts)
4. [Agent Patterns for This Project](#4-agent-patterns-for-this-project)
5. [Multi-Agent Orchestration](#5-multi-agent-orchestration)
6. [Running Agents Locally](#6-running-agents-locally)
7. [Tool Definitions](#7-tool-definitions)
8. [Testing Agents](#8-testing-agents)
9. [Environment Configuration](#9-environment-configuration)
10. [Deployment Options](#10-deployment-options)
11. [Troubleshooting](#11-troubleshooting)
12. [Resources](#12-resources)

---

## 1. Quick Start

Get your first ADK agent running in 5 minutes:

```bash
# Install uv (if not already installed)
curl -LsSf https://astral.sh/uv/install.sh | sh

# Create virtual environment
uv venv

# Activate environment
source .venv/bin/activate

# Install Google ADK
uv pip install google-adk

# Set up your Google API key
export GOOGLE_API_KEY="your_api_key_here"

# Create a simple agent (see examples below)
# Run it
adk run agents/intent_agent
```

---

## 2. Installation with uv

### 2.1 System Requirements

- Python 3.10 or later
- uv (preferred package manager)
- Google Cloud API key or project

### 2.2 Step-by-Step Setup

```bash
# 1. Create project directory structure
mkdir -p agents/intent_agent
mkdir -p agents/structure_agent
mkdir -p agents/content_agent
mkdir -p agents/refinement_agent
mkdir -p core
mkdir -p tests

# 2. Create and activate virtual environment
uv venv
source .venv/bin/activate  # macOS/Linux
# .venv\Scripts\activate    # Windows

# 3. Install Google ADK (stable release)
uv pip install google-adk

# 4. Install additional dependencies
uv pip install pydantic pytest python-dotenv

# 5. Create requirements.txt
uv pip freeze > requirements.txt
```

### 2.3 Version Management

```bash
# Install specific version
uv pip install google-adk==1.0.0

# Install development version (latest from GitHub)
uv pip install git+https://github.com/google/adk-python.git@main

# Update to latest stable
uv pip install --upgrade google-adk
```

**Note:** Google ADK has a roughly bi-weekly release cadence. Check [releases](https://github.com/google/adk-python/releases) regularly.

---

## 3. Core Concepts

### 3.1 What is an Agent?

An ADK agent is a Python object that:
- Has a name and description
- Uses an LLM model (typically Gemini)
- Can use tools (functions)
- Can coordinate with other agents (sub-agents)

### 3.2 Key Components

```python
from google.adk.agents import Agent

root_agent = Agent(
    name="agent_name",              # Unique identifier
    model="gemini-2.5-flash",       # LLM model to use
    instruction="What the agent does",  # System prompt
    description="Agent purpose",     # For coordination
    tools=[function1, function2]     # Available tools
)
```

### 3.3 Agent Types

**LlmAgent** - General-purpose agent with LLM reasoning
```python
from google.adk.agents import LlmAgent

agent = LlmAgent(
    name="my_agent",
    model="gemini-2.5-flash",
    instruction="Your instructions here",
    sub_agents=[other_agent]  # Can coordinate other agents
)
```

**Agent** - Base agent class (simpler, more direct)
```python
from google.adk.agents import Agent

agent = Agent(
    name="simple_agent",
    model="gemini-2.5-flash",
    description="What this agent does",
    tools=[my_tool]
)
```

### 3.4 Models Available

- `gemini-2.5-flash` - Fast, cost-effective (recommended for most tasks)
- `gemini-2.5-pro` - More capable, higher quality
- `gemini-3-flash-preview` - Latest experimental features

**For this project:** Use `gemini-2.5-flash` for fast iteration, `gemini-2.5-pro` for final quality.

---

## 4. Agent Patterns for This Project

### 4.1 Intent Agent

**Purpose:** Extract user intent from raw prompt

```python
# agents/intent_agent/agent.py
from google.adk.agents import Agent
from pydantic import BaseModel

class IntentOutput(BaseModel):
    topic: str
    audience: str  # beginner, intermediate, advanced
    depth: str     # quick, detailed, comprehensive
    format: str    # cheat sheet, tutorial, reference

def extract_intent(user_prompt: str) -> dict:
    """
    Analyze user prompt to extract intent.

    Args:
        user_prompt: Raw user input

    Returns:
        Structured intent data
    """
    # This is a placeholder - ADK agent will handle the logic
    return {
        "topic": "",
        "audience": "",
        "depth": "",
        "format": "cheat sheet"
    }

root_agent = Agent(
    name="intent_agent",
    model="gemini-2.5-flash",
    instruction="""
    Analyze the user's prompt and extract:
    - Topic: What subject they want to learn about
    - Audience: Their skill level (beginner/intermediate/advanced)
    - Depth: How detailed they want it (quick/detailed/comprehensive)
    - Format: What output format they prefer (default: cheat sheet)

    Be concise and accurate. If unclear, make reasonable assumptions.
    """,
    description="Extracts and clarifies user intent from prompts.",
    tools=[extract_intent]
)
```

### 4.2 Structure Agent

**Purpose:** Create outline and hierarchy for cheat sheet

```python
# agents/structure_agent/agent.py
from google.adk.agents import Agent
from typing import List
from pydantic import BaseModel

class Section(BaseModel):
    title: str
    order: int
    subsections: List[str] = []

class CheatSheetStructure(BaseModel):
    title: str
    sections: List[Section]

def create_outline(topic: str, audience: str, depth: str) -> dict:
    """
    Generate cheat sheet outline based on intent.

    Args:
        topic: Subject matter
        audience: Skill level
        depth: Level of detail

    Returns:
        Structured outline
    """
    return {
        "title": "",
        "sections": []
    }

root_agent = Agent(
    name="structure_agent",
    model="gemini-2.5-flash",
    instruction="""
    Create a logical, hierarchical outline for a cheat sheet on the given topic.

    Guidelines:
    - Start with fundamentals, progress to advanced
    - 5-8 main sections maximum
    - Each section should have 3-5 subsections
    - Use clear, descriptive titles
    - Ensure logical flow and progression
    - Adapt complexity to audience level

    Return a structured outline with titles and order.
    """,
    description="Generates logical cheat sheet structure and hierarchy.",
    tools=[create_outline]
)
```

### 4.3 Content Agent

**Purpose:** Generate actual content for each section

```python
# agents/content_agent/agent.py
from google.adk.agents import Agent
from pydantic import BaseModel

class SectionContent(BaseModel):
    title: str
    content: str
    examples: List[str] = []
    code_snippets: List[str] = []

def generate_section_content(
    section_title: str,
    topic: str,
    audience: str
) -> dict:
    """
    Generate content for a specific section.

    Args:
        section_title: Title of the section to generate
        topic: Overall topic
        audience: Target audience level

    Returns:
        Section content with examples
    """
    return {
        "title": section_title,
        "content": "",
        "examples": [],
        "code_snippets": []
    }

root_agent = Agent(
    name="content_agent",
    model="gemini-2.5-flash",
    instruction="""
    Generate clear, concise content for cheat sheet sections.

    Guidelines:
    - Use simple, direct language
    - Provide practical examples
    - Include code snippets where relevant
    - Focus on actionable information
    - Avoid unnecessary jargon
    - Make it scannable and easy to reference

    Adapt language complexity to the audience level.
    """,
    description="Generates clear, concise content for cheat sheet sections.",
    tools=[generate_section_content]
)
```

### 4.4 Refinement Agent

**Purpose:** Polish content for clarity and consistency

```python
# agents/refinement_agent/agent.py
from google.adk.agents import Agent

def refine_content(content: str, tone: str = "friendly") -> dict:
    """
    Refine and polish content for clarity.

    Args:
        content: Raw content to refine
        tone: Desired tone (friendly, professional, technical)

    Returns:
        Refined content
    """
    return {
        "refined_content": "",
        "improvements_made": []
    }

root_agent = Agent(
    name="refinement_agent",
    model="gemini-2.5-flash",
    instruction="""
    Review and refine cheat sheet content for:
    - Clarity and conciseness
    - Consistent terminology
    - Proper formatting
    - Removal of redundancy
    - Tone consistency
    - Grammar and spelling

    Maintain the original meaning while improving presentation.
    """,
    description="Polishes content for clarity and consistency.",
    tools=[refine_content]
)
```

---

## 5. Multi-Agent Orchestration

### 5.1 Coordinator Pattern

Create a coordinator agent that orchestrates the pipeline:

```python
# core/orchestrator.py
from google.adk.agents import LlmAgent

# Import individual agents
from agents.intent_agent.agent import root_agent as intent_agent
from agents.structure_agent.agent import root_agent as structure_agent
from agents.content_agent.agent import root_agent as content_agent
from agents.refinement_agent.agent import root_agent as refinement_agent

coordinator = LlmAgent(
    name="CheatSheetCoordinator",
    model="gemini-2.5-flash",
    instruction="""
    You coordinate the cheat sheet generation pipeline:

    1. Use intent_agent to understand user request
    2. Use structure_agent to create outline
    3. Use content_agent to generate content for each section
    4. Use refinement_agent to polish the final output

    Ensure each step completes before moving to the next.
    Pass relevant context between agents.
    """,
    description="Coordinates the full cheat sheet generation pipeline.",
    sub_agents=[intent_agent, structure_agent, content_agent, refinement_agent]
)

# Make coordinator the root agent
root_agent = coordinator
```

### 5.2 Sequential Pipeline

Alternative approach with explicit sequencing:

```python
# core/pipeline.py
from typing import Dict, Any

async def generate_cheatsheet(user_prompt: str) -> Dict[str, Any]:
    """
    Execute the full cheat sheet generation pipeline.

    Args:
        user_prompt: User's initial request

    Returns:
        Complete cheat sheet data
    """
    # Step 1: Extract intent
    intent = await intent_agent.process(user_prompt)

    # Step 2: Create structure
    structure = await structure_agent.process({
        "topic": intent["topic"],
        "audience": intent["audience"],
        "depth": intent["depth"]
    })

    # Step 3: Generate content for each section
    sections = []
    for section in structure["sections"]:
        content = await content_agent.process({
            "section_title": section["title"],
            "topic": intent["topic"],
            "audience": intent["audience"]
        })
        sections.append(content)

    # Step 4: Refine complete cheat sheet
    complete_content = {
        "title": structure["title"],
        "sections": sections
    }
    refined = await refinement_agent.process(complete_content)

    return refined
```

---

## 6. Running Agents Locally

### 6.1 CLI Mode

Run agents from the command line:

```bash
# Run a specific agent
adk run agents/intent_agent

# Run with input
adk run agents/intent_agent --input "Explain REST APIs for beginners"

# Run the coordinator
adk run core/orchestrator
```

### 6.2 Web UI Mode

Launch the interactive development interface:

```bash
# Start web UI on default port (8000)
adk web

# Specify custom port
adk web --port 3000

# Specify agent directory
adk web --agent-dir agents/intent_agent
```

Then open http://localhost:8000 in your browser.

**Web UI Features:**
- Interactive chat interface
- Tool execution visibility
- Session history
- Agent switching
- Debug logs

**Note:** Web UI is for development only, not production.

### 6.3 Python Script Mode

Run agents directly in Python:

```python
# test_agent.py
import asyncio
from agents.intent_agent.agent import root_agent

async def test_intent_agent():
    result = await root_agent.run(
        "Create a cheat sheet for Python decorators"
    )
    print(result)

if __name__ == "__main__":
    asyncio.run(test_intent_agent())
```

Run with:
```bash
python test_agent.py
```

---

## 7. Tool Definitions

### 7.1 Function Tools

Python functions become tools automatically:

```python
def calculate_complexity(topic: str, audience: str) -> dict:
    """
    Calculate appropriate complexity level.

    Args:
        topic: Subject matter
        audience: Target audience (beginner/intermediate/advanced)

    Returns:
        Dictionary with complexity metrics
    """
    complexity_map = {
        "beginner": {"depth": 1, "jargon": "minimal"},
        "intermediate": {"depth": 2, "jargon": "moderate"},
        "advanced": {"depth": 3, "jargon": "technical"}
    }
    return complexity_map.get(audience, complexity_map["beginner"])

# Add to agent
agent = Agent(
    name="my_agent",
    model="gemini-2.5-flash",
    tools=[calculate_complexity]  # Function becomes a tool
)
```

### 7.2 Tool Guidelines

**Good Tool Design:**
- Clear, descriptive function name
- Comprehensive docstring
- Type hints for all parameters
- Returns structured data (dict, Pydantic model)
- Single responsibility
- Handles errors gracefully

**Example:**
```python
from typing import Optional

def format_code_snippet(
    code: str,
    language: str,
    include_comments: bool = True
) -> dict:
    """
    Format code snippet for display in cheat sheet.

    Args:
        code: Raw code string
        language: Programming language (python, javascript, etc.)
        include_comments: Whether to include inline comments

    Returns:
        Formatted code with metadata

    Raises:
        ValueError: If language is not supported
    """
    supported_languages = ["python", "javascript", "java", "go"]
    if language not in supported_languages:
        raise ValueError(f"Unsupported language: {language}")

    # Formatting logic here
    return {
        "formatted_code": code,
        "language": language,
        "line_count": len(code.split("\n"))
    }
```

### 7.3 Built-in Tools

ADK provides pre-built tools:

```python
from google.adk.tools import google_search

agent = Agent(
    name="research_agent",
    model="gemini-2.5-flash",
    tools=[google_search]  # Built-in web search
)
```

**Available built-in tools:**
- `google_search` - Web search capability
- More tools in development

---

## 8. Testing Agents

### 8.1 Unit Testing

```python
# tests/test_intent_agent.py
import pytest
from agents.intent_agent.agent import root_agent

@pytest.mark.asyncio
async def test_intent_extraction_basic():
    """Test basic intent extraction."""
    result = await root_agent.run(
        "I need a Python cheat sheet for beginners"
    )

    assert "python" in result.lower()
    assert "beginner" in result.lower()

@pytest.mark.asyncio
async def test_intent_extraction_advanced():
    """Test advanced intent extraction."""
    result = await root_agent.run(
        "Create a comprehensive REST API guide for experienced developers"
    )

    assert "rest" in result.lower() or "api" in result.lower()
    assert "advanced" in result.lower() or "experienced" in result.lower()
```

### 8.2 Integration Testing

```python
# tests/test_pipeline.py
import pytest
from core.pipeline import generate_cheatsheet

@pytest.mark.asyncio
async def test_full_pipeline():
    """Test the complete cheat sheet generation pipeline."""
    result = await generate_cheatsheet(
        "Create a Git commands cheat sheet"
    )

    assert result is not None
    assert "title" in result
    assert "sections" in result
    assert len(result["sections"]) > 0

@pytest.mark.asyncio
async def test_pipeline_with_complex_request():
    """Test pipeline with complex, multi-part request."""
    result = await generate_cheatsheet(
        "I need a comprehensive guide to Python async/await "
        "for intermediate developers with practical examples"
    )

    assert result["sections"]
    # Check that content is appropriate for intermediate level
    content_text = str(result).lower()
    assert "async" in content_text
    assert "await" in content_text
```

### 8.3 Running Tests

```bash
# Run all tests
pytest

# Run specific test file
pytest tests/test_intent_agent.py

# Run with coverage
pytest --cov=agents --cov=core tests/

# Run in verbose mode
pytest -v

# Run with output
pytest -s
```

---

## 9. Environment Configuration

### 9.1 Environment Variables

Create a `.env` file in your project root:

```bash
# .env

# Google Cloud Configuration
GOOGLE_API_KEY=your_google_api_key_here
GOOGLE_PROJECT_ID=your_project_id

# Model Configuration
DEFAULT_MODEL=gemini-2.5-flash
FALLBACK_MODEL=gemini-2.5-pro

# Agent Configuration
MAX_RETRIES=3
TIMEOUT_SECONDS=30

# Development Settings
DEBUG=true
LOG_LEVEL=INFO
```

### 9.2 Loading Environment Variables

```python
# core/config.py
from dotenv import load_dotenv
import os

# Load environment variables
load_dotenv()

class Config:
    GOOGLE_API_KEY = os.getenv("GOOGLE_API_KEY")
    GOOGLE_PROJECT_ID = os.getenv("GOOGLE_PROJECT_ID")
    DEFAULT_MODEL = os.getenv("DEFAULT_MODEL", "gemini-2.5-flash")
    DEBUG = os.getenv("DEBUG", "false").lower() == "true"

    @classmethod
    def validate(cls):
        """Validate required configuration."""
        if not cls.GOOGLE_API_KEY:
            raise ValueError("GOOGLE_API_KEY not set in environment")

        return True

# Validate on import
Config.validate()
```

### 9.3 Agent-Specific Configuration

```python
# agents/intent_agent/.env
AGENT_NAME=intent_agent
MODEL=gemini-2.5-flash
TEMPERATURE=0.7
MAX_TOKENS=1000
```

### 9.4 Security Best Practices

**Never commit .env files:**
```bash
# .gitignore
.env
.env.*
*.env
.venv/
__pycache__/
```

**Use example files:**
```bash
# .env.example (safe to commit)
GOOGLE_API_KEY=your_api_key_here
GOOGLE_PROJECT_ID=your_project_id
DEFAULT_MODEL=gemini-2.5-flash
```

---

## 10. Deployment Options

### 10.1 Local Development (Current Phase)

Run agents on your local machine:

```bash
# Activate environment
source .venv/bin/activate

# Run agent
adk run core/orchestrator

# Or run Python directly
python -m core.orchestrator
```

### 10.2 Cloud Run (Future)

Deploy agents to Google Cloud Run:

```bash
# Build container
gcloud builds submit --tag gcr.io/PROJECT_ID/cheatsheet-agent

# Deploy to Cloud Run
gcloud run deploy cheatsheet-agent \
  --image gcr.io/PROJECT_ID/cheatsheet-agent \
  --platform managed \
  --region us-central1 \
  --allow-unauthenticated
```

### 10.3 Vertex AI Agent Engine (Production Scale)

Deploy to Vertex AI for production:

```bash
# Deploy agent to Vertex AI
gcloud ai agents deploy \
  --agent-file=agents/orchestrator.py \
  --region=us-central1 \
  --project=PROJECT_ID
```

**Benefits:**
- Automatic scaling
- Managed infrastructure
- Integration with Google Cloud services
- Built-in monitoring and logging

---

## 11. Troubleshooting

### 11.1 Common Issues

**Issue: `google.adk` module not found**
```bash
# Solution: Ensure virtual environment is activated
source .venv/bin/activate
uv pip install google-adk
```

**Issue: API key errors**
```bash
# Solution: Check environment variable
echo $GOOGLE_API_KEY

# Or set it explicitly
export GOOGLE_API_KEY="your_key_here"
```

**Issue: Agent not responding**
```python
# Solution: Check async/await usage
import asyncio

# Correct
result = await agent.run("prompt")

# Incorrect
result = agent.run("prompt")  # Missing await
```

**Issue: Tool not being called**
```python
# Solution: Ensure tool has proper docstring and type hints
def my_tool(param: str) -> dict:
    """
    Clear description of what this tool does.

    Args:
        param: Description of parameter

    Returns:
        Description of return value
    """
    return {"result": "value"}
```

### 11.2 Debugging Tips

**Enable debug logging:**
```python
import logging

logging.basicConfig(level=logging.DEBUG)
```

**Use ADK web UI for debugging:**
```bash
adk web --port 8000
# Provides visual debugging interface
```

**Test tools independently:**
```python
# Test tool outside of agent
from agents.intent_agent.agent import extract_intent

result = extract_intent("test prompt")
print(result)
```

### 11.3 Performance Issues

**Issue: Agent responses are slow**

Solutions:
- Use `gemini-2.5-flash` instead of `gemini-2.5-pro`
- Reduce token limits
- Simplify tool logic
- Cache repeated requests

```python
agent = Agent(
    name="fast_agent",
    model="gemini-2.5-flash",  # Faster model
    max_tokens=500  # Limit response length
)
```

---

## 12. Resources

### Official Documentation
- **ADK Documentation**: https://google.github.io/adk-docs/
- **Python Getting Started**: https://google.github.io/adk-docs/get-started/python/
- **API Reference**: https://google.github.io/adk-docs/api-reference/python/
- **Google Cloud ADK**: https://docs.cloud.google.com/agent-builder/agent-development-kit/overview

### Code Repositories
- **ADK Python**: https://github.com/google/adk-python
- **ADK Samples**: https://github.com/google/adk-samples
- **Example Agents**: https://github.com/google/adk-samples/tree/main/python

### Learning Resources
- **ADK Codelabs**: https://codelabs.developers.google.com/devsite/codelabs/build-agents-with-adk-foundation
- **Developer Blog**: https://developers.googleblog.com/en/agent-development-kit-easy-to-build-multi-agent-applications/
- **PyPI Package**: https://pypi.org/project/google-adk/

### Community
- **GitHub Issues**: https://github.com/google/adk-python/issues
- **Stack Overflow**: Tag `google-adk`

### Related Technologies
- **uv Package Manager**: https://github.com/astral-sh/uv
- **Pydantic**: https://docs.pydantic.dev/
- **Python asyncio**: https://docs.python.org/3/library/asyncio.html

---

## Quick Reference Card

```bash
# Setup
uv venv && source .venv/bin/activate
uv pip install google-adk
export GOOGLE_API_KEY="your_key"

# Run agents
adk run agents/intent_agent          # CLI mode
adk web --port 8000                  # Web UI mode
python -m agents.intent_agent.agent  # Python mode

# Testing
pytest tests/                        # Run all tests
pytest --cov=agents tests/          # With coverage

# Development
adk web                              # Interactive testing
python test_agent.py                # Quick script test
```

---

**Last Updated**: 2026-01-18
**ADK Version**: Compatible with google-adk 1.x
**Python Version**: 3.10+

For project context and philosophy, see [CLAUDE.md](CLAUDE.md).
