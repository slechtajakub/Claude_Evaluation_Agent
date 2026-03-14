# Evaluation Agent
_Automatizovaný import hodnocení žáků z PDF do Google Sheets_

## Problem Statement
Učitel ručně přepisu hodnocení ze skenů PDF (50+ žáků) do tabulky. Processuje se **~2 hodiny týdně**.

**Řešení:** OCR + automatické mapování jmen + zápis do Google Sheets API.

## Outcomes
- ⏱️ **2 hodiny → 5 minut** na evaluační cyklus
- 📊 **0 chyb** (automatické mapování podle jmen)
- 🔄 **Repeatable workflow** (jednou nastavit, běží neustále)

## How to Use (30 seconds)

1. Nahraj PDF (skoenovaný výstup žáka se jménem a hodnocením) do Claude Project chatu
2. Napiš: `LIST, SLOUPEC, CÍL`
   - Příklad: `Př 7.A, M, Savci cvičení`
3. Agent spustí OCR → parsuje → mapuje → zapisuje

**Output:** Import dat do Google Sheets, potvrzující zpráva s počtem zapsaných žáků

## Tech Stack
- **Frontend:** Claude Project (chat interface)
- **Backend:** Claude AI + MCP (google-sheets connector)
- **Data:** Google Sheets API
- **Pipeline:** OCR (PDF) → Text parsing → Row mapping → Batch write

## Key Features
✅ Automatické mapování jmen (bez manuálního výběru)  
✅ Batch write (efektivní API calls)  
✅ Struktura s řádky "finální/před opravou" (respektuje školský standard)  
✅ Validace mapování před zápisem (pokud jméno není v tabulce, alert)  
✅ Detailní output (koho zapsal, koho nenašel, co se stalo)  

## Real-World Context
- 🏫 ZŠ Labyrinth, třídy 7.A, .... (~150 žáků)
- 📝 Evaluace po testech, cvičeních, skupinových prací
- 🎯 Cílová tabulka: `"Hodnocení 2. pololetí 2025/26"`

## Next Steps
- [ ] Integrace s Edookit API (pokud existuje)
- [ ] Automatický weekly import (folder monitoring)
- [ ] Export do PDF reportu pro rodiče
- [ ] Histogram výsledků (quick stats)

---

**Autor:** Jakub Šlechta  
**Status:** Active (školní rok 2025/26)  
**Poslední aktualizace:** [14.3.2026]