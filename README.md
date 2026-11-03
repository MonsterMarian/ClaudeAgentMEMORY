# ClaudeAgentMEMORY

Paměť projektu pro **Claude Code** mezi sessions — bez `/resume`.

## K čemu to je

`/resume` stará session nese celý předchozí kontext a žere tokeny. Levnější je začínat
každou práci **novou session**. Jenže nová session neví, co se v projektu dělo.

Tohle řešení to spojí: v kořeni každého projektu vzniká malý soubor **`AgentMEMORY.md`**
(cíl repa, stav, hotovo, rozpracováno, rozhodnutí, log s datem a časem). Claude Code ho
na začátku session přečte a po každé odpovědi aktualizuje. Soubor se udržuje malý
(pod ~400 řádků) — agent mazání a slučování dělá sám.

## Jak to funguje

Hook `UserPromptSubmit` v Claude Code vloží krátké pravidlo
([`agent-memory-pravidlo.md`](agent-memory-pravidlo.md), ~20 řádků) do kontextu
**u každého promptu**. Proto na pravidlo agent nezapomene ani v dlouhé session
(`CLAUDE.md` se načítá jen na začátku). Podrobná pravidla a šablona: [`PRAVIDLA.md`](PRAVIDLA.md).

Platí globálně — pro všechny projekty, ve kterých Claude Code spustíš.

## Nastavení

**1. Zkopíruj pravidlo do `~/.claude/`**

Windows (PowerShell):
```powershell
git clone https://github.com/MonsterMarian/ClaudeAgentMEMORY
Copy-Item ClaudeAgentMEMORY\agent-memory-pravidlo.md $HOME\.claude\
```

macOS / Linux:
```bash
git clone https://github.com/MonsterMarian/ClaudeAgentMEMORY
cp ClaudeAgentMEMORY/agent-memory-pravidlo.md ~/.claude/
```

**2. Přidej hook do `~/.claude/settings.json`**

Do souboru přidej klíč `hooks` (když už nějaké hooky máš, jen přidej `UserPromptSubmit`
do existujícího objektu — nic nepřepisuj):

```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "[ -n \"$AGENT_MEMORY_OFF\" ] || cat ~/.claude/agent-memory-pravidlo.md",
            "timeout": 10
          }
        ]
      }
    ]
  }
}
```

Příkaz je pro bash (Linux, macOS; na Windows Claude Code spouští hooky přes Git Bash, když je
nainstalovaný). Bez Git Bashe běží hook v PowerShellu — tam použij
`"command": "if (-not $env:AGENT_MEMORY_OFF) { Get-Content -Encoding UTF8 ~/.claude/agent-memory-pravidlo.md }"`
(Windows PowerShell 5.1 by bez `-Encoding UTF8` rozbil češtinu).

**3. Ověř**

Spusť novou session Claude Code a napiš libovolný prompt. V přepisu uvidíš
`UserPromptSubmit hook success` s textem pravidla a po odpovědi se v kořeni projektu
objeví `AgentMEMORY.md`. Když ne, otevři v Claude Code `/hooks` (načte konfiguraci znovu)
nebo session restartuj.

## Úpravy

- **Pravidlo** měníš přímo v `~/.claude/agent-memory-pravidlo.md` — změna platí od dalšího promptu.
  Drž ho krátké: posílá se s každým promptem a stojí tokeny.
- **Výjimka pro složku** (např. pracovní repa, kam se soubor nesmí commitovat): přidej
  do pravidla řádek typu
  `V <cesta> AgentMEMORY.md necommituj — přidej ho do .git/info/exclude.`
- **Vypnutí pro jedno spuštění:** nastav proměnnou `AGENT_MEMORY_OFF=1`. Hodí se pro skripty
  a pomocná volání `claude -p` (rešerše, kontrola, práce v dočasné složce) — jinak by i ta
  dostala pravidlo a četla nebo zakládala `AgentMEMORY.md` zbytečně.
  Bash: `AGENT_MEMORY_OFF=1 claude -p "…"`, PowerShell: `$env:AGENT_MEMORY_OFF=1; claude -p "…"`.
- **Vypnutí úplně:** smaž hook ze `settings.json` (nebo ho vypni přes `/hooks`).

## Omezení

- Je to pokyn, ne vynucení — agent ho dodrží skoro vždy, ale ne zaručeně.
- Pravidlo (~20 řádků) jde do kontextu u každého promptu.
- Funguje jen v Claude Code (jiní agenti hooky Claude Code nečtou).
- Hook dostává jen hlavní session, ne subagenti — paměť po nich zapisuje hlavní agent.
