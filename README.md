# Ideas to Cheatsheet

> Transform rough ideas into clear, concise, interactive cheat sheets

An **Ideas to Life** experiment that converts your prompts into beautiful, shareable learning resources.

---

## 🎯 What It Does

**Ideas to Cheatsheet** takes a rough idea or topic and generates an interactive, well-structured cheat sheet optimized for learning and sharing.

**Example:**
```
Input: "I need to learn REST APIs"
Output: Interactive cheat sheet with fundamentals, examples, and best practices
```

---

## ✨ Features

- **Interactive HTML Output** - Beautiful, responsive cheat sheets that work in any browser
- **Zero Setup** - Open and use immediately, no installation required (current version)
- **Learning-Optimized** - Content structured for quick understanding and retention
- **Shareable** - Easy to distribute and reference
- **Future: AI-Powered** - Evolving to use agentic AI for intelligent content generation

---

## 🚀 Quick Start

### Current Version (HTML/JavaScript)

1. **Open the cheat sheet**
   ```bash
   open claude-cheatsheet.html
   ```

2. **Use the interactive features**
   - Search for specific topics
   - Filter by category
   - Click code snippets to copy
   - Navigate between sections

That's it! No installation, no dependencies.

---

## 🔮 Future Vision: Agentic AI

This project is evolving from static HTML to an intelligent, AI-powered system using **Google ADK** (Agent Development Kit).

### Planned Architecture

```
User Prompt → Intent Agent → Structure Agent → Content Agent → Refinement Agent → HTML Output
```

**Specialized Agents:**
- **Intent Agent** - Understands what you want to learn
- **Structure Agent** - Creates optimal cheat sheet outline
- **Content Agent** - Generates clear, concise explanations
- **Refinement Agent** - Polishes for clarity and consistency

### Technology Stack (Future)

- **Agent Framework:** Google ADK
- **Language:** Python 3.10+
- **Environment Manager:** uv
- **Deployment:** Local-first, then cloud-scale

---

## 📁 Project Structure

```
ideas-to-cheatsheet/
├── README.md                   # This file (user-facing docs)
├── CLAUDE.md                   # Project philosophy and architecture (for Claude Code)
├── ADK_GUIDE.md               # Google ADK implementation guide (technical reference)
├── claude-cheatsheet.html     # Current interactive cheat sheet
├── ideas-to-life-logo.png     # Branding assets
│
└── (Future: Python agent layer)
    ├── agents/                # Agent implementations
    │   ├── intent_agent/
    │   ├── structure_agent/
    │   ├── content_agent/
    │   └── refinement_agent/
    ├── core/                  # Domain logic and orchestration
    ├── ui/                    # Rendering and templates
    └── tests/                 # Test suite
```

---

## 🛠️ Development Setup (Future Agentic Version)

When the agentic AI features are added, setup will be:

### Prerequisites

- Python 3.10 or later
- uv (package manager)
- Google Cloud API key

### Installation

```bash
# Clone the repository
git clone https://github.com/your-username/ideas-to-cheatsheet.git
cd ideas-to-cheatsheet

# Install uv
curl -LsSf https://astral.sh/uv/install.sh | sh

# Create virtual environment
uv venv

# Activate environment
source .venv/bin/activate  # macOS/Linux
# .venv\Scripts\activate    # Windows

# Install dependencies
uv pip install -r requirements.txt

# Set up environment variables
cp .env.example .env
# Edit .env with your Google API key

# Run the agent pipeline
adk run core/orchestrator
```

For detailed implementation guidance, see [ADK_GUIDE.md](ADK_GUIDE.md).

---

## 📖 Documentation

- **[README.md](README.md)** (this file) - User-facing documentation
- **[CLAUDE.md](CLAUDE.md)** - Project architecture and philosophy for Claude Code
- **[ADK_GUIDE.md](ADK_GUIDE.md)** - Google ADK technical implementation guide

---

## 🎨 Design Philosophy

**Launch simple → Learn fast → Evolve deliberately**

This project follows the Ideas to Life approach:

1. **Start Simple** - Begin with working HTML/JavaScript
2. **Learn from Use** - Gather feedback and understand real needs
3. **Evolve Intelligently** - Add AI agents when complexity justifies it
4. **Stay Transparent** - Document decisions and trade-offs

---

## 🤝 Contributing

This is an experimental project under active development. Contributions are welcome!

### Areas for Contribution

- **Content improvements** - Enhance existing cheat sheet content
- **Agent development** - Help build the AI agent layer
- **Testing** - Add test coverage
- **Documentation** - Improve guides and examples
- **Design** - Enhance UI/UX

### Getting Started

1. Read [CLAUDE.md](CLAUDE.md) to understand the project vision
2. Check [ADK_GUIDE.md](ADK_GUIDE.md) for technical implementation details
3. Open an issue to discuss your ideas
4. Submit a pull request

---

## 🧪 Current Status

**Phase:** Initial Release (HTML/JavaScript)

- ✅ Interactive HTML cheat sheet
- ✅ Search and filter functionality
- ✅ Responsive design
- ✅ Copy-to-clipboard for code snippets
- 🚧 Python agent layer (planned)
- 🚧 Google ADK integration (planned)
- 🚧 Dynamic content generation (planned)

---

## 🗺️ Roadmap

### Phase 1: Foundation (Current)
- ✅ Static HTML/JavaScript implementation
- ✅ Interactive features
- ✅ Documentation structure

### Phase 2: Agent Development (Next)
- ⏳ Set up Python environment with uv
- ⏳ Implement Intent Agent
- ⏳ Implement Structure Agent
- ⏳ Implement Content Agent
- ⏳ Implement Refinement Agent

### Phase 3: Integration
- ⏳ Agent orchestration
- ⏳ Connect agents to HTML renderer
- ⏳ End-to-end pipeline testing

### Phase 4: Enhancement
- ⏳ Advanced agent capabilities
- ⏳ Customization options
- ⏳ Performance optimization

### Phase 5: Scale
- ⏳ Cloud deployment
- ⏳ API access
- ⏳ Multi-user support

---

## 🎯 Use Cases

**Students & Learners**
- Quick reference for new topics
- Study guides for exams
- Concept summaries

**Developers**
- API quick references
- Language syntax guides
- Framework cheat sheets

**Educators**
- Teaching aids
- Handout materials
- Course supplements

**Teams**
- Internal documentation
- Process guides
- Best practices references

---

## 🔗 Links

- **Ideas to Life**: [ideas-to-life.ai](https://ideas-to-life.ai)
- **GitHub Repository**: [github.com/your-username/ideas-to-cheatsheet](https://github.com/your-username/ideas-to-cheatsheet)
- **Live Demo**: [your-demo-url.com](https://your-demo-url.com)

---

## 📜 License

MIT License - See [LICENSE](LICENSE) for details

---

## 👤 Author

**Alexandre Franco**

Part of the Ideas to Life initiative - turning ideas into consumer experiences with Generative AI.

Built with transparency. Shipped with intent.

---

## 🙏 Acknowledgments

- **Google ADK** - Agent Development Kit framework
- **Anthropic** - Claude AI and Claude Code
- **Community** - Contributors and early adopters

---

## 📝 Changelog

### v0.1.0 (2026-01-18)
- Initial release with HTML/JavaScript implementation
- Interactive search and filter
- Responsive design
- Documentation structure (CLAUDE.md, ADK_GUIDE.md)

---

## 💬 Feedback

Have ideas, questions, or feedback?

- **Issues**: Open an issue on GitHub
- **Email**: alexandre@ideas-to-life.ai
- **Website**: [ideas-to-life.ai](https://ideas-to-life.ai)

---

**Made with ❤️ by Ideas to Life**

*This is an experiment. Launch simple → Learn fast → Evolve deliberately.*
