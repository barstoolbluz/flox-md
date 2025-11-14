# FLOX.md Branch Content Mapping Report

## Overview

This report defines the content structure for 6 use-case-specific branches of the Flox documentation. Each branch contains a curated `FLOX.md` file tailored to a specific persona/workflow.

**Main Branch**: Complete FLOX.md (1,566 lines, §0-§19)

**Branch Strategy**: Each topic branch has the same filename (`FLOX.md`) but filtered/curated content. Users browse GitHub branches to find their use case and get a specialized view of the documentation.

---

## Branch 1: `building-and-packaging-with-flox`

**Target Persona**: Build engineers, package maintainers, release engineers

**Core Focus**: Building reproducible packages, publishing to catalogs, creating OCI images, automating builds

### Section Mapping

| Main §  | Branch § | Section Title | Notes |
|---------|----------|---------------|-------|
| Quick Nav | Quick Nav | "How do I...?" | **Customized** for build workflows |
| §0 | §0 | Working Style & Structure | Keep all |
| §1 | §1 | Configuration & Secrets | Keep all |
| §2 | §2 | Flox Basics | Keep all |
| §3 | §3 | Core Commands | **Focus**: `flox build`, `flox publish`, `flox containerize` |
| §4 | §4 | Manifest Structure | Keep all |
| §4b | §4b | Common Pitfalls | Keep all |
| §5 | §5 | The [install] Section | **Focus**: `runtime-packages` for builds |
| §6 | §6 | Best Practices | Keep all |
| §7 | §7 | Editing Manifests Non-Interactively | Keep all |
| §8 | ❌ EXCLUDE | Services | Not relevant to build workflows |
| §9 | **§8** | Build System | **CORE CONTENT** — renumber to §8 |
| §10 | **§9** | Nix Expression Builds | **CORE CONTENT** — renumber to §9 |
| §11 | **§10** | Publishing | **CORE CONTENT** — renumber to §10 |
| §12 | §11 | Layering vs Composition | Keep for build composition patterns |
| §13 | **§12** | Containerization | **DEPLOYMENT TARGET** — renumber to §12 |
| §14 | **§13** | CI/CD Integration | **AUTOMATION** — renumber to §13 |
| §15 | ❌ EXCLUDE | Kubernetes Deployment | Separate branch: flox-and-k8s |
| §16 | §14 | Environment Variable Convention | Keep all |
| §17 | §15 | Quick Tips | Keep all |
| §18a-d | ❌ EXCLUDE | Language-Specific Patterns | Separate branch: local-dev-with-flox |
| §19 | §16 | Platform-Specific Pattern | Keep for cross-platform builds |

### Quick Navigation Customization

```markdown
## Quick Navigation Guide - "How do I...?"

### Building & Packaging (CORE)
- **Package my application** → §8.1 (Manifest Builds)
- **Create reproducible builds** → §8.2 (Sandbox modes)
- **Use Nix expressions** → §9 (Nix Expression Builds)
- **Publish to team catalog** → §10 (Publishing)
- **Package configuration/assets** → §8.9 (Beyond Code)

### Build Patterns
- **Create multi-stage builds** → §8.5 (Multi-Stage Examples)
- **Minimize runtime dependencies** → §8.6 (Trimming Dependencies)
- **Handle cross-platform builds** → §8.8 (Cross-Platform)

### Deployment
- **Build OCI container images** → §12 (Containerization)
- **Automate with CI/CD pipelines** → §13 (CI/CD Integration)

### Foundation
- **Understand manifest structure** → §4 (Manifest Structure)
- **Find and install packages** → §3 (flox search/install)
- **Handle package conflicts** → §5 (priority/pkg-group)
```

**Estimated Line Count**: ~950 lines (60% of main)

---

## Branch 2: `local-dev-with-flox`

**Target Persona**: Developers, data scientists, data engineers doing local development

**Core Focus**: Environment setup, language-specific patterns, local services, development workflows

### Section Mapping

| Main §  | Branch § | Section Title | Notes |
|---------|----------|---------------|-------|
| Quick Nav | Quick Nav | "How do I...?" | **Customized** for dev workflows |
| §0 | §0 | Working Style & Structure | Keep all |
| §1 | §1 | Configuration & Secrets | Keep all |
| §2 | §2 | Flox Basics | Keep all |
| §3 | §3 | Core Commands | **Focus**: `flox init`, `flox search`, `flox install`, `flox activate` |
| §4 | §4 | Manifest Structure | Keep all |
| §4b | §4b | Common Pitfalls | Keep all |
| §5 | §5 | The [install] Section | Keep all |
| §6 | §6 | Best Practices | Keep all |
| §7 | §7 | Editing Manifests Non-Interactively | Keep all |
| §8 | **§8** | Services | **CORE CONTENT** — databases, web servers |
| §9 | ❌ EXCLUDE | Build System | Separate branch: building-and-packaging-with-flox |
| §10 | ❌ EXCLUDE | Nix Expression Builds | Separate branch: building-and-packaging-with-flox |
| §11 | ❌ EXCLUDE | Publishing | Separate branch: building-and-packaging-with-flox |
| §12 | **§9** | Layering vs Composition | Keep all — renumber to §9 |
| §13 | ❌ EXCLUDE | Containerization | Separate branch: flox-and-containers |
| §14 | ❌ EXCLUDE | CI/CD Integration | Separate branch: ops-with-flox |
| §15 | ❌ EXCLUDE | Kubernetes Deployment | Separate branch: flox-and-k8s |
| §16 | §10 | Environment Variable Convention | Keep all |
| §17 | §11 | Quick Tips | Keep all |
| §18a | **§12a** | Python Virtual Environments | **CORE CONTENT** |
| §18b | **§12b** | C/C++ Development | **CORE CONTENT** |
| §18c | **§12c** | Node.js Development | **CORE CONTENT** |
| §18d | **§12d** | CUDA Development | **CORE CONTENT** |
| §19 | §13 | Platform-Specific Pattern | Keep all |

### Quick Navigation Customization

```markdown
## Quick Navigation Guide - "How do I...?"

### Getting Started (CORE)
- **Create my first environment** → §2 (Flox Basics), §3 (Core Commands)
- **Find and install packages** → §3 (flox search/install)
- **Understand the manifest structure** → §4 (Manifest Structure)

### Language-Specific Development (CORE)
- **Set up Python with virtual environments** → §12a (Python patterns)
- **Set up C/C++ development** → §12b (C/C++ environments)
- **Set up Node.js projects** → §12c (Node.js patterns)
- **Set up CUDA/GPU development** → §12d (CUDA environments)

### Services & Background Processes (CORE)
- **Run a database or web server** → §8 (Services)
- **Make services network-accessible** → §8 (Network services pattern)
- **Debug a failing service** → §8 (Service logging pattern)

### Environment Patterns
- **Layer multiple environments** → §9 (Layering pattern)
- **Compose reusable environments** → §9 (Composition pattern)
- **Handle package conflicts** → §5 (priority/pkg-group), §11 (Conflicts tip)

### Troubleshooting
- **Fix package conflicts** → §5 (priority), §11 (Conflicts tip)
- **Debug hooks not working** → §6 (Best Practices), §0 (Working Style)
- **Fix service startup issues** → §8 (Service patterns)
```

**Estimated Line Count**: ~680 lines (43% of main)

---

## Branch 3: `ops-with-flox`

**Target Persona**: SREs, operators, platform engineers, DevOps engineers

**Core Focus**: Production services, containerization, CI/CD automation, Kubernetes deployment, operational patterns

### Section Mapping

| Main §  | Branch § | Section Title | Notes |
|---------|----------|---------------|-------|
| Quick Nav | Quick Nav | "How do I...?" | **Customized** for ops/production workflows |
| §0 | §0 | Working Style & Structure | Keep all |
| §1 | §1 | Configuration & Secrets | Keep all |
| §2 | §2 | Flox Basics | Keep all |
| §3 | §3 | Core Commands | **Focus**: `flox activate -s`, `flox containerize` |
| §4 | §4 | Manifest Structure | Keep all |
| §4b | §4b | Common Pitfalls | Keep all |
| §5 | §5 | The [install] Section | Keep all |
| §6 | §6 | Best Practices | Keep all |
| §7 | §7 | Editing Manifests Non-Interactively | Keep all |
| §8 | **§8** | Services | **CORE CONTENT** — production services, daemons |
| §9 | ❌ EXCLUDE | Build System | Ops teams consume builds, don't create |
| §10 | ❌ EXCLUDE | Nix Expression Builds | Ops teams consume builds, don't create |
| §11 | ❌ EXCLUDE | Publishing | Build teams publish, ops teams deploy |
| §12 | **§9** | Layering vs Composition | Keep for environment reuse — renumber to §9 |
| §13 | **§10** | Containerization | **CORE CONTENT** — renumber to §10 |
| §14 | **§11** | CI/CD Integration | **CORE CONTENT** — renumber to §11 |
| §15 | **§12** | Kubernetes Deployment | **CORE CONTENT** — renumber to §12 |
| §16 | §13 | Environment Variable Convention | Keep all |
| §17 | §14 | Quick Tips | Keep all |
| §18a-d | ❌ EXCLUDE | Language-Specific Patterns | Not ops-focused |
| §19 | §15 | Platform-Specific Pattern | Keep all |

### Quick Navigation Customization

```markdown
## Quick Navigation Guide - "How do I...?"

### Production Services (CORE)
- **Run production services** → §8 (Services)
- **Make services network-accessible** → §8 (Network services pattern)
- **Debug failing services** → §8 (Service logging pattern)

### Deployment & Automation (CORE)
- **Build OCI container images** → §10 (Containerization)
- **Automate with CI/CD pipelines** → §11 (CI/CD Integration)
- **Deploy imageless Kubernetes pods** → §12 (Kubernetes Deployment)
- **Test K8s locally** → §12 (Local K8s testing with kind/colima)

### Production Patterns
- **Layer environments for reuse** → §9 (Layering vs Composition)
- **Handle secrets and configuration** → §1 (Configuration & Secrets)
- **Manage environment variables** → §13 (Environment Variable Convention)

### Kubernetes Operations (CORE)
- **Install containerd shim** → §12 (Prerequisites)
- **Configure RuntimeClass** → §12 (RuntimeClass setup)
- **Pin environment generations** → §12 (Generation pinning)
- **Remediate CVEs** → §12 (CVE remediation workflow)

### Troubleshooting
- **Fix container build issues** → §10 (Troubleshooting)
- **Debug K8s pod startup** → §12 (Pod configuration)
- **Fix service failures** → §8 (Service patterns)
```

**Estimated Line Count**: ~820 lines (52% of main)

---

## Branch 4: `flox-and-cuda`

**Target Persona**: CUDA developers, ML engineers, GPU computing specialists

**Core Focus**: CUDA toolkit setup, GPU package conflicts, cross-platform GPU support, GPU-accelerated frameworks

### Section Mapping

| Main §  | Branch § | Section Title | Notes |
|---------|----------|---------------|-------|
| Quick Nav | Quick Nav | "How do I...?" | **Customized** for CUDA/GPU workflows |
| §0 | §0 | Working Style & Structure | Keep all |
| §1 | §1 | Configuration & Secrets | Keep all |
| §2 | §2 | Flox Basics | Keep all |
| §3 | §3 | Core Commands | Keep all |
| §4 | §4 | Manifest Structure | Keep all |
| §4b | §4b | Common Pitfalls | Keep all |
| §5 | **§5** | The [install] Section | **CORE** — priority, pkg-group for CUDA conflicts |
| §6 | §6 | Best Practices | Keep all |
| §7 | §7 | Editing Manifests Non-Interactively | Keep all |
| §8 | §8 | Services | Keep for GPU-accelerated services |
| §9 | **§9** | Build System | Keep for building CUDA applications |
| §10 | ❌ EXCLUDE | Nix Expression Builds | Advanced, not CUDA-specific |
| §11 | ❌ EXCLUDE | Publishing | Not CUDA-specific |
| §12 | §10 | Layering vs Composition | Keep all |
| §13 | **§11** | Containerization | Keep for GPU container images |
| §14 | ❌ EXCLUDE | CI/CD Integration | Not CUDA-specific |
| §15 | ❌ EXCLUDE | Kubernetes Deployment | Separate branch: flox-and-k8s |
| §16 | §12 | Environment Variable Convention | Keep all |
| §17 | §13 | Quick Tips | Keep all |
| §18a | ❌ EXCLUDE | Python Patterns | Could merge PyTorch/TensorFlow into §14 |
| §18b | ❌ EXCLUDE | C/C++ Patterns | Could merge CUDA C++ into §14 |
| §18c | ❌ EXCLUDE | Node.js Patterns | Not GPU-relevant |
| §18d | **§14** | CUDA Development | **CORE CONTENT** — renumber to §14 |
| §19 | **§15** | Platform-Specific Pattern | **ESSENTIAL** for macOS Metal fallback |

### Quick Navigation Customization

```markdown
## Quick Navigation Guide - "How do I...?"

### CUDA Development (CORE)
- **Set up CUDA toolkit** → §14 (CUDA Development)
- **Resolve CUDA package conflicts** → §5 (priority/pkg-group), §14
- **Support Linux + macOS** → §14 (Cross-platform GPU), §15 (Platform patterns)
- **Install cuDNN** → §14 (cuDNN integration)

### GPU Frameworks (CORE)
- **Set up PyTorch/TensorFlow** → §14 (GPU-accelerated frameworks)
- **Configure GPU compute** → §14 (CUDA toolkit patterns)
- **Handle CUDA version conflicts** → §5 (priority), §14

### Packaging & Deployment
- **Build CUDA applications** → §9 (Build System)
- **Create GPU container images** → §11 (Containerization)
- **Layer GPU environments** → §10 (Layering vs Composition)

### Troubleshooting
- **Fix CUDA package conflicts** → §5 (priority/pkg-group), §13 (Conflicts tip)
- **Debug GPU detection** → §14 (CUDA patterns)
- **Handle macOS fallback** → §15 (Platform-Specific Pattern)
```

**Estimated Line Count**: ~580 lines (37% of main)

---

## Branch 5: `flox-and-k8s`

**Target Persona**: Kubernetes developers, platform engineers, cloud-native engineers

**Core Focus**: Imageless Kubernetes deployments, local K8s testing, CI/CD GitOps, production K8s patterns

### Section Mapping

| Main §  | Branch § | Section Title | Notes |
|---------|----------|---------------|-------|
| Quick Nav | Quick Nav | "How do I...?" | **Customized** for Kubernetes workflows |
| §0 | §0 | Working Style & Structure | Keep all |
| §1 | §1 | Configuration & Secrets | **Focus**: K8s ConfigMaps/Secrets integration |
| §2 | §2 | Flox Basics | Keep all |
| §3 | §3 | Core Commands | **Focus**: `flox build`, `flox containerize`, `flox publish` |
| §4 | §4 | Manifest Structure | Keep all |
| §4b | §4b | Common Pitfalls | Keep all |
| §5 | §5 | The [install] Section | **Focus**: `runtime-packages` for K8s workloads |
| §6 | §6 | Best Practices | Keep all |
| §7 | §7 | Editing Manifests Non-Interactively | Keep all |
| §8 | §8 | Services | Keep to understand services before K8s pods |
| §9 | **§9** | Build System | Keep for building K8s workloads |
| §10 | ❌ EXCLUDE | Nix Expression Builds | Advanced, not K8s-specific |
| §11 | **§10** | Publishing | Keep for FloxHub generation references |
| §12 | §11 | Layering vs Composition | Keep all |
| §13 | **§12** | Containerization | **CORE** — fallback to OCI images |
| §14 | **§14** | CI/CD Integration | **CORE** — essential for K8s GitOps |
| §15 | **§13** | Kubernetes Deployment | **CORE CONTENT** — renumber to §13 |
| §16 | §15 | Environment Variable Convention | Keep all |
| §17 | §16 | Quick Tips | Keep all |
| §18a-d | ❌ EXCLUDE | Language-Specific Patterns | Separate branch: local-dev-with-flox |
| §19 | ❌ EXCLUDE | Platform-Specific Pattern | K8s is platform-agnostic |

### Quick Navigation Customization

```markdown
## Quick Navigation Guide - "How do I...?"

### Kubernetes Deployment (CORE)
- **Deploy imageless K8s pods** → §13 (Kubernetes Deployment)
- **Test K8s locally (kind/colima)** → §13 (Local K8s testing)
- **Install containerd shim** → §13 (Prerequisites)
- **Configure RuntimeClass** → §13 (RuntimeClass setup)

### Production Kubernetes (CORE)
- **Pin environment generations** → §13 (Generation pinning)
- **Remediate CVEs in K8s** → §13 (CVE remediation workflow)
- **Deploy to managed K8s (EKS/GKE/AKS)** → §13 (Managed K8s considerations)
- **Integrate with GitOps** → §14 (CI/CD Integration), §13 (K8s GitOps patterns)

### Build & Publish
- **Build K8s workloads** → §9 (Build System)
- **Publish to FloxHub** → §10 (Publishing)
- **Fallback to OCI images** → §12 (Containerization)

### Environment Patterns
- **Layer K8s environments** → §11 (Layering vs Composition)
- **Handle K8s secrets** → §1 (Configuration & Secrets)
- **Manage environment variables** → §15 (Environment Variable Convention)

### Troubleshooting
- **Debug pod startup** → §13 (Pod configuration)
- **Fix containerd shim issues** → §13 (Prerequisites)
- **Verify K8s cluster config** → §13 (RuntimeClass setup)
```

**Estimated Line Count**: ~870 lines (55% of main)

---

## Branch 6: `flox-and-containers`

**Target Persona**: Container engineers, Docker/Podman users, OCI image maintainers

**Core Focus**: OCI image creation, registry workflows, container configuration, multi-stage builds

### Section Mapping

| Main §  | Branch § | Section Title | Notes |
|---------|----------|---------------|-------|
| Quick Nav | Quick Nav | "How do I...?" | **Customized** for container workflows |
| §0 | §0 | Working Style & Structure | Keep all |
| §1 | §1 | Configuration & Secrets | **Focus**: Container runtime config |
| §2 | §2 | Flox Basics | Keep all |
| §3 | §3 | Core Commands | **Focus**: `flox build`, `flox containerize`, `flox publish` |
| §4 | §4 | Manifest Structure | Keep all |
| §4b | §4b | Common Pitfalls | Keep all |
| §5 | §5 | The [install] Section | **Focus**: `runtime-packages` for containers |
| §6 | §6 | Best Practices | Keep all |
| §7 | §7 | Editing Manifests Non-Interactively | Keep all |
| §8 | §8 | Services | Keep to understand services in containers |
| §9 | **§9** | Build System | **CORE** — what goes into containers |
| §10 | ❌ EXCLUDE | Nix Expression Builds | Advanced, not container-specific |
| §11 | **§10** | Publishing | Keep for registry workflows |
| §12 | §11 | Layering vs Composition | Keep all |
| §13 | **§12** | Containerization | **CORE CONTENT** — renumber to §12 |
| §14 | **§13** | CI/CD Integration | Keep for automated image builds |
| §15 | ❌ EXCLUDE | Kubernetes Deployment | Separate branch: flox-and-k8s |
| §16 | §14 | Environment Variable Convention | Keep all |
| §17 | §15 | Quick Tips | Keep all |
| §18a-d | ❌ EXCLUDE | Language-Specific Patterns | Separate branch: local-dev-with-flox |
| §19 | **§16** | Platform-Specific Pattern | **ESSENTIAL** for macOS Docker/Podman |

### Quick Navigation Customization

```markdown
## Quick Navigation Guide - "How do I...?"

### Container Creation (CORE)
- **Build OCI container images** → §12 (Containerization)
- **Configure container users** → §12 (User field auto-creation)
- **Expose ports** → §12 (Manifest configuration)
- **Set default commands** → §12 (cmd configuration)

### Container Patterns (CORE)
- **Create multi-stage builds** → §12 (Multi-stage patterns)
- **Push to registries** → §12 (Registry workflows)
- **Handle macOS builds** → §12 (Platform-specific builds), §16 (Platform patterns)
- **Clean up Docker/Podman volumes** → §12 (Volume cleanup)

### Building for Containers
- **Package applications** → §9 (Build System)
- **Minimize runtime dependencies** → §9.6 (Trimming Dependencies)
- **Publish to catalog** → §10 (Publishing)

### Automation
- **Automate image builds** → §13 (CI/CD Integration)
- **Layer environments** → §11 (Layering vs Composition)

### Troubleshooting (CORE)
- **Fix container build failures** → §12 (Troubleshooting)
- **Debug OCI annotations** → §12 (labels, stop-signal requirements)
- **Fix macOS volume issues** → §12 (Volume cleanup), §16 (Platform patterns)
```

**Estimated Line Count**: ~720 lines (46% of main)

---

## Summary Statistics

| Branch | Target Persona | Est. Lines | % of Main | Core Sections |
|--------|---------------|------------|-----------|---------------|
| `main` | Complete reference | 1,566 | 100% | All §0-§19 |
| `building-and-packaging-with-flox` | Build engineers | ~950 | 60% | §9 Build, §10 Nix, §11 Publishing, §13 Containerization, §14 CI/CD |
| `local-dev-with-flox` | Developers, data scientists | ~680 | 43% | §8 Services, §18a-d Language patterns |
| `ops-with-flox` | SREs, operators | ~820 | 52% | §8 Services, §13 Containerization, §14 CI/CD, §15 Kubernetes |
| `flox-and-cuda` | CUDA developers | ~580 | 37% | §5 [install], §18d CUDA, §19 Platform patterns |
| `flox-and-k8s` | Kubernetes engineers | ~870 | 55% | §15 Kubernetes, §13 Containerization, §14 CI/CD |
| `flox-and-containers` | Container engineers | ~720 | 46% | §13 Containerization, §9 Build System, §14 CI/CD |

---

## Cross-Reference Considerations

Each branch will need section cross-references updated to match the renumbered structure:

**Example**: In `building-and-packaging-with-flox` branch:
- References to "§9 Build System" in main → remain "§8 Build System" in branch
- References to "§13 Containerization" in main → become "§12 Containerization" in branch

**Recommendation**: After creating each branch's FLOX.md, run automated cross-reference validation to ensure all § references point to existing sections.

---

## Implementation Checklist

For each branch:

1. ✅ **Map content** (DONE in this report)
2. ⬜ **Create curated FLOX.md**
   - Copy relevant sections from main
   - Renumber sections per mapping
   - Customize Quick Navigation
3. ⬜ **Update cross-references**
   - Search for all `§N` references
   - Update to match new section numbers
4. ⬜ **Validate**
   - Verify all § cross-references resolve
   - Check Quick Navigation links
   - Confirm no orphaned references
5. ⬜ **Create branch**
   - `git checkout -b <branch-name>`
   - `git add FLOX.md`
   - `git commit -m "Create <persona> documentation view"`
   - `git push origin <branch-name>`

---

## Next Steps

1. **Review this mapping** with stakeholders
2. **Choose first branch** to implement (suggest: `local-dev-with-flox` as simplest)
3. **Create pilot branch** and validate workflow
4. **Iterate** on remaining 5 branches
5. **Update README.md** in main to explain branch strategy

---

**Report Generated**: 2025-11-11
**Main FLOX.md Version**: 1,566 lines (§0-§19)
**Number of Branches**: 6 + main = 7 total
