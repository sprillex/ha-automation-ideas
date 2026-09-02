# Home Assistant Community Automation Ideas & Suggester

A community-driven, local, zero-AI catalog of automation recipes for Home Assistant.

## 💡 How to Add New Automation Ideas

We welcome community contributions! You do not need to write blueprints or automation scripts:

1. Open [`recipes.json`](recipes.json).
2. Click the pencil icon (Edit this file) on GitHub.
3. Add your idea using this format:
   ```json
   {
     "id": "unique_idea_name",
     "title": "Clear Idea Title",
     "desc": "Trigger condition -> Resulting action.",
     "requires": ["motion", "light"]
   }
   ```
Valid `requires` tags: `motion`, `light`, `door`, `switch`, `media`, `climate`, `lock`, `cover`, `power`, `presence`, `sun`.

4. Submit a Pull Request. Once merged, your idea immediately appears in the web explorer!

---

## 🚀 Usage in Home Assistant

### 1. View Interactive Catalog on Your Dashboard (Zero YAML)

1. In Home Assistant, click the three dots in top right > **Edit Dashboard**.
2. Click **Add Card** > **Webpage**.
3. Set the URL to: `https://sprillex.github.io/ha-automation-ideas/`

---

### 2. Auto-Audit Blueprint (Zero Input Configuration)

Click the badge below to import the auto-audit blueprint template directly into your Home Assistant:

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fsprillex%2Fha-automation-ideas%2Fblob%2Fmain%2Fblueprints%2Fautomation_suggester.yaml)

#### Manual UI Import
1. In Home Assistant, go to **Settings** > **Automations & Scenes** > **Blueprints**.
2. Click **Import Blueprint** (bottom right).
3. Paste:
   `https://github.com/sprillex/ha-automation-ideas/blob/main/blueprints/automation_suggester.yaml`
4. Click **Preview** and then **Import**.
