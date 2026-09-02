# Home Assistant Community Automation Discovery Directory

A zero-YAML, community-driven discovery catalog that points Home Assistant users directly to upstream community automation repositories based on installed device hardware.

---

## 🚀 One-Click Auto-Audit Blueprint Import

Click the badge below to import the zero-configuration auto-audit blueprint into your Home Assistant instance:

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fsprillex%2Fha-automation-ideas%2Fblob%2Fmain%2Fblueprints%2Fautomation_suggester.yaml)

### Manual UI Import Instructions
1. In Home Assistant, navigate to **Settings** > **Automations & Scenes** > **Blueprints**.
2. Click **Import Blueprint** in the bottom right corner.
3. Paste the blueprint URL:
   `https://github.com/sprillex/ha-automation-ideas/blob/main/blueprints/automation_suggester.yaml`
4. Click **Preview** and then **Import**.

---

## 💡 About the Discovery Directory

This project acts as an interactive directory for discovering local, zero-AI automation ideas and community-maintained Home Assistant blueprints:

1. **Interactive Dashboard Browser:** Embed `https://sprillex.github.io/ha-automation-ideas/` into your Home Assistant dashboard via a **Webpage Card** to filter automation ideas by installed hardware (Motion, Lights, Climate, Locks, Covers, Power, etc.) and keyword search.
2. **Direct Upstream Linking:** Each card features a **View Repository** button that links directly to the creator's upstream GitHub project repository so you can review documentation, source code, and installation instructions before importing.

---

## 🤝 How Community Authors Can Feature Their Repositories

We encourage community authors to list their automation blueprints and repositories in our catalog!

1. Open [`recipes.json`](recipes.json) on GitHub.
2. Click the pencil icon (**Edit this file**).
3. Add your project entry using this JSON format:
   ```json
   {
     "id": "my_unique_automation_id",
     "title": "Clear Automation Title",
     "desc": "Short description of triggers and actions.",
     "requires": ["motion", "light"],
     "repo_url": "https://github.com/username/repository-name"
   }
   ```
   *Valid `requires` tags:* `motion`, `light`, `door`, `switch`, `media`, `climate`, `lock`, `cover`, `power`, `presence`, `sun`.
4. Submit a Pull Request. Once merged, your project will immediately feature in the web discovery catalog!
