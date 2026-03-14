# OpenClaw Sensor Tower Skill (sensor-tower)

A self-contained skill for OpenClaw using **YAML** Swagger definitions to analyze mobile market data.

---

## 📂 Directory Structure

```text
sensor-tower/
├── SKILL.md           # AI Logic (Updated for YAML)
├── README.md          # This documentation
├── config.json        # (Optional) Local API Key storage
└── swaggerdocs/       # (Required) YAML definitions
    ├── app_analysis.yml
    ├── market_analysis.yml
    ├── store_marketing.yml
    ├── custom_fields_metadata.yml
    └── your_metrics.yml
```

---

## 🚀 Setup Instructions

### 1. [Optional] Update Swagger Definitions

**Source**: Sensor Tower API Documentation (YAML format)

**Why optional?**: The skill already includes the Swagger definitions in `swaggerdocs/`. Only update if you need newer API specs or additional endpoints.

**Action**: Download the `.yaml` files from your Sensor Tower account dashboard.

**Placement**: Place them in `sensor-tower/swaggerdocs/`, replacing existing files if needed.

---

### 2. Configure API Key

Choose one method:

#### Method A: Environment Variable (Recommended)

Set the environment variable based on your operating system:

**Windows (Command Prompt)**:
```cmd
setx SENSOR_TOWER_AUTH_TOKEN "your_key_here"
```
Restart your terminal or command prompt after setting.

**Windows (PowerShell)**:
```powershell
$env:SENSOR_TOWER_AUTH_TOKEN="your_key_here"
```
Note: This is session-only. For permanent storage, use `setx` or add to PowerShell profile.

**macOS / Linux (Bash/Zsh)**:
```bash
export SENSOR_TOWER_AUTH_TOKEN="your_key_here"
```
To make it permanent, add the line to your shell profile:
- `~/.bashrc` or `~/.bash_profile` (Bash)
- `~/.zshrc` (Zsh)

**Cross-platform tip**: You can also set it in OpenClaw's system environment if running as a service.

#### Method B: Local `config.json`

Create a `config.json` file in the `sensor-tower/` skill directory with the following content:

```json
{
  "api_key": "your_key_here"
}
```

**File location**:
- The skill expects the file at: `sensor-tower/config.json` (relative to the skill directory)
- This file should **NOT** be committed to version control (it's in `.gitignore`).

**Create the file**:

<details>
<summary>Click for OS-specific instructions</summary>

**Windows (Command Prompt)**:
```cmd
echo {^"api_key^": ^"your_key_here^"} > C:\Users\wfansh\.openclaw\workspace\skills\sensor-tower\config.json
```

**Windows (PowerShell)**:
```powershell
'{"api_key": "your_key_here"}' | Out-File -FilePath "C:\Users\wfansh\.openclaw\workspace\skills\sensor-tower\config.json" -Encoding UTF8
```

**macOS / Linux (Bash)**:
```bash
echo '{"api_key": "your_key_here"}' > /path/to/sensor-tower/config.json
```

Replace the path with your actual skill location.

</details>

---

### 3. Usage

Restart OpenClaw and ask:

> "Based on `app_analysis.yml`, find the App ID for 'TikTok' on iOS."

Or:

> "Show me the top 10 grossing games in the US for yesterday."

---

## 🛠️ Technical Details

| Property | Value |
|----------|-------|
| **Base URL** | `https://api.sensortower.com` |
| **Definition Format** | OpenAPI / Swagger (YAML) |
| **Auth Method** | `auth_token` via Query String |
| **Rate Limit** | 6 requests/sec |
| **Platforms** | `ios`, `android`, `unified`, `both_stores` |

---

## 📖 API Coverage (50 Endpoints)

The skill covers 5 major Sensor Tower API products:

| Swagger File | API Product | Endpoints | Key Functions |
|--------------|-------------|-----------|---------------|
| `app_analysis.yml` | Usage Intelligence | 17 | App/Publisher metadata, custom fields, unified IDs |
| `market_analysis.yml` | Ad Intelligence | 4 | Rankings, download/revenue estimates, top apps |
| `store_marketing.yml` | Store Intelligence | 23 | Featured content, keywords, reviews, Search Ads |
| `custom_fields_metadata.yml` | Custom Fields Metadata | 1 | Custom field definitions |
| `your_metrics.yml` | App Teardown (My Apps) | 5 | Connected app analytics, API usage |

**Total**: 50 API endpoints across all categories.

---

## 🔐 Security Notes

- The skill reads the API key from `config.json` or environment variable.
- Never commit `config.json` with a real API key to version control.
- Add `config.json` to `.gitignore` if using git.

---

## 🧪 Example Queries

```
"Get the download estimates for app ID 6448786147 for the last 30 days."
"Search for apps with 'strategy' in the name on Android."
"Show me the top 5 keywords for Age of Origins in the US."
"Get the review summary for TikTok from last week."
```

---

## 📚 References

- [Sensor Tower API Docs](https://docs.sensortower.com/)
- [OpenClaw Skill Development Guide](https://docs.openclaw.ai/)

---

## 📄 License

This skill is provided as-is for use with OpenClaw. Please ensure you comply with Sensor Tower's Terms of Service.
