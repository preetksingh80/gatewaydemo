# Copilot Instructions for gatewaydemo

## Project Overview

This is a minimal vanilla JavaScript static web application designed for deployment to Azure Static Web Apps. The project intentionally avoids frameworks, bundlers, and build steps - it serves static HTML/CSS directly from the `src/` directory.

## Architecture & Structure

- **Frontend**: Single-page vanilla HTML (`src/index.html`) with basic CSS (`src/styles.css`)
- **No build process**: Files in `src/` are served directly without transpilation or bundling
- **Deployment target**: Azure Static Web Apps with two environments configured (green-water, gray-mushroom)
- **Dev environment**: Dev container configured for Azure Static Web Apps development with Node 16 and Azure Functions Core Tools v4

## Development Workflow

### Local Development

```powershell
# Start local dev server on port 8000
npm start
```

Uses `sirv` to serve `src/` directory with CORS enabled and SPA routing on port 8000.

### Dev Container

The project is containerized for consistent development. Opening in GitHub Codespaces or VS Code Remote Containers automatically sets up:

- Node.js 16
- Azure Functions Core Tools v4
- Forwarded ports: 7071 (Azure Functions), 4280 (Static Web Apps CLI)
- Pre-installed extensions: Azure Static Web Apps, Azure Functions, ESLint

### Deployment

Automated CI/CD via GitHub Actions (`.github/workflows/`):

- Triggers on push to `main` or pull requests
- Uses `Azure/static-web-apps-deploy@v1` action
- **Critical paths**: `app_location: "/src"`, `output_location: "/src"` (no build step)
- Two deployment environments with separate API tokens (configured as GitHub secrets)

## Testing

- Playwright is installed (`@playwright/test` v1.22.2) but no test files currently exist
- Test results are gitignored (`/test-results/`)
- When adding tests, place them in a `tests/` directory or as `*.spec.js` files

## Key Conventions

- **No JavaScript files**: Currently pure HTML/CSS only
- **Minimal dependencies**: Only dev dependencies (Playwright for future testing)
- **Direct serving**: Any files in `src/` are publicly accessible without processing
- **CSS approach**: Global styles in `styles.css` using simple element selectors, centered layout pattern

## Adding Features

When extending this application:

1. Add new HTML files directly to `src/` - they'll be accessible at their filename path
2. For JavaScript, add `<script>` tags in HTML or new `.js` files in `src/`
3. Keep dependencies minimal - prefer vanilla JS over frameworks
4. Respect the Azure Static Web Apps deployment model (static files in `src/`, optional API in separate directory)
5. Update workflow files if changing `app_location` or adding an `api_location`

## Azure Static Web Apps Specifics

- No API layer configured (`api_location: ""`)
- Single-page app mode enabled in `sirv` for local dev (`--single` flag)
- Repository configured for multiple deployment slots (two workflow files for different environments)
