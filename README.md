# Home Assistant Community Automation Discovery Directory

A zero-YAML, community-driven discovery directory and hardware auto-audit system for Home Assistant. It points Home Assistant users directly to upstream community automation repositories based on installed device hardware without requiring external AI services.

## Features

- **Interactive Hardware & Keyword Discovery**: Client-side filtering of automation ideas by installed hardware capabilities (motion, light, door/contact, switch/plug, media player, climate, lock, cover, power monitor, presence, sun) and real-time text search.
- **One-Click Blueprint Import**: Direct URL redirection badge to import zero-configuration auto-audit blueprints into Home Assistant instances.
- **Community-Driven JSON Catalog**: Decentralized recipe entries (`recipes.json`) and blueprint indexes (`blueprint_index/all.json`) allowing easy pull-request-based repository submissions.
- **Automated Home Assistant Device Audit**: Zero-configuration YAML blueprint (`blueprints/automation_suggester.yaml`) that inspects Home Assistant entity registries and generates persistent notifications for matching automation recipes.
- **Direct Upstream Repository Linking**: Instant navigation to community creator repositories and raw YAML blueprint source files.

## Tech Stack & Architecture

- **Frontend / Client UI**: Vanilla HTML5, CSS3 (CSS custom properties), and JavaScript (ES6+ `fetch` API, DOM manipulation).
- **Static Hosting & CDN**: GitHub Pages static website hosting.
- **Data Layer**: Flat JSON database files (`recipes.json`, `blueprint_index/all.json`).
- **Automation Engine**: Home Assistant Blueprint Schema (YAML) with Jinja2 Templating and HA Entity Registry triggers.
- **Runtime Requirements**: Any modern HTTP web browser or Home Assistant Webpage Card (Lovelace dashboard iframe integration).

## Repository Layout

```
.
├── README.md                          # Project overview, setup, and usage guide
├── API.md                             # Comprehensive API and data endpoints documentation
├── index.html                         # Interactive web application & filtering dashboard
├── recipes.json                       # Catalog of community automation recipe ideas
├── blueprints/
│   └── automation_suggester.yaml     # Home Assistant auto-audit discovery blueprint
└── blueprint_index/
    └── all.json                       # Catalog index of popular Home Assistant blueprints
```

## Prerequisites & Setup

### Prerequisites
- Node.js (v18+) or Python 3.8+ (for local HTTP preview server)
- Git for version control
- Home Assistant 2023.8+ (optional, for running the YAML blueprint)

### Local Setup
1. Clone the repository:
   ```bash
   git clone https://github.com/sprillex/ha-automation-ideas.git
   cd ha-automation-ideas
   ```

2. No additional compilation or package installation is needed as this project uses native web standards and static files.

## Configuration

This project operates as a client-side static web application with no server-side environment variables or secrets required.

- **Static Asset Relative Fetching**: `index.html` loads relative static files (`recipes.json`). Ensure relative file paths are maintained when deploying or hosting under custom subpaths.
- **Upstream Repository Links**: Edit `recipes.json` or `blueprint_index/all.json` to configure blueprint URLs, category tags, and creator repository endpoints.

## Running the Application

### Development Server
You can launch a local web server using Python's built-in HTTP server or Node.js `npx http-server`:

Using Python:
```bash
python3 -m http.server 8000
```

Using Node.js / `http-server`:
```bash
npx http-server -p 8000
```

Access the application in your browser at `http://localhost:8000`.

### Production Deployment
Deploy the root directory to any static web host (e.g., GitHub Pages, Cloudflare Pages, Netlify, Vercel, or Nginx static server).

## Testing

### Validating JSON Schemas
Validate that catalog JSON files maintain proper JSON formatting before committing changes:

Using Python:
```bash
python3 -m json.tool recipes.json > /dev/null
python3 -m json.tool blueprint_index/all.json > /dev/null
```

Using Node.js:
```bash
node -e "JSON.parse(require('fs').readFileSync('recipes.json'))"
node -e "JSON.parse(require('fs').readFileSync('blueprint_index/all.json'))"
```

### HTML & Lint Checks
Ensure all HTML tags and JavaScript function references in `index.html` validate cleanly using standard linters (e.g., `htmlhint` or `eslint` if installed).

## API Reference

This repository exposes static JSON data endpoints, blueprint YAML specifications, and client-side querying interfaces for Home Assistant automation discovery.

For complete endpoint paths, data schemas, field validations, HTTP response status codes, JSON response examples, and Home Assistant event interface definitions, please refer to [API.md](./API.md).
