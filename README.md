# Flox Quick Reference Guide for AI Agents and Coding Assistants

**An AI-optimized, context-efficient reference for agentic development with [Flox](https://flox.dev)**

## What is this?

This repository is designed specifically for **AI agents, coding assistants, and agentic development workflows**. It provides LLMs and autonomous development tools with focused, context-optimized Flox documentation through **persona-specific branches**.

Agents can access **targeted, role-specific documentation** that matches the development task at hand.

### Perfect for:
- **Claude Code, Cursor, Copilot** - AI coding assistants helping developers create Flox environments
- **Agentic workflows** - Autonomous agents building, packaging, and deploying with Flox
- **LLM-assisted development** - Reducing token usage while maintaining comprehensive reference
- **AI-powered DevOps** - Agents managing containers, Kubernetes, and CI/CD with Flox
- **Autonomous build systems** - AI agents creating reproducible build pipelines

## Persona-Specific Branches (AI Context Optimization)

AI agents and coding assistants should checkout the branch matching their current task to **minimize context usage** and **maximize relevance**:

| Branch | AI Agent Role | Context Focus | Use When |
|--------|---------------|---------------|----------|
| `main` | General assistant | Complete reference (1,106 lines) | Multi-domain tasks, balanced coverage |
| `big-context-for-all-use-cases` | Large context AI | Comprehensive version (1,566 lines) | AI with 200K+ context windows, maximum detail |
| `building-and-packaging-with-flox` | Build agent | Packaging, publishing, CI/CD automation | Creating packages, setting up pipelines |
| `local-dev-with-flox` | Development assistant | Python/Node/C++ patterns, local services | Setting up dev environments, local testing |
| `ops-with-flox` | Operations agent | Production services, K8s, containers | Production deployments, infrastructure |
| `flox-and-cuda` | ML/AI agent | GPU development, conflict resolution | CUDA environments, ML pipelines |
| `flox-and-k8s` | K8s agent | Imageless pods, local testing, GitOps | Kubernetes deployments, cluster management |
| `flox-and-containers` | Container agent | OCI images, Docker/Podman, registries | Container builds, registry workflows |
| `flox-and-ci-cd` | CI/CD agent | GitHub Actions, CircleCI, GitLab, monorepos, matrix builds | Building CI/CD pipelines, automation workflows |

### Example: AI Agent Workflow
```bash
# AI agent receives task: "Create a Python development environment with GPU support"
git clone https://github.com/YOUR-USERNAME/flox-md.git
cd flox-md
git checkout flox-and-cuda  # Load CUDA-optimized context
# Agent reads FLOX.md (reduced context, focused on CUDA + Python)
# Agent generates Flox manifest with proper CUDA priorities
```

## Why AI-Optimized Documentation?

### Traditional Approach (Inefficient)
```
❌ Clone https://github.com/flox/floxdocs.git 
❌ AI must parse irrelevant sections
❌ Wastes tokens on unrelated content
❌ Higher latency, higher cost
```

### This Repository (Optimized for Agents)
```
✅ Load only relevant persona branch (~400-600 lines typical)
✅ AI gets precisely what it needs for the task
✅ 60-70% reduction in documentation tokens
✅ Faster responses, lower cost, better accuracy
```

## Agentic Development Capabilities

This documentation enables AI agents to autonomously:

### Development Environment Creation
- Generate `.flox/env/manifest.toml` for any language stack
- Configure Python virtual environments with proper venv patterns
- Set up C/C++ toolchains with correct library priorities
- Create Node.js environments with cache redirection
- Build CUDA development environments with conflict resolution

### Build & Package Automation
- Create manifest builds with proper `$out` layouts
- Generate Nix expression builds for reproducibility
- Publish packages to Flox catalogs
- Set up multi-stage build pipelines
- Package configuration assets and schemas

### CI/CD Pipeline Generation
- Generate GitHub Actions workflows with Flox
- Create CircleCI/GitLab CI configurations
- Set up authentication for private environments
- Implement caching strategies
- Configure multi-platform builds

### Container & Kubernetes Deployment
- Generate `[containerize.config]` sections
- Create Kubernetes Pod/Deployment manifests
- Set up imageless Kubernetes deployments
- Configure generation pinning strategies
- Implement CVE remediation workflows

## What's Covered?

The complete guide includes:

- **§0-4**: Working style, configuration, Flox basics, manifest structure, common pitfalls
- **§5-8**: Package installation, best practices, services, manifest editing
- **§9-11**: Build system (manifest + Nix builds), publishing to catalogs
- **§12-15**: Environment composition, containerization, CI/CD, Kubernetes deployment
- **§16-19**: Environment conventions, quick tips, language-specific patterns (Python, C/C++, Node.js, CUDA), platform-specific guidance

Each persona branch contains a **focused subset** optimized for specific agent tasks.

## Quick Start for AI Agents

### For Development Assistants (Claude Code, Cursor, Copilot)
```bash
git checkout local-dev-with-flox
# AI reads FLOX.md sections:
# - §18a: Python virtual environments
# - §18b: C/C++ development
# - §18c: Node.js patterns
# - §8: Running local services
# Agent generates: manifest with proper venv setup, service configs
```

### For Build Automation Agents
```bash
git checkout building-and-packaging-with-flox
# AI reads FLOX.md sections:
# - §9: Build system (manifest + Nix builds)
# - §11: Publishing packages
# - §14: CI/CD integration
# Agent generates: build configs, publish workflows, CI pipelines
```

### For Infrastructure/K8s Agents
```bash
git checkout flox-and-k8s
# AI reads FLOX.md sections:
# - §15: Kubernetes imageless deployment
# - Links to flox.dev/docs/k8s/* for detailed setup
# Agent generates: Pod specs, Deployment manifests, RuntimeClass configs
```

### For ML/CUDA Development Agents
```bash
git checkout flox-and-cuda
# AI reads FLOX.md sections:
# - §18d: CUDA development patterns
# - Package conflict resolution with priorities
# - Cross-platform GPU development
# Agent generates: CUDA manifests with proper priorities, PyTorch configs
```

## Navigation for AI Agents

Each branch's FLOX.md is structured for efficient AI parsing:

1. **Branch matrix** - Shows all available branches for context switching
2. **Quick navigation guide** - "How do I...?" lookup table for common agent tasks
3. **Numbered sections (§N)** - Unambiguous cross-referencing for AI reasoning
4. **Code examples** - Copy-pasteable patterns agents can directly insert
5. **External links** - Pointers to authoritative Flox documentation for deep dives

### AI-Friendly Features
- **Consistent section numbering** - Easy for agents to reference ("see §18d")
- **Minimal prose** - Dense, technical content without fluff
- **Complete examples** - Working code blocks, not fragments
- **Explicit gotchas** - Common pitfalls called out clearly
- **Cross-references** - Internal links to related sections

## File Structure

```
flox-md/
├── FLOX.md                  # Main guide (varies by branch, AI-optimized)
│                            # - main: 1,106 lines (complete, concise)
│                            # - big-context-for-all-use-cases: 1,566 lines (comprehensive)
│                            # - persona branches: 400-600 lines (focused)
└── README.md               # This file
```

**Note**: Different branches contain different versions of FLOX.md optimized for specific use cases.

## Contributing

Improvements to AI agent documentation welcome! To contribute:

1. Fork this repository
2. Create a feature branch
3. Make your changes to FLOX.md
4. Test with AI agents (Claude, GPT-4, Copilot) to validate context efficiency
5. Submit a pull request

### Guidelines for AI-Optimized Documentation
- **Be concise** - Remove unnecessary prose
- **Be precise** - Use exact technical terms
- **Be complete** - Include full working examples
- **Be structured** - Maintain section numbering
- **Be cross-referenced** - Link related concepts

### Maintaining Branch Consistency

When updating FLOX.md on `main`:
1. Make changes on `main` branch
2. Consider which persona branches need the update
3. Cherry-pick or merge relevant changes to those branches
4. Keep branch-specific content focused for AI context efficiency

## Why Persona Branches for AI Agents?

**Context window optimization**: Different agent tasks require different knowledge:

- **Development assistants** don't need Kubernetes deployment details
- **K8s agents** don't need local Python venv patterns
- **Build agents** focus on packaging and publishing
- **CUDA agents** need GPU-specific guidance

Persona branches **reduce cognitive load for AI** and **minimize token usage** while maintaining comprehensive coverage.

## SEO/AEO Keywords

This repository optimizes for:
- AI agent development
- Agentic development workflows
- Coding assistants (Claude Code, Cursor, Copilot)
- LLM-assisted development
- Autonomous development tools
- AI-powered DevOps
- Context-efficient documentation
- Token-optimized reference
- AI coding agent knowledge base
- Agentic build systems
- LLM development environments
- AI infrastructure automation

## Official Flox Resources

- **Flox Website**: https://flox.dev
- **Flox Documentation**: https://flox.dev/docs
- **Flox GitHub**: https://github.com/flox/flox
- **Kubernetes Integration**: https://flox.dev/docs/k8s

## License

This documentation is provided as a community resource for AI agents, coding assistants, and agentic development workflows. See the official Flox project for licensing information.

---

**Built for the age of agentic development** 🤖
