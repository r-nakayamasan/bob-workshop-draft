# AGENTS.md

This file provides guidance to agents when working with code in this repository.

## Project Overview
MkDocs Material-based documentation site for IBM Bob workshop tutorials (Japanese language).

## Build & Run Commands

**Development server:**
```bash
uv run mkdocs serve
# or using Python module syntax:
uv run python -m mkdocs serve
```
Access at: http://127.0.0.1:8000

**Build static site:**
```bash
uv run mkdocs build
```
Output: `site/` directory

**Deploy to GitHub Pages:**
```bash
uv run mkdocs gh-deploy
```

## Project-Specific Patterns

### Image Path Convention
- Images MUST use relative paths from docs/ directory: `../assets/images/`
- Images are organized by section: `../assets/images/01_introduction/`, `../assets/images/02_bob_interface/`, etc.
- DO NOT use absolute paths like `/assets/images/` - they will fail in GitHub Pages deployment

### Documentation Structure
- All markdown files are in `docs/` directory
- Navigation is defined in `mkdocs.yml` under `nav:` section
- File naming: Use descriptive names with section numbers (e.g., `00_preparation_and_setup.md`, `01_introduction.md`)

### MkDocs Configuration
- Site uses Material theme with Japanese language support
- IBM Plex Sans/Mono fonts are configured
- Code blocks support syntax highlighting with line numbers via pymdownx extensions
- Mermaid diagrams are supported via pymdownx.superfences

### Package Management
- Uses `uv` (not pip) for dependency management
- Python 3.12+ required
- Dependencies defined in `pyproject.toml`
- Run `uv sync` to install dependencies

## Non-Obvious Gotchas

1. **Image 404 Warnings**: The terminal shows 404 warnings for images during development - this is expected when images haven't been copied to the `site/` build directory yet. Images will work correctly when accessed through the proper navigation flow.

2. **Site URL Configuration**: The `site_url` in mkdocs.yml includes `/bob-workshop-draft/` path segment for GitHub Pages deployment. Local development uses root path.

3. **No Testing Framework**: This is a documentation-only project with no automated tests.