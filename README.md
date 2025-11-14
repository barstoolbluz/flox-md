# Flox Quick Reference Guide

A comprehensive, persona-focused quick reference guide for [Flox](https://flox.dev) - the declarative, reproducible development environment manager.

## What is this?

This repository contains **FLOX.md**, a 1,106-line quick reference covering everything from local development to production deployment with Flox. Instead of one massive document for everyone, we use **persona-specific branches** so you can focus on what matters to you.

## Persona-Specific Branches

Checkout the branch that matches your use case:

| Branch | For | Focus |
|--------|-----|-------|
| `main` | Everyone | Complete reference (all 1,106 lines) |
| `building-and-packaging-with-flox` | Build engineers | Packaging, publishing, CI/CD automation |
| `local-dev-with-flox` | Developers | Python/Node/C++ patterns, local services |
| `ops-with-flox` | SREs/operators | Production services, K8s, containers |
| `flox-and-cuda` | CUDA developers | GPU development, conflict resolution |
| `flox-and-k8s` | K8s engineers | Imageless pods, local testing, GitOps |
| `flox-and-containers` | Container engineers | OCI images, Docker/Podman, registries |

**Example:**
```bash
git clone https://github.com/YOUR-USERNAME/flox-md.git
cd flox-md
git checkout local-dev-with-flox  # Developer-focused branch
cat FLOX.md                         # Read the focused guide
```

## What's Covered?

The guide includes:

- **§0-4**: Working style, configuration, Flox basics, manifest structure, common pitfalls
- **§5-8**: Package installation, best practices, services, manifest editing
- **§9-11**: Build system (manifest + Nix builds), publishing to catalogs
- **§12-15**: Environment composition, containerization, CI/CD, Kubernetes deployment
- **§16-19**: Environment conventions, quick tips, language-specific patterns (Python, C/C++, Node.js, CUDA), platform-specific guidance

## Quick Start

### For Developers (Python/Node/C++)
```bash
git checkout local-dev-with-flox
# Read FLOX.md sections:
# - §18a: Python virtual environments
# - §18b: C/C++ development
# - §18c: Node.js patterns
# - §8: Running local services
```

### For Build Engineers
```bash
git checkout building-and-packaging-with-flox
# Read FLOX.md sections:
# - §9: Build system (manifest + Nix builds)
# - §11: Publishing packages
# - §14: CI/CD integration
```

### For K8s Engineers
```bash
git checkout flox-and-k8s
# Read FLOX.md sections:
# - §15: Kubernetes imageless deployment
# - Links to flox.dev/docs/k8s/* for detailed setup
```

### For CUDA Developers
```bash
git checkout flox-and-cuda
# Read FLOX.md sections:
# - §18d: CUDA development patterns
# - Package conflict resolution with priorities
# - Cross-platform GPU development
```

## Navigation

Each branch's FLOX.md includes:

1. **Branch matrix** - Shows all available branches
2. **Quick navigation guide** - "How do I...?" lookup table
3. **Numbered sections (§N)** - Easy cross-referencing
4. **Code examples** - Copy-pasteable patterns
5. **External links** - Deep dives into Flox docs

## File Structure

```
flox-md/
├── FLOX.md                  # Main guide (varies by branch)
├── FLOX-OMNIBUS.md         # Original 1,566-line comprehensive version
├── FLOX_refactor_preserve_structure.md  # 1,194-line refactored version
└── README.md               # This file
```

## Contributing

Improvements welcome! To contribute:

1. Fork this repository
2. Create a feature branch
3. Make your changes to FLOX.md
4. Test across relevant persona branches
5. Submit a pull request

### Maintaining Branch Consistency

When updating FLOX.md on `main`:
1. Make changes on `main` branch
2. Consider which persona branches need the update
3. Cherry-pick or merge relevant changes to those branches
4. Keep branch-specific content focused

## Why Persona Branches?

Different users need different information:

- **Developers** don't need Kubernetes deployment details
- **K8s engineers** don't need local Python venv patterns
- **Build engineers** focus on packaging and publishing
- **CUDA developers** need GPU-specific guidance

Persona branches keep each guide focused and relevant, reducing cognitive load.

## Official Flox Resources

- **Flox Website**: https://flox.dev
- **Flox Documentation**: https://flox.dev/docs
- **Flox GitHub**: https://github.com/flox/flox
- **Kubernetes Integration**: https://flox.dev/docs/k8s

## License

This documentation is provided as a community resource for Flox users. See the official Flox project for licensing information.
