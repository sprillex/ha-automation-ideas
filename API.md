# Home Assistant Community Automation Discovery Directory - API Reference

## Overview

This repository acts as a static RESTful data service and blueprint distribution endpoint for Home Assistant automation ideas.

- **Base URL**: `https://sprillex.github.io/ha-automation-ideas` (or local static server host, e.g., `http://localhost:8000`)
- **Protocols**: HTTP / HTTPS
- **Versioning**: Data format schemas follow semantic structure (`v1` implicit flat JSON array structure).

All endpoints return static resources over standard HTTP response codes without requiring server-side state processing or dynamic query evaluation.

---

## Authentication

All static JSON and YAML endpoints provided by this repository are public assets.

- **Auth Scheme**: None (Public Unauthenticated Access).
- **Headers**: No API keys, Bearer tokens, or session cookies are required.
- **CORS Policy**: Public cross-origin access (`Access-Control-Allow-Origin: *`) supported via static host settings (e.g., GitHub Pages).

---

## Standard Envelopes & Error Formats

### Success Response Envelope
All static endpoints return JSON arrays directly or standard YAML content types:
- **HTTP Status**: `200 OK`
- **Content-Type**: `application/json; charset=utf-8` (for JSON endpoints) or `text/yaml; charset=utf-8` / `text/plain` (for blueprint YAML endpoints).

### Standard Error Formats
Static file servers or CDN hosts return standard HTTP status codes in case of client or resource errors:

| Status Code | Reason | Description | Example Payload |
| :--- | :--- | :--- | :--- |
| **`404 Not Found`** | Resource Missing | Returned when requesting a file path or endpoint that does not exist on the server. | `{"error": "404 Not Found", "message": "The requested resource was not found on this server."}` |
| **`403 Forbidden`** | Access Restricted | Returned if file permission restrictions prevent reading static assets. | `{"error": "403 Forbidden", "message": "Access denied."}` |
| **`304 Not Modified`** | Cached Content | Returned when client conditional headers (`If-None-Match`, `If-Modified-Since`) match current static assets. | *Empty Body* |

---

## Endpoints by Resource

### 1. Automation Recipes Catalog

#### `GET /recipes.json`

Fetches the complete catalog of community automation recipe ideas. Used by the web interface (`index.html`) to populate discovery cards and filter options.

- **Description**: Public catalog of automation recipes detailing required hardware tags, descriptions, and optional repository links.
- **Permissions**: Public (no authorization required)
- **Headers**:
  - `Accept`: `application/json` (optional)
- **Parameters**: None (Static endpoint)

##### Request Example
```http
GET /recipes.json HTTP/1.1
Host: sprillex.github.io
Accept: application/json
```

##### Response Body Schema (`200 OK`)
An array of recipe objects with the following fields:

| Field | Type | Required | Description & Validations |
| :--- | :--- | :--- | :--- |
| `id` | `string` | **Yes** | Unique slug identifying the automation recipe (e.g., `motion_hallway_light`). |
| `title` | `string` | **Yes** | Descriptive title of the automation action. |
| `desc` | `string` | **Yes** | Short summary explaining triggers, conditions, and actions. |
| `requires` | `array[string]` | **Yes** | List of required device/hardware capability tags. Valid values: `motion`, `light`, `door`, `switch`, `media`, `climate`, `lock`, `cover`, `power`, `presence`, `sun`. |
| `repo_url` | `string` | No | Direct URL to upstream GitHub repository or project homepage. |
| `github_url` | `string` | No | Direct URL to raw blueprint file or specific YAML source file. |

##### Success Response Example (`200 OK`)
```json
[
  {
    "id": "motion_hallway_light",
    "title": "Motion-Activated Hallway Lighting",
    "desc": "Motion sensor detects movement -> Turn on pathway lights with auto-off after 5 min vacancy.",
    "requires": ["motion", "light"]
  },
  {
    "id": "welcome_home_exterior",
    "title": "Welcome Home Exterior Lighting",
    "desc": "First person arrives home after sunset -> Turn on porch and entryway fixtures.",
    "requires": ["presence", "light", "sun"],
    "repo_url": "https://github.com/sprillex/ha-automation-ideas"
  }
]
```

---

### 2. Community Blueprint Index

#### `GET /blueprint_index/all.json`

Fetches the curated index of popular Home Assistant community blueprints from external GitHub repositories.

- **Description**: Extended index mapping blueprints to categories, GitHub source URLs, and detailed descriptions.
- **Permissions**: Public (no authorization required)
- **Headers**:
  - `Accept`: `application/json` (optional)
- **Parameters**: None (Static endpoint)

##### Request Example
```http
GET /blueprint_index/all.json HTTP/1.1
Host: sprillex.github.io
Accept: application/json
```

##### Response Body Schema (`200 OK`)
An array of blueprint metadata objects:

| Field | Type | Required | Description & Validations |
| :--- | :--- | :--- | :--- |
| `id` | `string` | **Yes** | Unique identifier for the blueprint entry. |
| `name` | `string` | **Yes** | Display name of the blueprint project. |
| `github_url` | `string` | **Yes** | Direct URL to the raw `.yaml` blueprint file on GitHub. |
| `description` | `string` | **Yes** | Detailed description of the blueprint functionality. |
| `category` | `array[string]` | **Yes** | List of hardware capability tags (e.g., `["switch", "light"]`). |

##### Success Response Example (`200 OK`)
```json
[
  {
    "id": "yagrasdemond_yaman",
    "name": "YAMAN - Yet Another Motion Automation (N-th time)",
    "github_url": "https://github.com/Yagrasdemond/home-assistant-blueprints/blob/main/yaman.yaml",
    "description": "Comprehensive motion-activated lighting automation with sun conditions, brightness settings, off-delay, and manual override options.",
    "category": ["motion", "light"]
  },
  {
    "id": "epmatx_awesome_ha_blueprints_hook_light",
    "name": "Awesome HA Blueprints - Light Hook",
    "github_url": "https://github.com/EPMatt/awesome-ha-blueprints/blob/main/blueprints/controllers/hooks/light/light.yaml",
    "description": "Connects controllers (switches, remotes) to light entities with full dimming, color temperature, and scene toggle capabilities.",
    "category": ["switch", "light"]
  }
]
```

---

### 3. Home Assistant Blueprint File

#### `GET /blueprints/automation_suggester.yaml`

Retrieves the zero-configuration Home Assistant Auto-Audit Automation Suggester blueprint YAML file.

- **Description**: Blueprint source code importable into Home Assistant instances to audit local hardware entities against automation rules.
- **Permissions**: Public (no authorization required)
- **Headers**:
  - `Accept`: `text/yaml`, `text/plain`

##### Request Example
```http
GET /blueprints/automation_suggester.yaml HTTP/1.1
Host: sprillex.github.io
```

##### Success Response Example (`200 OK`)
```yaml
blueprint:
  name: "Community Auto-Audit Automation Suggester"
  description: "Audits your entire Home Assistant entity registry and creates a notification of compatible automation ideas."
  domain: automation
  input: {}

trigger:
  - trigger: homeassistant
    event: start
    id: startup
  - trigger: time
    at: "09:00:00"
    id: daily_check
  - trigger: event
    event_type: "generate_automation_ideas"
    id: manual_event
```

---

### 4. Home Assistant Internal Event & Action Interface

The auto-audit blueprint (`blueprints/automation_suggester.yaml`) interfaces with standard Home Assistant Core services and event buses.

#### Event Trigger: `generate_automation_ideas`
- **Event Bus Name**: `generate_automation_ideas`
- **Direction**: Inbound Trigger
- **Description**: Custom event payload fired within Home Assistant to manually force an auto-audit device scan.

##### Event Trigger Example
```json
{
  "event_type": "generate_automation_ideas",
  "data": {}
}
```

#### Action Call: `persistent_notification.create`
- **Service Name**: `persistent_notification.create`
- **Direction**: Outbound Action
- **Description**: Dispatches notification cards to Home Assistant UI populated with catalog rule matches.

##### Action Service Data Schema
```json
{
  "title": "Automation Suggestions for Your Home",
  "notification_id": "auto_device_suggester",
  "message": "Home Assistant scanned your devices and found **20 compatible automation ideas**:\n\n- **Motion-Activated Hallway Lighting**\n  Motion sensor trips -> Turn on pathway lights with auto-off after 5 min."
}
```

---

## Pagination & Querying

Because datasets in `recipes.json` and `blueprint_index/all.json` are flat arrays, filtering, searching, and URL derivation are performed client-side in `index.html`.

### Client-Side Hardware Tag Filtering
Hardware capability filters evaluate `requires` or `category` tag arrays. A recipe entry is displayed only if **all** of its required tags match the active hardware filters.

Valid Tag Keywords:
- `motion`: Motion sensors / Binary occupancy sensors
- `light`: Dimmers, bulbs, RGB fixtures
- `door`: Contact sensors, doors, windows, garage doors
- `switch`: Smart plugs, relays, wall switches
- `media`: Media players, TVs, speakers
- `climate`: Thermostats, HVAC units, AC controls
- `lock`: Smart door locks & deadbolts
- `cover`: Motorized blinds, shades, curtains
- `power`: Smart plug energy/wattage monitors
- `presence`: Person entities, device trackers
- `sun`: Solar elevation & sun position sensors

### Client-Side Search Querying
Search query strings from the input bar (`#search-input`) perform case-insensitive substring matches against recipe fields:
- `title` / `name`
- `desc` / `description`

### Repository URL Resolution Logic
When rendering cards, repository and blueprint file URLs are dynamically resolved using the following priority:

```javascript
function deriveRepoUrl(item) {
  if (item.repo_url) return item.repo_url;
  if (item.github_url) {
    const match = item.github_url.match(/https:\/\/github\.com\/[^\/]+\/[^\/]+/);
    if (match) return match[0];
  }
  return "https://github.com/sprillex/ha-automation-ideas";
}

function getFileUrl(item) {
  if (item.github_url) return item.github_url;
  if (item.repo_url) return item.repo_url;
  return "https://github.com/sprillex/ha-automation-ideas/blob/main/recipes.json";
}
```
