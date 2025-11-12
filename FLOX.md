# Flox Environment Creation Quick Guide

## Quick Navigation Guide - "How do I...?"

### Getting Started
- **Create my first environment** → §2 (Flox Basics), §3 (Core Commands)
- **Find and install packages** → §3 (flox search/install), §5 (install section details)
- **Understand the manifest structure** → §4 (Manifest Structure)

### Language-Specific Development
- **Set up Python with virtual environments** → §12a (Python patterns)
- **Set up C/C++ development** → §12b (C/C++ environments)
- **Set up Node.js projects** → §12c (Node.js patterns)
- **Set up CUDA/GPU development** → §12d (CUDA environments)

### Services & Background Processes
- **Run a database or web server** → §8 (Services)
- **Make services network-accessible** → §8 (Network services pattern)
- **Debug a failing service** → §8 (Service logging pattern)

### Environment Patterns
- **Layer multiple environments** → §9 (Layering pattern)
- **Compose reusable environments** → §9 (Composition pattern)
- **Design environments for both** → §9 (Dual-purpose environments)
- **Handle package conflicts** → §5 (priority/pkg-group), §11 (Quick Tips)

### Platform-Specific
- **Handle Linux-only packages** → §5 (systems attribute), §12d (CUDA)
- **Handle macOS-specific frameworks** → §13 (Platform-Specific Pattern)
- **Support multiple platforms** → §12d (Cross-platform GPU), §13 (Platform patterns)

### Troubleshooting
- **Fix package conflicts** → §5 (priority), §11 (Conflicts tip)
- **Debug hooks not working** → §6 (Best Practices), §0 (Working Style)
- **Fix service startup issues** → §8 (Service patterns)

### Advanced Topics
- **Edit manifests programmatically** → §7 (Non-Interactive Editing)
- **Use environment variable conventions** → §10 (Environment Variables)

### Anti-Patterns to Avoid
- **Common pitfalls** → §4b (Common Pitfalls)

## 0 Working Style & Structure
- Use **modular, idempotent bash functions** in hooks
- Never, ever use absolute paths. Flox environments are designed to be reproducible. Use Flox's environment variables (see §2, "Flox Basics") instead
- I REPEAT: NEVER, EVER USE ABSOLUTE PATHS. Don't do it. Use `$FLOX_ENV` for environment-specific runtime dependencies; use `$FLOX_ENV_PROJECT` for the project directory. See §2 (Flox Basics)
- Name functions descriptively (e.g., `setup_postgres()`)
- Consider using **gum** for styled output when creating environments for interactive use; this is an anti-pattern in CI
- Put persistent data/configs in `$FLOX_ENV_CACHE`
- Return to `$FLOX_ENV_PROJECT` at end of hooks
- Use `mktemp` for temp files, clean up immediately
- Do not over-engineer: e.g., do not create unncessary echo statements or superfluous comments; do not print unnecessary information displays in `[hook]` or `[profile]`; do not create helper functions or aliases without the user requesting these explicitly.

## 1 Configuration & Secrets
- Support `VARIABLE=value flox activate` pattern for runtime overrides
- Never store secrets in manifest; use:
  - Environment variables
  - `~/.config/<env_name>/` for persistent secrets
  - Existing config files (e.g., `~/.aws/credentials`)

## 2 Flox Basics
- Flox is built on Nix; fully Nix-compatible
- Flox uses nixpkgs as its upstream; packages are _usually_ named the same; unlike nixpkgs, FLox Catalog has millions of historical package-version combinations.
- Key paths:
  - `.flox/env/manifest.toml`: Environment definition
  - `.flox/env.json`: Environment metadata
  - `$FLOX_ENV_CACHE`: Persistent, local-only storage (survives `flox delete`)
  - `$FLOX_ENV_PROJECT`: Project root directory (where .flox/ lives)
  - `$FLOX_ENV`: basically the path to `/usr`: contains all the libs, includes, bins, configs, etc. available to a specific flox environment
- Always use `flox init` to create environments
- Manifest changes take effect on next `flox activate` (not live reload)

## 3 Core Commands
```bash
flox init                       # Create new env
flox search <string> [--all]    # Search for a package
flox show <pkg>                 # Show available historical versions of a package
flox install <pkg>              # Add package
flox list [-e | -c | -n | -a]   # List installed packages: `-e` = default; `-c` = shows the raw contents of the manifest; `-n` = shows only the install ID of each package; `-a` = shows all available package information including priority and license.
flox activate                   # Enter env
flox activate -s                # Start services
flox activate -- <cmd>          # Run without subshell
flox build <target>             # Build defined target
flox containerize               # Export as OCI image
```

## 4 Manifest Structure
- `[install]`: Package list with descriptors (see detailed section below)
- `[vars]`: Static variables
- `[hook]`: Non-interactive setup scripts
- `[profile]`: Shell-specific functions/aliases
- `[services]`: Service definitions with commands and optional shutdown
- `[build]`: Reproducible build commands
- `[include]`: Compose other environments
- `[options]`: Activation mode, supported systems

## 4b Common Pitfalls
- Hooks run EVERY activation (keep them fast/idempotent)
- Hook functions are not available to users in the interactive shell; use `[profile]` for user-invokable commands/aliases
- Profile code runs for each layered/composed environment; keep auto-run display logic in `[hook]` to avoid repetition
- Services see fresh environment (no preserved state between restarts)
- Build commands can't access network in pure mode (pre-fetch deps)
- Manifest syntax errors prevent ALL flox commands from working
- Package search is case-sensitive; use `flox search --all` for broader results

## 5 The [install] Section

### Package Installation Basics
The `[install]` table specifies packages to install.

```toml
[install]
ripgrep.pkg-path = "ripgrep"
pip.pkg-path = "python310Packages.pip"
```

### Package Descriptors
Each entry has:
- **Key**: Install ID (e.g., `ripgrep`, `pip`) - your reference name for the package
- **Value**: Package descriptor - specifies what to install

### Catalog Descriptors (Most Common)
Options for packages from the Flox catalog:

```toml
[install]
example.pkg-path = "package-name"           # Required: location in catalog
example.pkg-group = "mygroup"               # Optional: group packages together
example.version = "1.2.3"                   # Optional: exact or semver range
example.systems = ["x86_64-linux"]          # Optional: limit to specific platforms;
example.priority = 3                        # Optional: resolve file conflicts (lower = higher priority)
```

#### Key Options Explained:

**pkg-path** (required)
- Location in the package catalog
- Can be simple (`"ripgrep"`) or nested (`"python310Packages.pip"`)
- Can use array format: `["python310Packages", "pip"]`

**pkg-group**
- Groups packages that work well together
- Packages without explicit group belong to default group
- Groups upgrade together to maintain compatibility
- Use different groups to avoid version conflicts

**version**
- Exact: `"1.2.3"`
- Semver ranges: `"^1.2"`, `">=2.0"`
- Partial versions act as wildcards: `"1.2"` = latest 1.2.X

**systems**
- Constrains package to specific platforms
- Options: `"x86_64-linux"`, `"x86_64-darwin"`, `"aarch64-linux"`, `"aarch64-darwin"`
- Defaults to manifest's `options.systems` if omitted

**priority**
- Resolves file conflicts between packages
- Default: 5
- Lower number = higher priority wins conflicts
- **Critical for CUDA packages** (see §18d)


### Practical Examples

```toml
# Platform-specific Python
[install]
python.pkg-path = "python311Full"
uv.pkg-path = "uv" # installs uv, modern rust-based successor to uvicorn
systems = ["x86_64-linux", "aarch64-linux"]  # Linux only

# Version-pinned with custom priority
[nodejs]
nodejs.pkg-path = "nodejs"
version = "^20.0"
priority = 1  # Takes precedence in conflicts

# Multiple package groups to avoid conflicts
[install]
gcc.pkg-path = "gcc12"
gcc.pkg-group = "stable"
```

## 6 Best Practices
- Check manifest before installing new packages
- Use `return` not `exit` in hooks
- Define env vars with `${VAR:-default}`
- Use descriptive, prefixed function names in composed envs
- Cache downloads in `$FLOX_ENV_CACHE`
- Log service output to `$FLOX_ENV_CACHE/logs/`
- Test activation with `flox activate -- <command>` before adding to services
- When debugging services, run the exact command from manifest manually first
- Use `--quiet` flag with uv/pip in hooks to reduce noise

## 7 Editing Manifests Non-Interactively
```bash
flox list -c > /tmp/manifest.toml
# Edit with sed/awk
flox edit -f /tmp/manifest.toml
```

## 8 Services
- Start with `flox activate --start-services` or `flox activate -s`
- Define `is-daemon`, `shutdown.command` for background processes
- Keep services running using `tail -f /dev/null`
- Use `flox services status/logs/restart` to manage (must be in activated env)
- Service commands don't inherit hook activations; explicitly source/activate what you need
- **Network services pattern**: Always make host/port configurable via vars:
  ```toml
  [services.webapp]
  command = '''exec app --host "$APP_HOST" --port "$APP_PORT"'''
  vars.APP_HOST = "0.0.0.0"  # Network-accessible
  vars.APP_PORT = "8080"
  ```
- **Service logging**: Always pipe to `$FLOX_ENV_CACHE/logs/` for debugging:
  ```toml
  command = '''exec app 2>&1 | tee -a "$FLOX_ENV_CACHE/logs/app.log"'''
  ```
- **Python venv pattern**: Services must activate venv independently:
  ```toml
  command = '''
    [ -f "$FLOX_ENV_CACHE/venv/bin/activate" ] && \
      source "$FLOX_ENV_CACHE/venv/bin/activate"
    exec python-app "$@"
  '''
  ```
- **Using packaged services**: Override package's service by redefining with same name
- Example:
```toml
[services.database]
command = "postgres start"
vars.PGUSER = "myuser"
vars.PGPASSWORD = "super-secret"
vars.PGDATABASE = "mydb"
vars.PGPORT = "9001"
```


## 9 Layering vs Composition - Environment Design Guide

| Aspect     | Layering                          | Composition                     |
|------------|-----------------------------------|---------------------------------|
| When       | Runtime (activate order matters) | Build time (deterministic)     |
| Conflicts  | Surface at runtime                | Surface at build time          |
| Flexibility| High                              | Predefined structure           |
| Use case   | Ad hoc tools/services            | Repeatable, shareable stacks   |
| Isolation  | Preserves subshell boundaries    | Merges into single manifest    |

### Creating Layer-Optimized Environments
**Design for runtime stacking with potential conflicts:**
```toml
[vars]
# Prefix vars to avoid masking
MYAPP_PORT = "8080"
MYAPP_HOST = "localhost"

[profile.common]
# Use unique, prefixed function names
myapp_setup() { ... }
myapp_debug() { ... }

[services.myapp-db]  # Prefix service names
command = "..."
```
**Best practices:**
- Single responsibility per environment
- Expect vars/binaries might be overridden by upper layers
- Document what the environment provides/expects
- Keep hooks fast and idempotent

**CUDA layering example:** Layer debugging tools (`flox activate -r team/cuda-debugging`) on base CUDA environment for ad-hoc development (see §18d).

### Creating Composition-Optimized Environments
**Design for clean merging at build time:**
```toml
[install]
# Use pkg-groups to prevent conflicts
gcc.pkg-path = "gcc"
gcc.pkg-group = "compiler"

[vars]
# Never duplicate var names across composed envs
POSTGRES_PORT = "5432"  # Not "PORT"

[hook]
# Check if setup already done (idempotent)
setup_postgres() {
  [ -d "$FLOX_ENV_CACHE/postgres" ] || init_db
}
```
**Best practices:**
- No overlapping vars, services, or function names
- Use explicit, namespaced naming (e.g., `postgres_init` not `init`)
- Minimal hook logic (composed envs run ALL hooks)
- Avoid auto-run logic in `[profile]` (runs once per layer/composition; help displays will repeat); see §4b
- Test composability: `flox activate` each env standalone first

**CUDA composition example:** Compose base CUDA, math libraries, and ML frameworks into reproducible stack:
```toml
[include]
environments = [
    { remote = "team/cuda-base" },
    { remote = "team/cuda-math" },
    { remote = "team/python-ml" }
]
```

### Creating Dual-Purpose Environments
**Design for both patterns:**
```toml
[install]
# Clear package groups
python.pkg-path = "python311"
python.pkg-group = "runtime"

[vars]
# Namespace everything
MYPROJECT_VERSION = "1.0"
MYPROJECT_CONFIG = "$FLOX_ENV_CACHE/config"

[profile.common]
# Defensive function definitions
if ! type myproject_init >/dev/null 2>&1; then
  myproject_init() { ... }
fi
```

### Usage Examples
- **Layer**: `flox activate -r team/postgres -- flox activate -r team/debug`
- **Compose**: `[include] environments = [{ remote = "team/postgres" }]`
- **Both**: Compose base, layer tools on top


## 10 Environment Variable Convention Example

- Use variables like `POSTGRES_HOST`, `POSTGRES_PORT` to define where services run.
- These store connection details *separately*:
  - `*_HOST` is the hostname or IP address (e.g., `localhost`, `db.example.com`).
  - `*_PORT` is the network port number (e.g., `5432`, `6379`).
- This pattern ensures users can override them at runtime:
  ```bash
  POSTGRES_HOST=db.internal POSTGRES_PORT=6543 flox activate
  ```
- Use consistent naming across services so the meaning is clear to any system or person reading the variables.


## 11 Quick Tips for [install] Section
- **Tricky Dependencies**: If we need `libstdc++`, we get this from the `gcc-unwrapped` package, not from `gcc`; if we need to have both in the same environment, we use either package groups or assign priorities. (See **`Conflicts`**, below); also, if user is working with python and requests `uv`, they typically do not mean `uvicorn`; clarify which package user wants.
- **Conflicts**: If packages conflict, use different `pkg-group` values or adjust `priority`. **CUDA packages require explicit priorities** (see §18d).
- **Versions**: Start loose (`"^1.0"`), tighten if needed (`"1.2.3"`)
- **Platforms**: Only restrict `systems` when package is platform-specific. **CUDA is Linux-only**: `["aarch64-linux", "x86_64-linux"]`
- **Naming**: Install ID can differ from pkg-path (e.g., `gcc.pkg-path = "gcc13"`)
- **Search**: Use `flox search` to find correct pkg-paths before installing


## 12 Language-Specific Dev Patterns

## 12a Python and Python Virtual Environments
  - **venv creation pattern**: Always check existence before activation - `uv venv` may not complete synchronously:
    ```bash
    if [ ! -d "$venv" ]; then
      uv venv "$venv" --python python3
    fi
    # Guard activation - venv creation might not be complete
    if [ -f "$venv/bin/activate" ]; then
      source "$venv/bin/activate"
    fi
	```
  **venv location**: Always use $FLOX_ENV_CACHE/venv - survives environment rebuilds
  **uv with venv**: Use `uv pip install --python "$venv/bin/python"` NOT `"$venv/bin/python" -m uv`
  **Service commands**: Use venv Python directly: $FLOX_ENV_CACHE/venv/bin/python not python
- **Activation**: Always `source "$venv/bin/activate"` before pip/uv operations
- **PyTorch CUDA**: Install with `--index-url https://download.pytorch.org/whl/cu124` for GPU support (see §18d)
- **PyTorch gotcha**: Needs `gcc-unwrapped` for libstdc++.so.6, not just `gcc`
- **PyTorch CPU/GPU**: Use separate index URLs: `/whl/cpu` vs `/whl/cu124` (don't mix!)
- **Service scripts**: Must activate venv inside service command, not rely on hook activation
- **Cache dirs**: Set `UV_CACHE_DIR` and `PIP_CACHE_DIR` to `$FLOX_ENV_CACHE` subdirs
- **Dependency installation flag**: Touch `$FLOX_ENV_CACHE/.deps_installed` to prevent reinstalls
- **Service venv pattern**: Always use absolute paths and explicit activation in service commands:
  ```toml
  [services.myapp]
  command = '''
  source "$FLOX_ENV_CACHE/venv/bin/activate"
  exec "$FLOX_ENV_CACHE/venv/bin/python" app.py
  '''
  ```
- **Using Python packages from catalog**: Override data dirs to use local paths:
  ```toml
  [install]
  myapp.pkg-path = "owner/myapp"
  [vars]
  MYAPP_DATA = "$FLOX_ENV_PROJECT"  # Use repo not ~/.myapp
  ```
- **Wrapping package commands**: Alias to customize behavior:
  ```bash
  # In [profile]
  alias myapp-setup="MYAPP_DATA=$FLOX_ENV_PROJECT command myapp-setup"
  ```

**Note**: `uv` is installed in the Flox environment, not inside the venv. We use `uv pip install --python "$venv/bin/python"` so that `uv` targets the venv's Python interpreter.

## 12b C/C++ Development Environments
- **Package Names**: `gbenchmark` not `benchmark`, `catch2_3` for Catch2, `gcc13`/`clang_18` for specific versions
- **System Constraints**: Linux-only tools need explicit systems: `valgrind.systems = ["x86_64-linux", "aarch64-linux"]`
- **Essential Groups**: Separate `compilers`, `build`, `debug`, `testing`, `libraries` groups prevent conflicts
- **Core Stack**: gcc13/clang_18, cmake/ninja/make, gdb/lldb, boost/eigen/fmt/spdlog, gtest/catch2/gbenchmark
- **libstdc++ Access**: ALWAYS include `gcc-unwrapped` for C++ stdlib headers/libs (gcc alone doesn't expose them):
```toml
gcc-unwrapped.pkg-path = "gcc-unwrapped"
gcc-unwrapped.priority = 5  # Lower priority to avoid conflicts
gcc-unwrapped.pkg-group = "libraries"
```

## 12c Node.js Development Environments
- **Package managers**: Install `nodejs` (includes npm); add `yarn` or `pnpm` separately if needed
- **Version pinning**: Use `version = "^20.0"` for LTS, or exact versions for reproducibility
- **Global tools pattern**: Use `npx` for one-off tools, install commonly-used globals in manifest
- **Service pattern**: Always specify host/port for network services:
  ```toml
  [services.dev-server]
  command = '''exec npm run dev -- --host "$DEV_HOST" --port "$DEV_PORT"'''
  ```

## 12d CUDA Development Environments

### Prerequisites & Authentication
- Sign up for early access at https://flox.dev, authenticate with `flox auth login`
- **Linux-only**: CUDA packages only work on `["aarch64-linux", "x86_64-linux"]`
- All CUDA packages are prefixed with `flox-cuda/` in the catalog

### Package Discovery
```bash
flox search cudatoolkit --all | grep flox-cuda
flox search nvcc --all | grep 12_8              # Specific versions
flox show flox-cuda/cudaPackages.cudatoolkit    # All available versions
```

### Essential CUDA Packages
| Package Pattern | Purpose | Example |
|-----------------|---------|---------|
| `cudaPackages_X_Y.cudatoolkit` | Main CUDA Toolkit | `cudaPackages_12_8.cudatoolkit` |
| `cudaPackages_X_Y.cuda_nvcc` | NVIDIA C++ Compiler | `cudaPackages_12_8.cuda_nvcc` |
| `cudaPackages.cuda_cudart` | CUDA Runtime API | `cuda_cudart` |
| `cudaPackages_X_Y.libcublas` | Linear algebra | `cudaPackages_12_8.libcublas` |
| `cudaPackages_X_Y.cudnn_9_11` | Deep neural networks | `cudaPackages_12_8.cudnn_9_11` |

### Critical: Conflict Resolution
**CUDA packages have LICENSE file conflicts requiring explicit priorities:**
```toml
[install]
cuda_nvcc.pkg-path = "flox-cuda/cudaPackages_12_8.cuda_nvcc"
cuda_nvcc.systems = ["aarch64-linux", "x86_64-linux"]
cuda_nvcc.priority = 1                    # Highest priority

cuda_cudart.pkg-path = "flox-cuda/cudaPackages.cuda_cudart"
cuda_cudart.systems = ["aarch64-linux", "x86_64-linux"]
cuda_cudart.priority = 2

cudatoolkit.pkg-path = "flox-cuda/cudaPackages_12_8.cudatoolkit"
cudatoolkit.systems = ["aarch64-linux", "x86_64-linux"]
cudatoolkit.priority = 3                  # Lower for LICENSE conflicts

gcc.pkg-path = "gcc"
gcc-unwrapped.pkg-path = "gcc-unwrapped"  # For libstdc++
gcc-unwrapped.priority = 5
```

### Cross-Platform GPU Development
Dual CUDA/CPU packages for portability (Linux gets CUDA, macOS gets CPU fallback):
```toml
[install]
## CUDA packages (Linux only)
cuda-pytorch.pkg-path = "flox-cuda/python3Packages.torch"
cuda-pytorch.systems = ["x86_64-linux", "aarch64-linux"]
cuda-pytorch.priority = 1

## Non-CUDA packages (macOS + Linux fallback)
pytorch.pkg-path = "python313Packages.pytorch"
pytorch.systems = ["x86_64-darwin", "aarch64-darwin"]
pytorch.priority = 6                     # Lower priority
```

### GPU Detection Pattern
**Dynamic CPU/GPU package installation in hooks:**
```bash
setup_gpu_packages() {
  venv="$FLOX_ENV_CACHE/venv"
  
  if [ ! -f "$FLOX_ENV_CACHE/.deps_installed" ]; then
    if lspci 2>/dev/null | grep -E 'NVIDIA|AMD' > /dev/null; then
      echo "GPU detected, installing CUDA packages"
      uv pip install --python "$venv/bin/python" \
        torch torchvision --index-url https://download.pytorch.org/whl/cu129
    else
      echo "No GPU detected, installing CPU packages"
      uv pip install --python "$venv/bin/python" \
        torch torchvision --index-url https://download.pytorch.org/whl/cpu
    fi
    touch "$FLOX_ENV_CACHE/.deps_installed"
  fi
}
```

### Best Practices
- **Always use priority values**: CUDA packages have predictable conflicts
- **Version consistency**: Use specific versions (e.g., `_12_8`) for reproducibility
- **Modular design**: Split base CUDA, math libs, debugging into separate environments
- **Test compilation**: Verify `nvcc hello.cu -o hello` works after setup
- **Platform constraints**: Always include `systems = ["aarch64-linux", "x86_64-linux"]`

### Common CUDA Gotchas
- **CUDA toolkit ≠ complete toolkit**: Add libraries (libcublas, cudnn) as needed
- **License conflicts**: Every CUDA package may need explicit priority
- **No macOS support**: Use Metal alternatives on Darwin
- **Version mixing**: Don't mix CUDA versions; use consistent `_X_Y` suffixes

### Complete Example
```toml
[install]
cuda_nvcc.pkg-path = "flox-cuda/cudaPackages_12_8.cuda_nvcc"
cuda_nvcc.priority = 1
cuda_cudart.pkg-path = "flox-cuda/cudaPackages.cuda_cudart"
cuda_cudart.priority = 2
libcublas.pkg-path = "flox-cuda/cudaPackages.libcublas"
torch.pkg-path = "flox-cuda/python3Packages.torch"
python313Full.pkg-path = "python313Full"
uv.pkg-path = "uv"
gcc.pkg-path = "gcc"
gcc-unwrapped.pkg-path = "gcc-unwrapped"
gcc-unwrapped.priority = 5

[vars]
CUDA_VERSION = "12.8"
PYTORCH_CUDA_ALLOC_CONF = "max_split_size_mb:128"

[hook]
setup_cuda_venv() {
  venv="$FLOX_ENV_CACHE/venv"
  [ ! -d "$venv" ] && uv venv "$venv" --python python3
  [ -f "$venv/bin/activate" ] && source "$venv/bin/activate"
}
```


## 13 **Platform-Specific Pattern**:
```toml
# Darwin-specific frameworks and tools
IOKit.pkg-path = "darwin.apple_sdk.frameworks.IOKit"
IOKit.systems = ["x86_64-darwin", "aarch64-darwin"]
CoreFoundation.pkg-path = "darwin.apple_sdk.frameworks.CoreFoundation"
CoreFoundation.priority = 2
CoreFoundation.systems = ["x86_64-darwin", "aarch64-darwin"]

# Platform-preferred compilers (remove constraints if cross-platform needed)
gcc.pkg-path = "gcc"
gcc.systems = ["x86_64-linux", "aarch64-linux"]
clang.pkg-path = "clang" 
clang.systems = ["x86_64-darwin", "aarch64-darwin"]

# Darwin GNU compatibility layer (Darwin's built-ins are ancient/limited)
coreutils.pkg-path = "coreutils"
coreutils.systems = ["x86_64-darwin", "aarch64-darwin"]
gnumake.pkg-path = "gnumake"
gnumake.systems = ["x86_64-darwin", "aarch64-darwin"] 
gnused.pkg-path = "gnused"
gnused.systems = ["x86_64-darwin", "aarch64-darwin"]
gawk.pkg-path = "gawk"
gawk.systems = ["x86_64-darwin", "aarch64-darwin"]
bashInteractive.pkg-path = "bashInteractive"
bashInteractive.systems = ["x86_64-darwin", "aarch64-darwin"]
```

**Note**: CUDA is Linux-only (see §18d); use Metal-accelerated packages on Darwin when available.
