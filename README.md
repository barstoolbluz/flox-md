# Flox Documentation Package (flox-md)

A portable, installable package containing the comprehensive Flox environment creation guide (FLOX.md) with convenient reader tools.

## What is this?

This package turns the FLOX.md documentation into an installable Flox package with:
- Beautiful terminal rendering with `glow`
- Fast searching with `ripgrep`
- Interactive section browsing with `fzf`
- Quick reference card
- Section shortcuts for common topics

## Installation

Once published, users can install the documentation:

```bash
flox install barstoolbluz/flox-md
```

## Usage

After installation, these commands become available:

### Read the Full Guide
```bash
flox-md                    # Beautiful rendering with glow
flox-md --raw              # Raw markdown
flox-md --bat              # Syntax highlighted
flox-md --less             # Simple pager
```

### Jump to Sections
```bash
flox-md 9                  # Jump to Build System section
flox-md python             # Python patterns (section 16a)
flox-md cuda               # CUDA patterns (section 16c)
flox-md build              # Build system details
flox-md service            # Service configuration
```

### Search Documentation
```bash
flox-search venv           # Search for "venv"
flox-search "pkg-path"     # Search for "pkg-path"
flox-md --search hook      # Alternative search method
```

### Browse Interactively
```bash
flox-browse                # Interactive section browser with fzf
flox-md --list             # List all sections
```

### Quick Reference
```bash
flox-quick                 # Display quick reference card
```

## Building and Publishing

### Build the Package

```bash
cd flox-md
flox activate
flox build flox-md
```

### Test Locally

```bash
./result-flox-md/bin/flox-md --help
./result-flox-md/bin/flox-quick
```

### Publish to Flox Catalog

```bash
# Ensure you're logged in
flox auth login

# Commit and push changes
git add .
git commit -m "Update flox-md documentation package"
git push origin main

# Publish to personal namespace
flox publish flox-md

# Or publish to organization
flox publish -o <org-name> flox-md
```

## Package Contents

The package includes:
- `/bin/flox-md` - Main documentation reader with multiple viewing modes
- `/bin/flox-search` - Search tool for finding content
- `/bin/flox-browse` - Interactive section browser
- `/bin/flox-quick` - Quick reference card
- `/share/doc/flox-md/FLOX.md` - The documentation itself

## Features

- **Multiple Viewers**: Choose between glow, bat, or less for reading
- **Smart Navigation**: Jump directly to sections by number or topic
- **Fast Search**: Ripgrep-powered searching with context
- **Interactive Browse**: Use fzf to browse and preview sections
- **Offline Access**: No internet required once installed
- **Cross-Platform**: Works on Linux and macOS

## Why Package Documentation?

1. **Instant Access**: `flox-md` anywhere, anytime
2. **Version Control**: Pin specific documentation versions
3. **Offline Friendly**: No internet needed
4. **Better UX**: Purpose-built reader commands
5. **Searchable**: Fast grep through examples and patterns
6. **Portable**: Take your reference guide everywhere

## Development

To modify the package:

1. Edit `.flox/env/manifest.toml`
2. Update commands in the `[build.flox-md]` section
3. Test with `flox build flox-md`
4. Use the built commands in `./result-flox-md/bin/`

## Tips

- Install `glow` for the best reading experience
- Use `flox-browse` for exploring unfamiliar sections
- `flox-quick` is perfect for quick command lookups
- Search supports regex: `flox-search 'pkg.*path'`

## Contributing

To improve the documentation or package:
1. Fork this repository
2. Make your changes
3. Test thoroughly
4. Submit a pull request

## License

The documentation and packaging are provided under the same terms as Flox itself.