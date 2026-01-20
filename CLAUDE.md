# CLAUDE.md

This file defines how Claude Code should work inside the ideas-to-cheatsheet project.

It captures:
- The current project architecture
- The intended evolution toward an Agentic AI system
- Coding standards and conventions
- Setup and dependency expectations
- Workflows and custom commands
- Important constraints and gotchas

Claude should treat this file as authoritative project context and follow it before making changes.

---

## 1. Project Overview

**ideas-to-cheatsheet** is an experiment within the Ideas to Life initiative.

For canonical Ideas to Life experiment guardrails, see **experiments-charter.md (platform-level charter)**:
- https://github.com/ideas-to-life/ideas-to-life-prompts/blob/main/governance/experiments-charter.md

Note: this repo may also include a local `EXPERIMENTS.md` pointer file for convenience, but the platform-level charter above is the source of truth.

### Purpose
- Transform a rough idea or prompt into a clear, concise, interactive cheat sheet
- Optimized for learning, sharing, and signal gathering

### Current State
- Static/interactive HTML + JavaScript implementation
- Generated cheat sheets rendered in the browser
- Minimal dependencies
- Designed for fast iteration and public deployment

### Vision
- Evolve from static UI logic into an Agentic AI-powered system
- Use **Google ADK** as the agentic framework
- Introduce specialized agents to handle understanding, structuring, synthesis, and refinement
- Agent layer written in **Python**
- Virtual environment management via **uv**

### Guiding Principle

> Launch simple → learn fast → evolve deliberately

---

## 2. Current Architecture (Baseline)

### 2.1 Frontend

**Technology Stack:**
- Plain HTML, CSS, and JavaScript
- No heavy frontend frameworks
- Components are simple, explicit, and readable
- Logic is mostly imperative and synchronous

**File Structure:**
- [claude-cheatsheet.html](claude-cheatsheet.html) - Main interactive cheat sheet implementation

**Responsibilities:**
- Accept user input (idea/topic)
- Render cheat sheet content
- Handle lightweight interactivity (expand/collapse, navigation)

### 2.2 Constraints (Intentional)
- Avoid unnecessary abstractions
- Avoid premature optimization
- Prefer clarity over cleverness

**Claude must not introduce frameworks or build systems unless explicitly requested.**

---

## 3. Target Architecture (Vision)

The project is expected to evolve into an Agentic AI architecture.

### 3.1 High-Level Agent Roles

Planned agent responsibilities (conceptual, not all implemented yet):

1. **Intent/Idea Agent**
   - Interprets raw user intent
   - Clarifies scope and audience
   - Determines cheat sheet requirements

2. **Structure Agent**
   - Produces a canonical cheat sheet outline
   - Enforces consistency and hierarchy
   - Defines section organization

3. **Content Agent**
   - Generates concise, accurate explanations
   - Applies learning-oriented language
   - Creates examples and code snippets

4. **Refinement/Editor Agent**
   - Improves clarity, removes redundancy
   - Ensures tone and formatting consistency
   - Validates completeness

5. **UI/Presentation Agent** (optional)
   - Maps structured content to HTML-friendly output
   - Handles styling and interactivity

### 3.2 Agentic Framework

**Primary Framework:** Google ADK (Agentic Development Kit)

**Key Decisions:**
- First phase: agents run **locally during development** (and in early deployments)
- Agent layer language: **Python** (not TypeScript)
- Virtual environment management: **uv** (not poetry, conda, or pipenv)

**Agent Design Principles:**
- Small
- Single-responsibility
- Composable
- Loosely coupled

Claude should anticipate this evolution when refactoring code:
- Keep boundaries clear
- Avoid tightly coupling UI logic with generation logic
- Prepare for agent-based orchestration

### 3.3 Data Flow (Future State)

```
User Input → Intent Agent → Structure Agent → Content Agent → Refinement Agent → UI Agent → HTML Output
```

Each agent should:
- Accept structured input
- Produce structured output
- Be independently testable
- Handle errors gracefully

---

## 4. Coding Standards & Conventions

### 4.1 General Principles

- **Readability > abstraction**
- **Explicit > implicit**
- Small functions
- Clear naming
- Document why, not what

### 4.2 JavaScript Conventions

**For Current Frontend Code:**
- Prefer modern ES6+ syntax
- Use `const` by default, `let` only when reassignment is required
- Avoid deeply nested logic
- Use pure functions where possible
- No jQuery or heavy dependencies
- Keep DOM manipulation explicit

**Example:**
```javascript
// Good
const generateSection = (title, content) => {
  const section = document.createElement('section');
  section.className = 'cheat-section';
  section.innerHTML = `<h2>${title}</h2><div>${content}</div>`;
  return section;
};

// Avoid
function doStuff(x) {
  let y = x;
  if (y) {
    // deeply nested logic
  }
}
```

### 4.3 Python Conventions

**For Future Agent Layer:**
- Follow PEP 8 style guide
- Use type hints (Python 3.10+)
- Prefer dataclasses or Pydantic models for structured data
- Use async/await for I/O-bound operations
- Keep functions small and focused
- Write docstrings for public APIs

**Example:**
```python
from dataclasses import dataclass
from typing import List

@dataclass
class CheatSheetSection:
    title: str
    content: str
    order: int

async def generate_section(prompt: str) -> CheatSheetSection:
    """
    Generate a cheat sheet section from a prompt.

    Args:
        prompt: User input describing the desired section

    Returns:
        Structured section data
    """
    # Implementation here
    pass
```

### 4.4 File Structure (Guidance)

As the project grows, prefer:

```
/
├── claude-cheatsheet.html       # Current frontend
├── CLAUDE.md                    # This file
├── README.md                    # Project documentation
├── /agents                      # Future: Agent definitions
│   ├── intent_agent.py
│   ├── structure_agent.py
│   ├── content_agent.py
│   └── refinement_agent.py
├── /core                        # Future: Domain logic
│   ├── models.py               # Data models
│   ├── orchestrator.py         # Agent coordination
│   └── utils.py
├── /ui                         # Future: Rendering logic
│   ├── templates/
│   └── renderer.py
└── /tests                      # Future: Test suite
    ├── test_agents.py
    └── test_integration.py
```

**Do not move files unless explicitly asked.**

---

## 5. Dependencies & Setup

### 5.1 Environment Management (Python)

**Primary Tool:** `uv`

`uv` is the preferred tool for:
- Creating and managing virtual environments
- Installing Python dependencies
- Running scripts locally
- Fast, deterministic, minimal ceremony

**Why uv?**
- Significantly faster than pip/poetry
- Simple, predictable behavior
- Aligns with local-first agent development
- Minimal configuration required

**Conventions:**
```bash
# Create a virtual environment
uv venv

# Activate the environment
source .venv/bin/activate  # macOS/Linux
.venv\Scripts\activate     # Windows

# Install dependencies
uv pip install google-adk anthropic pydantic

# Install from requirements file
uv pip install -r requirements.txt

# Run a script
uv run python agents/intent_agent.py
```

**Do not introduce alternative tools (poetry, conda, pipenv) unless explicitly requested.**

### 5.2 Google ADK Integration

For detailed Google ADK implementation guidance, see [ADK_GUIDE.md](ADK_GUIDE.md).

The ADK Guide includes:
- Installation and setup with uv
- Agent structure patterns for this project (Intent, Structure, Content, Refinement agents)
- Multi-agent orchestration examples
- Local development workflow (CLI and Web UI)
- Tool definitions and best practices
- Testing strategies
- Environment configuration
- Deployment options

**Quick Reference:**
```bash
# See ADK_GUIDE.md for full details
uv pip install google-adk
export GOOGLE_API_KEY="your_key"
adk run agents/intent_agent
```

### 5.3 Current State

**Current Dependencies:**
- None (runs entirely in the browser)
- No build step required
- No package.json or node_modules

### 5.4 Future State (Anticipated)

When agentic components are introduced:

**Python Runtime:**
- Python 3.10+ required
- Virtual environment via `uv venv`

**Expected Dependencies:**
```
google-adk          # Agentic framework
anthropic           # Claude API access (if needed)
pydantic            # Data validation
fastapi             # Optional: API layer
uvicorn             # Optional: ASGI server
pytest              # Testing
black               # Code formatting
ruff                # Linting
```

**Clear Separation:**
- Generation logic: Agent layer (local-first initially)
- Rendering logic: Frontend (HTML/JS)
- Communication: JSON over HTTP or direct Python invocation

**Claude should:**
- Clearly document any new dependency
- Explain why it is needed
- Avoid introducing tooling by default

---

## 6. Workflows for Claude Code

### 6.1 Default Workflow

1. Read existing files fully
2. Identify current intent and constraints
3. Propose minimal, incremental changes
4. Implement with comments where clarity helps

### 6.2 Refactoring Workflow

When refactoring:
- Preserve behavior unless explicitly told otherwise
- Prefer extraction over rewriting
- Keep diffs small and reviewable
- Test after each change

### 6.3 Agentic Evolution Workflow

When introducing agent concepts:
1. Start with interfaces/contracts (define data models first)
2. Mock behavior before full implementation
3. Keep agents decoupled from UI
4. Write tests alongside agent code
5. Document agent responsibilities clearly

**Example Agent Implementation Steps:**
```
1. Define input/output models (Pydantic/dataclasses)
2. Create agent skeleton with type hints
3. Implement core logic
4. Add error handling
5. Write unit tests
6. Document usage examples
7. Integrate with orchestrator
```

### 6.4 Testing Workflow

**Current State:** No automated tests yet

**Future State:**
```bash
# Run all tests
pytest

# Run specific test file
pytest tests/test_agents.py

# Run with coverage
pytest --cov=agents tests/
```

### 6.5 Git Workflow

**Commit Guidelines:**
- Use descriptive commit messages
- Reference issue numbers when applicable
- Keep commits focused and atomic

**Example:**
```bash
git add agents/intent_agent.py tests/test_intent_agent.py
git commit -m "Add intent agent with basic prompt parsing"
```

---

## 7. Custom Commands (Conceptual)

Claude may reference or suggest (but not assume existing):

**Conceptual Commands:**
- `generate-cheatsheet` - End-to-end generation pipeline
- `validate-structure` - Ensure cheat sheet schema validity
- `render-html` - Map structured output to HTML
- `test-agents` - Run agent test suite

**Future CLI (when agent layer exists):**
```bash
# Generate a cheat sheet from a prompt
python -m core.cli generate "explain REST APIs"

# Test a specific agent
python -m agents.intent_agent test

# Run the orchestrator
python -m core.orchestrator serve
```

Commands are conceptual unless explicitly implemented.

---

## 8. Gotchas & Mistakes to Avoid

**❌ Avoid These:**
- Introducing frameworks without request
- Mixing UI rendering with generation logic
- Over-engineering agent orchestration too early
- Losing simplicity of the original experiment
- Creating abstractions before you have 3 concrete examples
- Adding dependencies that aren't strictly necessary
- Writing generic "utility" functions that obscure intent
- Premature optimization of agent communication

**✅ Do These Instead:**
- Keep the HTML/JS frontend simple and standalone
- Design clear boundaries between agents
- Start with the simplest working implementation
- Add complexity only when needed
- Document why decisions were made
- Test agents independently before integration

**Remember:**
> This is an experiment, not a platform (yet).

---

## 9. Decision-Making Heuristics

When uncertain, Claude should:

1. **Ask before making architectural leaps**
   - "Should I introduce a new agent for this?"
   - "Do you want me to set up the Python environment now?"

2. **Default to the simplest working solution**
   - One file > multiple files
   - Inline logic > extracted function (until pattern emerges)
   - Direct invocation > complex orchestration

3. **Align changes with the Ideas to Life philosophy**
   - Fast iteration over perfect architecture
   - Learning from real usage over speculation
   - Transparency in decision-making

4. **Respect the evolution path**
   - Don't break the current HTML implementation
   - Prepare for Python agents without forcing them
   - Keep both worlds working during transition

---

## 10. Python/uv Specific Guidance

### 10.1 Setting Up the Python Environment

**First Time Setup:**
```bash
# Install uv (if not already installed)
curl -LsSf https://astral.sh/uv/install.sh | sh

# Create virtual environment
uv venv

# Activate environment
source .venv/bin/activate

# Install base dependencies
uv pip install google-adk pydantic
```

### 10.2 Project Structure for Python Code

When adding Python agents, use this structure:

```python
# agents/base.py
from abc import ABC, abstractmethod
from typing import Generic, TypeVar

Input = TypeVar('Input')
Output = TypeVar('Output')

class Agent(ABC, Generic[Input, Output]):
    """Base class for all agents."""

    @abstractmethod
    async def process(self, input_data: Input) -> Output:
        """Process input and return output."""
        pass
```

### 10.3 Google ADK Integration

**Expected Usage Pattern:**
```python
from google_adk import Agent, Task
from pydantic import BaseModel

class IntentInput(BaseModel):
    user_prompt: str

class IntentOutput(BaseModel):
    topic: str
    audience: str
    depth: str

class IntentAgent(Agent):
    async def execute(self, input_data: IntentInput) -> IntentOutput:
        # Agent logic here
        pass
```

### 10.4 Local Development

**Running Agents Locally:**
```bash
# Activate environment
source .venv/bin/activate

# Run a single agent
python agents/intent_agent.py

# Run the orchestrator
python -m core.orchestrator

# Run tests
pytest tests/
```

### 10.5 Dependencies Management

**Adding New Dependencies:**
```bash
# Install and save to requirements.txt
uv pip install package-name
uv pip freeze > requirements.txt

# Install from requirements
uv pip install -r requirements.txt
```

**Pinning Versions:**
```
# requirements.txt
google-adk>=1.0.0,<2.0.0
anthropic==0.25.0
pydantic>=2.0.0
```

---

## 11. Open Questions / Clarifications

Claude may ask questions if needed, especially about:

1. **Backend Architecture:**
   - When to introduce a backend layer beyond local-first execution?
   - Should there be an API between agents and frontend?
   - How should agent orchestration work?

2. **Google ADK Details:**
   - Preferred Google ADK runtime configuration?
   - How should agents communicate (direct calls, message queue, events)?
   - What authentication/API keys are needed?

3. **Data Persistence:**
   - Should generated cheat sheets be saved?
   - Where should agent state be stored?
   - Do we need a database?

4. **Deployment:**
   - How will agents be deployed initially (local, cloud, hybrid)?
   - What's the path from local development to production?
   - Should the frontend and agents be separate deployments?

**If unclear, pause and ask rather than assume.**

---

## 12. Evolution Checklist

When transitioning from HTML/JS to Agentic AI:

**Phase 1: Preparation**
- [ ] Set up Python environment with uv
- [ ] Install Google ADK and core dependencies
- [ ] Define data models for agent communication
- [ ] Create agent base classes

**Phase 2: First Agent**
- [ ] Implement Intent Agent
- [ ] Write tests for Intent Agent
- [ ] Create simple CLI to test agent
- [ ] Verify agent works locally

**Phase 3: Agent Pipeline**
- [ ] Implement Structure Agent
- [ ] Implement Content Agent
- [ ] Create orchestrator to chain agents
- [ ] Test full pipeline end-to-end

**Phase 4: Integration**
- [ ] Connect agent output to HTML renderer
- [ ] Update frontend to call agent pipeline
- [ ] Test integrated system
- [ ] Maintain backward compatibility with static version

**Phase 5: Refinement**
- [ ] Add Refinement Agent
- [ ] Implement error handling
- [ ] Add logging and monitoring
- [ ] Optimize agent performance

---

## 13. Resources & References

**Google ADK:**
- Documentation: (to be added when available)
- Examples: (to be added)

**uv Documentation:**
- https://github.com/astral-sh/uv
- Install: `curl -LsSf https://astral.sh/uv/install.sh | sh`

**Python Best Practices:**
- PEP 8: https://pep8.org/
- Type hints: https://docs.python.org/3/library/typing.html
- Pydantic: https://docs.pydantic.dev/

**Ideas to Life:**
- Philosophy: Launch simple → learn fast → evolve deliberately
- Focus: Consumer experiences with Generative AI
- Transparency and intent in every decision

---

## 14. Summary for Claude Code

When working on this project:

1. **Respect the current simplicity** - The HTML/JS implementation works and should remain functional
2. **Prepare for agentic evolution** - Design with clear boundaries and interfaces
3. **Use uv for Python** - Don't introduce other environment managers
4. **Keep agents small** - Single responsibility, composable, testable
5. **Ask before big changes** - Architecture decisions should be collaborative
6. **Document decisions** - Future you (and others) will thank you
7. **Test as you go** - Don't build a house of cards

**When in doubt: simple, working code > perfect architecture**

---

*End of CLAUDE.md*
