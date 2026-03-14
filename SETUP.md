# Setup Guide — Local MCP (Claude + Windows)

Toto je průvodce pro běh **Evaluation Agent** lokálně na Windows s Claude a MCP serverem.

---

## Předpoklady

- [ ] Claude nainstalován (`claude` command v terminálu)
- [ ] Windows (PowerShell nebo Command Prompt)
- [ ] Node.js 18+ (`node --version`)
- [ ] `uvx` nainstalován (Anthropic tool runner)
- [ ] Google Cloud Console přístup
- [ ] Git (pro klonování repo)

---

## Instalace (20–30 minut)

### ČÁST 1: Google Cloud Setup

#### Krok 1.1: Vytvoř projekt v Google Cloud Console

1. Jdi na [console.cloud.google.com](https://console.cloud.google.com)
2. Klikni na projekt dropdown (top left)
3. **"NEW PROJECT"**
   - Název: `Evaluation Agent`
   - Organization: ponechej default
4. Počkej 30 sekund, až se projekt vytvoří

#### Krok 1.2: Aktivuj Google Sheets a Google Drive APIs

1. V levém menu: **"APIs & Services"** → **"Library"**
2. Hledej a klikni na **"Google Sheets API"**
   - Klikni **"ENABLE"**
   - Vrať se do Library (back button)
3. Hledej a klikni na **"Google Drive API"**
   - Klikni **"ENABLE"**

#### Krok 1.3: Vytvoř Service Account

1. V levém menu: **"APIs & Services"** → **"Credentials"**
2. **"+ CREATE CREDENTIALS"** (top) → **"Service Account"**
3. Vyplň:
   - **Service account name:** `evaluation-agent`
   - Klikni **"CREATE AND CONTINUE"**
4. Grant roles (skip) — klikni **"CONTINUE"**
5. Klikni **"DONE"**

#### Krok 1.4: Stáhni key.json

1. V **"Credentials"**, vidíš seznam Service Accountů
2. Klikni na email Service Accountu (řádek s `evaluation-agent@...`)
3. Jdi na tab **"KEYS"**
4. **"ADD KEY"** → **"Create new key"** → **"JSON"**
5. Automaticky se stáhne soubor `evaluation-agent-XXXXX.json`
   - Přejmenuj na: `key.json`
   - Ulož do složky projektu: `C:\Users\[tvoje_jmeno]\Documents\AI\Claude-other\ClaudeEvaluatorMCP\key.json`
   
   ⚠️ **ZAPOMEŇ, ŽE EXISTUJE** — více info níže v security

#### Krok 1.5: Dej Service Accountu přístup k tabulce

1. Otevři svou Google Sheets tabulku
2. Klikni **"SHARE"** (top right)
3. Vlož email Service Accountu: `evaluation-agent-sa@evaluation-agent-XXXXX.iam.gserviceaccount.com`
   - (Najdeš ho v Cloud Console: Service Accounts → Email)
4. Zvol **"Editor"** (musí mít write access)
5. Klikni **"Share"**

---


#### Krok 2: Uprav `claude.developer-edit` config

1. V Claude → **"File"** → **"Preferences"** (nebo Ctrl+Shift+P)
2. Hledej **"Claude Config"** nebo vytvoř soubor `.claude-developer-edit`
3. Zkopíruj a vlož:

```json
{
  "mcpServers": {
    "google-sheets": {
      "command": "uvx",
      "args": [
        "mcp-google-sheets"
      ],
      "env": {
        "GOOGLE_APPLICATION_CREDENTIALS": "C:/Users/.../Documents/AI/Claude-other/ClaudeEvaluatorMCP/key.json"
      }
    }
  }
}
```

**UPRAV TYTO CESTY:**
- `"C:/Users/.../..."` → Tvá cesta (Windows style: forward slashes)
  - Zjistíš: `cd` v terminálu → `pwd` → zkopíruj cestu


### ČÁST 3: Claude Project Instructions

#### Krok 3.1: Zkopíruj Instructions do Claude

1. Otevři soubor `INSTRUCTIONS.md` (ze repo)
2. V Claude → **"New File"** → `system-instructions.md`
3. Zkopíruj obsah z `INSTRUCTIONS.md`
4. **UPRAV TYTO HODNOTY:**

```markdown
| Parametr | Hodnota |
|----------|---------|
| **Spreadsheet ID** | 1nzAAnnA1qadGPhQUuPfstscLjdEf-vqL3k5PSE78BNPkkDVFc |
← Vlož TVŮJ ID odtud
| **Tabulka** | "Projekt hodnocení 2. pololetí 2025/26" |
← Vlož TVŮJ sheet name
| **Email projekt** | ...@gmail.com |
← Vlož TVŮJ email
```

---

### ČÁST 4: Test

V Claude chatu napiš:

```
Ověř, že se můžeš připojit k tabulce pomocí MCP serveru. 
Vypsat seznam všech listů v tabulce "Projekt hodnocení 2. pololetí 2025/26".
```

**Očekávaný výstup:**
```
✓ Connected to Google Sheets
✓ Spreadsheet ID: 1nzAAnnA1sadasddsdqGPhQUuPtscL564jdEf-vqL3kdsdad5PSEBNPkkDVFc
✓ Sheets:
  - Př 7.A
  - Př 8.C
  - Př 9.B
```

---

**Autor:** Jakub Šlechta  
**Setup Type:** Local MCP (Claude + Windows)  
**Poslední aktualizace:** 2026
